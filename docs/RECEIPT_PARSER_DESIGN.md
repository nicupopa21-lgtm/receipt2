# Receipt Parser Design Document

## Product goal

Build an offline-first receipt parser and pantry assistant for Android.

The app should support physical receipts, screenshots, PDFs, and online receipts. It should extract line items, classify them as grocery, non-grocery, ignore, or unknown, and create pantry entries from grocery items.

## Target markets

- Europe
- United States
- Indonesia

## Main languages

- English
- German
- Romanian
- Indonesian

## Processing pipeline

```text
Input source
  → image/PDF preparation
  → local OCR
  → OCR normalization
  → receipt metadata parser
  → line-item parser
  → item classifier
  → pantry conversion
  → review/correction UI
  → local learning
```

## Input types

### Physical receipts

Preferred implementation:

- Android document scanner flow if available.
- Store scanned image locally or as app-private file.
- Run OCR locally.

### Images/screenshots

- Android Photo Picker.
- No broad gallery permission.
- Treat screenshot as normal OCR input.

### PDFs

- Storage Access Framework.
- First pass may render pages to images.
- Later optimization: extract embedded text directly if available.

### Online receipts

MVP:

- User imports screenshot/PDF manually.

Later:

- Android share target.
- Email import only after privacy review.

## OCR layer

Create an `OcrService` abstraction so the parser does not depend directly on ML Kit classes.

Recommended internal model:

```kotlin
data class OcrDocument(
    val fullText: String,
    val lines: List<OcrLine>,
    val sourceWidth: Int?,
    val sourceHeight: Int?,
    val confidence: Float?
)

data class OcrLine(
    val text: String,
    val boundingBox: Rect?,
    val lineIndex: Int,
    val confidence: Float?
)
```

## Parser layer

The parser should be deterministic first.

Parser stages:

1. Normalize text.
2. Detect merchant/date/currency/total.
3. Remove obvious non-item lines.
4. Reconstruct item lines.
5. Extract name, quantity, unit, unit price, and line total.
6. Validate item sum against total.
7. Mark uncertain lines for review.

## Classification layer

Classification order:

1. Exact alias match.
2. Normalized alias match.
3. Merchant-specific alias match.
4. Keyword/category rule.
5. Fuzzy match.
6. Unknown.

## Pantry conversion

Only add to pantry when:

```text
itemType = grocery
isPantryEligible = true
confidence >= threshold
not ignored
```

Low-confidence groceries should appear in review before being added.

## User correction learning

Every correction should optionally create an alias:

```text
raw receipt item → canonical item
```

Examples:

```text
MLCH 1.5 → milk
SUSU UHT 1L → milk
LAPTE 3.5 → milk
BANANE KG → banana
```

User corrections must be stored separately from seed data so seed updates never wipe learned behavior.

## Architecture rules

- OCR SDK objects must not leak into parser layer.
- Parser logic must not live inside UI composables.
- Database entities should not be the only UI state model.
- Seed data and user corrections must be separate.
- Cloud features must be disabled by default.
