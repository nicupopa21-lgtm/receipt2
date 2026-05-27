# Codex Working Prompt

Use this prompt to continue implementation:

```text
Track and maintain docs/DEVELOPMENT_PROGRESS_TRACKER.md, docs/RECEIPT_PARSER_DESIGN.md, tests/PARSER_TEST_CASES.md, docs/LOCAL_DATABASE_SCHEMA.md, docs/SEED_COMPARISON_DATABASE.md, and docs/PRIVACY_AND_PLAY_POLICY_NOTES.md.

Continue implementing the receipt parser + pantry app according to the development tracker.

In this pass:
1. Read the tracker and design files first.
2. Identify the highest-impact next 5 concrete tasks.
3. Implement only those tasks.
4. Preserve offline-first architecture.
5. Keep parser, OCR, database, and UI layers separated.
6. Add or update tests where useful.
7. Update progress percentages and notes.
8. Do not add cloud, ads, payments, accounts, analytics, or email import unless explicitly requested.
9. Do not rewrite unrelated working code.
10. Stop and mark BLOCKED if required files, SDK setup, permissions, or project structure are missing.

Return:
- files changed
- what was implemented
- what was tested
- remaining tasks
- blockers
- next recommended pass
```
