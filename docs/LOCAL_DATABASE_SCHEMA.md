# Local Database Schema

Use Room/SQLite.

## receipts

Stores one scanned/imported receipt.

```text
id: String
merchantName: String?
merchantAddress: String?
countryCode: String?
languageCode: String?
currencyCode: String?
receiptDate: LocalDate?
receiptTime: LocalTime?
subtotal: Decimal?
taxTotal: Decimal?
discountTotal: Decimal?
depositTotal: Decimal?
total: Decimal?
paymentMethod: String?
sourceType: String
rawOcrText: String?
parseConfidence: Float
needsReview: Boolean
createdAt: Instant
updatedAt: Instant
```

## receipt_items

Stores extracted receipt line items.

```text
id: String
receiptId: String
rawText: String
cleanedText: String?
lineIndex: Int
nameRaw: String?
nameNormalized: String?
canonicalItemId: String?
quantity: Decimal?
unit: String?
unitPrice: Decimal?
lineTotal: Decimal?
currencyCode: String?
itemType: String
pantryCategory: String?
confidence: Float
needsReview: Boolean
userCorrected: Boolean
createdAt: Instant
updatedAt: Instant
```

Allowed `itemType` values:

```text
grocery
non_grocery
ignore
unknown
```

## canonical_items

Known normalized item identities.

```text
id: String
canonicalName: String
displayNameEn: String
displayNameDe: String?
displayNameRo: String?
displayNameId: String?
itemType: String
pantryCategory: String
defaultStorage: String?
defaultExpiryDays: Int?
isPantryEligible: Boolean
createdAt: Instant
updatedAt: Instant
```

## item_aliases

Multilingual and learned aliases.

```text
id: String
aliasText: String
aliasNormalized: String
canonicalItemId: String
languageCode: String?
countryCode: String?
merchantScope: String?
confidence: Float
source: String
createdAt: Instant
updatedAt: Instant
```

Allowed `source` values:

```text
seed
user_correction
merchant_rule
imported
```

## category_rules

Keyword rules for classification.

```text
id: String
keyword: String
keywordNormalized: String
languageCode: String?
countryCode: String?
itemType: String
pantryCategory: String?
priority: Int
createdAt: Instant
updatedAt: Instant
```

## pantry_items

Pantry inventory.

```text
id: String
canonicalItemId: String?
displayName: String
pantryCategory: String
storageLocation: String?
quantity: Decimal?
unit: String?
boughtDate: LocalDate
estimatedExpiryDate: LocalDate?
sourceReceiptId: String?
sourceReceiptItemId: String?
status: String
confidence: Float
createdAt: Instant
updatedAt: Instant
```

Allowed `status` values:

```text
active
used
expired
unknown
removed
```

## parse_corrections

Correction history.

```text
id: String
receiptItemId: String
fieldName: String
oldValue: String?
newValue: String?
createdAliasId: String?
createdAt: Instant
```

## Migration rule

Never wipe user corrections or aliases during seed database updates.
