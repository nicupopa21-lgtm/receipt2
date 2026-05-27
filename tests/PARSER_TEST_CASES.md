# Parser Test Cases

## Test case format

Each test should include:

```text
id
region
language
currency
inputRawOcr
expectedMerchant
expectedDate
expectedTotal
expectedItems
expectedGroceryCount
expectedNonGroceryCount
expectedIgnoreCount
expectedPantryItems
```

## Required receipt groups

- English US grocery receipt
- English online receipt
- German supermarket receipt
- Romanian supermarket receipt
- Indonesian minimarket receipt
- Mixed grocery + household receipt
- Receipt with discounts
- Receipt with deposits/Pfand
- Receipt with weighted produce
- Receipt with OCR mistakes
- Long receipt
- Bad photo receipt
- PDF receipt
- Screenshot receipt

## Example: German receipt

```text
LIDL
Bon
Milch 1L                 1,29
Bananen 0,824 kg x 1,79  1,47
Waschmittel              4,99
Pfand                    0,25
Zwischensumme            7,75
MwSt                     0,45
Gesamt                   7,75
```

Expected:

```json
{
  "currency": "EUR",
  "total": 7.75,
  "items": [
    { "name": "Milch", "canonical": "milk", "type": "grocery", "category": "dairy", "lineTotal": 1.29 },
    { "name": "Bananen", "canonical": "banana", "type": "grocery", "category": "fresh_produce", "lineTotal": 1.47 },
    { "name": "Waschmittel", "canonical": "detergent", "type": "non_grocery", "category": "household_non_food", "lineTotal": 4.99 },
    { "name": "Pfand", "type": "ignore", "lineTotal": 0.25 }
  ]
}
```

## Example: Indonesian receipt

```text
INDOMARET
SUSU UHT 1L            18000
INDOMIE GORENG 2 PCS    7000
DETERJEN               12000
SUBTOTAL               37000
PPN                     3700
TOTAL                  40700
TUNAI                  50000
KEMBALI                 9300
```

Expected:

```json
{
  "currency": "IDR",
  "total": 40700,
  "items": [
    { "name": "SUSU UHT 1L", "canonical": "milk", "type": "grocery", "category": "dairy" },
    { "name": "INDOMIE GORENG 2 PCS", "canonical": "instant_noodles", "type": "grocery", "category": "grains_pasta_rice" },
    { "name": "DETERJEN", "canonical": "detergent", "type": "non_grocery", "category": "household_non_food" }
  ]
}
```

## Regression rule

Every parser bug should become a permanent test case.
