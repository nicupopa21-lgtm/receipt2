# Receipt Parser + Pantry App

Offline-first Android app concept for scanning physical receipts, screenshots, PDFs, and online receipts, extracting line items, classifying grocery vs non-grocery, and creating/updating a pantry list.

## Target regions

- Europe
- United States
- Indonesia

## Main languages

- English
- German
- Romanian
- Indonesian

## Core principle

The app should work locally first:

```text
receipt/photo/PDF
→ OCR
→ receipt parser
→ grocery/non-grocery classifier
→ pantry creation
→ user correction learning
```

Cloud OCR, AI parsing, barcode enrichment, accounts, analytics, ads, and payments are deferred until the base parser is stable and privacy implications are reviewed.

## Main documents

- `docs/DEVELOPMENT_PROGRESS_TRACKER.md`
- `docs/RECEIPT_PARSER_DESIGN.md`
- `docs/LOCAL_DATABASE_SCHEMA.md`
- `docs/SEED_COMPARISON_DATABASE.md`
- `tests/PARSER_TEST_CASES.md`
- `docs/PRIVACY_AND_PLAY_POLICY_NOTES.md`
- `prompts/CODEX_PROMPT.md`

## MVP target

- Scan/import receipt.
- Run local OCR.
- Extract receipt metadata.
- Extract item lines.
- Classify grocery/non-grocery/ignore/unknown.
- Add grocery items to pantry.
- Let user correct mistakes.
- Learn corrections locally.
- Let user delete/export local data.
