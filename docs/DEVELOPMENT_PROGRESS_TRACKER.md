# Receipt Parser + Pantry App — Development Attack Plan & Progress Tracker

Last updated: 2026-05-27

Project goal: Build an Android app that scans physical receipts, screenshots, PDFs, and online receipts, extracts line items, classifies grocery vs non-grocery, and creates/updates a pantry list.

Target regions:
- Europe
- United States
- Indonesia

Main languages:
- English
- German
- Romanian
- Indonesian

Core product principle:
- Offline-first
- Local database first
- User correction learning
- Cloud/AI only as optional fallback later

---

## Status Legend

```text
[TODO]      Not started
[ACTIVE]    Currently being implemented
[BLOCKED]   Cannot continue until blocker is solved
[REVIEW]    Implemented but needs manual/QA review
[DONE]      Implemented and accepted
[DEFERRED]  Deliberately postponed
```

Progress scoring:

```text
0%   Not started
25%  Basic structure exists
50%  Main functionality works in simple cases
75%  Works in realistic cases, needs polish
90%  Feature-complete, needs tests/review
100% Accepted
```

---

## Phase Overview

| Phase | Name | Status | Progress | Notes |
|---|---|---:|---:|---|
| 0 | Project Setup | TODO | 0% | Create clean Kotlin/Compose project |
| 1 | Local Database Design | TODO | 0% | Room schema first |
| 2 | Seed Comparison Database | TODO | 0% | Start small, grow by corrections |
| 3 | Receipt Input | TODO | 0% | Camera, image, PDF |
| 4 | OCR Layer | TODO | 0% | Local OCR abstraction |
| 5 | Metadata Parser | TODO | 0% | Merchant/date/total/currency |
| 6 | Line Item Parser | TODO | 0% | Main hard parser |
| 7 | Grocery Classification | TODO | 0% | Alias/rules/fuzzy matching |
| 8 | Pantry Creation | TODO | 0% | Grocery to pantry |
| 9 | Review & Correction UX | TODO | 0% | User correction learning |
| 10 | Testing Dataset | TODO | 0% | Multilingual parser tests |
| 11 | Privacy / Play Readiness | TODO | 0% | Local-first, delete/export |
| 12 | Advanced Features | DEFERRED | 0% | Barcode, AI, cloud, smart pantry |

---

# PHASE 0 — Project Setup

## Goal

Create a clean Android project structure that can grow without becoming messy.

## Tasks

- [ ] Create Android project using Kotlin.
- [ ] Use Jetpack Compose for UI.
- [ ] Add basic app theme.
- [ ] Add navigation structure.
- [ ] Add package/module structure.
- [ ] Add tracker documents.
- [ ] Ensure parser logic is not placed directly in UI files.

## Suggested package structure

```text
app/src/main/java/.../
  scan/
  ocr/
  parser/
  classification/
  pantry/
  review/
  database/
  model/
  settings/
  privacy/
```

## Acceptance criteria

- [ ] Project builds successfully.
- [ ] App launches to home screen.
- [ ] Navigation shell exists.
- [ ] Tracker files exist.
- [ ] No parser logic placed directly in UI files.

---

# PHASE 1 — Local Database Design

## Goal

Create the local database that stores receipts, parsed items, pantry entries, aliases, and user corrections.

## Required tables

- `receipts`
- `receipt_items`
- `canonical_items`
- `item_aliases`
- `category_rules`
- `pantry_items`
- `parse_corrections`

## Tasks

- [ ] Define all Room entities.
- [ ] Define DAOs.
- [ ] Define migrations.
- [ ] Add seed database loader.
- [ ] Add local repository layer.
- [ ] Add export/import backup later.

## Acceptance criteria

- [ ] Database creates successfully.
- [ ] Sample seed items can be inserted.
- [ ] Receipts and receipt items can be saved.
- [ ] Pantry entries can be created from receipt items.
- [ ] User correction can create/update alias.

---

# PHASE 2 — Seed Comparison Database

## Goal

Create the first comparison database used to classify receipt text.

## Minimum seed size

```text
100 common groceries
50 common non-groceries
50 common ignore/payment/tax/receipt terms
4 language aliases per major item where possible
```

## Core categories

```text
fresh_produce
dairy
meat_fish
frozen
bakery
grains_pasta_rice
canned_jarred
spices_condiments
snacks
drinks
baby_food
pet_food
household_non_food
personal_care_non_food
medicine_health
other
unknown
```

## Tasks

- [ ] Create seed JSON or CSV format.
- [ ] Add seed loader into Room database.
- [ ] Add version number to seed data.
- [ ] Add alias matching service.
- [ ] Add fuzzy matching service.
- [ ] Add user correction learning.
- [ ] Add merchant-specific alias support.

## Acceptance criteria

- [ ] App can identify at least 100 common grocery aliases.
- [ ] App can identify obvious non-food items.
- [ ] App ignores tax/payment/footer lines.
- [ ] User correction creates reusable alias.
- [ ] Seed data can be updated without wiping user corrections.

---

# PHASE 3 — Receipt Input

## Goal

Allow receipts to enter the app from camera scan, image, screenshot, or PDF.

## Input types

### Camera scan

- [ ] Add receipt scan screen.
- [ ] Use document scanning flow where possible.
- [ ] Save scanned image locally.
- [ ] Show scan preview.
- [ ] Allow retake.

### Image import

- [ ] Add image import from gallery/photo picker.
- [ ] Support screenshot receipts.
- [ ] Save imported image reference or local copy.

### PDF import

- [ ] Add document picker.
- [ ] Accept PDF files.
- [ ] Convert PDF page to image or extract embedded text where possible.
- [ ] Support multi-page receipt PDFs later.

## Acceptance criteria

- [ ] User can scan physical receipt.
- [ ] User can import screenshot.
- [ ] User can import PDF.
- [ ] Input source is stored with receipt record.
- [ ] App does not request unnecessary storage permissions.

---

# PHASE 4 — OCR Layer

## Goal

Convert receipt image/PDF input into structured OCR text.

## Internal model

```text
OcrDocument
  fullText
  blocks
  lines
  words
  boundingBoxes
  confidence
  sourceImageWidth
  sourceImageHeight
```

## Tasks

- [ ] Add OCR service interface.
- [ ] Add ML Kit OCR implementation.
- [ ] Keep OCR layer independent from parser.
- [ ] Store raw OCR text.
- [ ] Store structured OCR lines if needed.
- [ ] Add OCR failure handling.
- [ ] Add image quality warning.
- [ ] Add retry scan option.

## Acceptance criteria

- [ ] OCR runs locally.
- [ ] Raw OCR text appears in debug/review screen.
- [ ] OCR result is saved with receipt.
- [ ] Parser does not directly depend on OCR SDK classes.
- [ ] Bad image shows useful error/retry message.

---

# PHASE 5 — Receipt Metadata Parser

## Goal

Extract merchant, date, currency, totals, tax, discount, deposit, and payment hints.

## Regional keywords

English / US:

```text
subtotal, tax, total, cash, card, change, debit, credit, visa, mastercard
```

German:

```text
summe, gesamt, zwischensumme, mwst, ust, bar, karte, rückgeld, pfand, rabatt
```

Romanian:

```text
total, subtotal, tva, bon fiscal, numerar, card, rest, reducere, lei, ron
```

Indonesian:

```text
total, subtotal, ppn, tunai, kartu, kembali, diskon, rp, harga, jumlah
```

## Tasks

- [ ] Add language/country hint detector.
- [ ] Add currency detector.
- [ ] Add date parser.
- [ ] Add total parser.
- [ ] Add tax parser.
- [ ] Add payment method parser.
- [ ] Add deposit/discount parser.
- [ ] Add receipt footer/header filtering.

## Acceptance criteria

- [ ] Detects total on simple receipts.
- [ ] Detects EUR, RON, USD, IDR.
- [ ] Detects common date formats.
- [ ] Detects tax lines separately from item lines.
- [ ] Does not treat total/payment lines as pantry items.

---

# PHASE 6 — Line Item Parser

## Goal

Turn OCR lines into actual purchased items.

## Common patterns

```text
ITEM NAME                          1.99
ITEM NAME                         1,99
2 x ITEM NAME                      3.98
ITEM NAME 2 x 1.99                3.98
BANANAS 0.824 kg x 1.79/kg        1.47
SUSU UHT 1L                       18000
INDOMIE GORENG 2 PCS              7000
```

## Tasks

- [ ] Normalize OCR characters.
- [ ] Join broken item lines.
- [ ] Split item name from price.
- [ ] Parse decimal comma and decimal dot.
- [ ] Parse Indonesian thousand separators.
- [ ] Parse weighted produce.
- [ ] Parse quantity multipliers.
- [ ] Parse discounts.
- [ ] Parse bottle deposit/Pfand separately.
- [ ] Detect non-item lines.
- [ ] Add confidence scoring.

## Acceptance criteria

- [ ] Correctly parses simple item-price lines.
- [ ] Correctly handles German decimal comma.
- [ ] Correctly handles US decimal point.
- [ ] Correctly handles Indonesian Rp amounts.
- [ ] Weighted produce is parsed with quantity/unit.
- [ ] Parser marks uncertain lines for review.

---

# PHASE 7 — Grocery Classification

## Classification pipeline

```text
1. Exact alias match
2. Normalized alias match
3. Merchant-specific alias match
4. Keyword/category rule match
5. Fuzzy match
6. Optional AI fallback later
7. Unknown
```

## Tasks

- [ ] Add ItemClassifier interface.
- [ ] Add exact alias classifier.
- [ ] Add keyword classifier.
- [ ] Add fuzzy classifier.
- [ ] Add grocery/non-grocery/ignore categories.
- [ ] Add confidence scoring.
- [ ] Add unknown fallback.
- [ ] Add correction learning.

## Acceptance criteria

- [ ] Common groceries classify correctly.
- [ ] Common household items classify as non-grocery.
- [ ] Tax/payment lines classify as ignore.
- [ ] Unknown items are not automatically added to pantry.
- [ ] Corrections improve future classifications.

---

# PHASE 8 — Pantry Creation

## Goal

Convert grocery receipt items into pantry entries.

## Pantry conversion rules

Only add to pantry if:

```text
itemType = grocery
isPantryEligible = true
confidence >= configured threshold
not marked ignore
not marked prepared/restaurant-only
```

## Simple expiry defaults

```text
fresh_produce: 5 days
dairy: 7 days
meat_fish: 2 days
bakery: 4 days
frozen: 90 days
grains_pasta_rice: 365 days
canned_jarred: 365 days
spices_condiments: 365 days
snacks: 180 days
drinks: 180 days
unknown: no expiry estimate
```

## Acceptance criteria

- [ ] Grocery receipt items create pantry entries.
- [ ] Non-grocery items do not create pantry entries.
- [ ] Unknown items require review.
- [ ] User can edit pantry item name/category/quantity.
- [ ] Pantry item links back to source receipt.

---

# PHASE 9 — Review & Correction UX

## Required screens

- Receipt review screen
- Raw OCR screen
- Correction dialog
- Pantry review screen

## Tasks

- [ ] Build receipt review UI.
- [ ] Build pantry review UI.
- [ ] Build correction dialog.
- [ ] Add confidence badges.
- [ ] Add warning badges.
- [ ] Add accept all safe items button.
- [ ] Add manual item add button.
- [ ] Add undo last correction.

## Acceptance criteria

- [ ] User can correct every parser mistake manually.
- [ ] Correction creates an alias.
- [ ] Alias is used on next scan.
- [ ] Low-confidence items are clearly visible.
- [ ] User is never forced to accept wrong pantry entries.

---

# PHASE 10 — Testing Dataset

## Required test groups

```text
English US grocery receipt
English online receipt
German Lidl/Aldi/Kaufland-style receipt
Romanian supermarket receipt
Indonesian minimarket receipt
Mixed grocery + household receipt
Receipt with discounts
Receipt with deposits/Pfand
Receipt with weighted produce
Receipt with OCR errors
Long receipt
Bad photo receipt
PDF receipt
Screenshot receipt
```

## Acceptance criteria

- [ ] Parser tests run automatically.
- [ ] Test data covers all four languages.
- [ ] Test data covers Europe, US, Indonesia.
- [ ] New parser changes do not break old receipts silently.
- [ ] Failing parser cases are tracked.

---

# PHASE 11 — Privacy, Security, and Play Store Readiness

## MVP privacy rules

```text
no account required
no cloud upload by default
no receipt sharing by default
no unnecessary permissions
local-only receipt storage
user can delete receipts
user can export data
```

## Acceptance criteria

- [ ] User can delete all local data.
- [ ] App functions without account.
- [ ] App functions without cloud.
- [ ] Permissions are minimal.
- [ ] Privacy policy matches actual behavior.
- [ ] No hidden data transmission.

---

# PHASE 12 — Optional Advanced Features

Status: [DEFERRED]

Candidates:

- Barcode scanning
- Open Food Facts lookup
- USDA FoodData Central lookup
- Cloud parser fallback
- LLM strict JSON parser
- Expiry reminders
- Shopping list prediction
- Email receipt import
- Share-to-app import

---

# Suggested First 10 Implementation Passes

## Pass 1 — Project skeleton and tracker files

- [ ] Create module/package structure.
- [ ] Add home screen.
- [ ] Add empty screens: Scan, Receipts, Pantry, Settings.
- [ ] Add tracker files.
- [ ] Add basic architecture notes.

## Pass 2 — Room database schema

- [ ] Add ReceiptEntity.
- [ ] Add ReceiptItemEntity.
- [ ] Add CanonicalItemEntity.
- [ ] Add ItemAliasEntity.
- [ ] Add PantryItemEntity.
- [ ] Add DAOs.
- [ ] Add repository layer.

## Pass 3 — Seed comparison database

- [ ] Create seed JSON.
- [ ] Add seed loader.
- [ ] Add 100 common groceries.
- [ ] Add 50 non-groceries.
- [ ] Add ignore terms.
- [ ] Add seed versioning.

## Pass 4 — Input import

- [ ] Add image picker.
- [ ] Add PDF picker.
- [ ] Store selected file source.
- [ ] Show preview.
- [ ] Save input metadata.

## Pass 5 — OCR abstraction

- [ ] Add OcrService interface.
- [ ] Add ML Kit implementation.
- [ ] Add OcrDocument model.
- [ ] Add raw OCR screen.
- [ ] Store OCR text in receipt.

## Pass 6 — Metadata parser

- [ ] Add ReceiptMetadataParser.
- [ ] Add currency rules.
- [ ] Add date rules.
- [ ] Add total/tax rules.
- [ ] Add tests.

## Pass 7 — Line item parser

- [ ] Add line cleanup.
- [ ] Add item-price parser.
- [ ] Add quantity parser.
- [ ] Add decimal comma/dot handling.
- [ ] Add IDR amount handling.
- [ ] Add tests.

## Pass 8 — Classifier

- [ ] Add ItemClassifier.
- [ ] Add exact alias match.
- [ ] Add keyword match.
- [ ] Add fuzzy match.
- [ ] Add confidence score.
- [ ] Add tests.

## Pass 9 — Pantry creation

- [ ] Add PantryCreationService.
- [ ] Add duplicate merge logic.
- [ ] Add expiry estimates.
- [ ] Add pantry list screen.
- [ ] Add edit/delete.

## Pass 10 — Review and learning

- [ ] Add receipt review screen.
- [ ] Add correction dialog.
- [ ] Add create alias from correction.
- [ ] Add confidence badges.
- [ ] Add unknown item review.

---

# Quality Rules

## Architecture

- Parser logic must not live inside UI composables.
- OCR SDK objects must not leak into parser layer.
- Database entities should not be used directly as UI state everywhere.
- Use repository/service boundaries.
- Keep seed data separate from user corrections.
- Never wipe user corrections when updating seed data.

## Parser

- Never auto-add low-confidence unknown items to pantry.
- Always keep raw OCR text for review.
- Always link pantry item back to source receipt item.
- Always allow manual correction.
- Always remember useful corrections.

## Privacy

- Do not upload receipts by default.
- Do not add analytics in MVP.
- Do not add ads SDK in MVP.
- User must be able to delete receipt data.
- User must be able to export data.
- Privacy policy must match real app behavior.

## Testing

- Every parser bug should become a regression test.
- Test all four target languages.
- Test decimal comma, decimal dot, and IDR formatting.
- Test discounts, deposits, tax, and total lines.
- Test bad OCR examples.

---

# Minimum Launch Standard

- [ ] App can scan/import receipt.
- [ ] OCR works on-device.
- [ ] Receipt review screen exists.
- [ ] User can correct parsed items.
- [ ] Pantry list works.
- [ ] User can delete all local data.
- [ ] Privacy policy exists.
- [ ] Data Safety answers match actual behavior.
- [ ] No hidden cloud upload.
- [ ] No unnecessary permissions.
- [ ] Parser does not silently create many wrong pantry items.
