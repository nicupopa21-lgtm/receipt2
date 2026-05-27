# Seed Data Directory

This directory is reserved for local seed data used by the receipt parser.

Planned files:

```text
canonical_items.json
item_aliases_en.json
item_aliases_de.json
item_aliases_ro.json
item_aliases_id.json
ignore_terms.json
category_rules.json
```

Rules:

- Keep seed data separate from user corrections.
- Seed updates must never wipe learned aliases.
- Use stable canonical IDs, for example `milk`, `rice`, `banana`, `detergent`.
- Support English, German, Romanian, and Indonesian aliases.
