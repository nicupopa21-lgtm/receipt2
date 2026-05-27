# Seed Comparison Database Plan

## Goal

Create a small local comparison database to identify common groceries, non-groceries, ignore lines, and pantry behavior.

Do not try to build a complete world product database at launch.

## MVP seed target

```text
100 common groceries
50 common non-groceries
50 ignore/payment/tax/receipt terms
4 language aliases where possible
```

## Supported languages

- English
- German
- Romanian
- Indonesian

## Canonical item example

```json
{
  "id": "milk",
  "canonicalName": "milk",
  "displayNameEn": "Milk",
  "displayNameDe": "Milch",
  "displayNameRo": "Lapte",
  "displayNameId": "Susu",
  "itemType": "grocery",
  "pantryCategory": "dairy",
  "defaultStorage": "fridge",
  "defaultExpiryDays": 7,
  "isPantryEligible": true
}
```

## Alias example

```json
[
  { "aliasText": "milk", "canonicalItemId": "milk", "languageCode": "en", "source": "seed" },
  { "aliasText": "milch", "canonicalItemId": "milk", "languageCode": "de", "source": "seed" },
  { "aliasText": "lapte", "canonicalItemId": "milk", "languageCode": "ro", "source": "seed" },
  { "aliasText": "susu", "canonicalItemId": "milk", "languageCode": "id", "source": "seed" },
  { "aliasText": "susu uht", "canonicalItemId": "milk", "languageCode": "id", "source": "seed" }
]
```

## Initial grocery categories

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
other
unknown
```

## Non-grocery categories

```text
household_non_food
personal_care_non_food
medicine_health
paper_goods
cleaning
electronics
other_non_food
```

## Ignore terms

English:

```text
total
subtotal
tax
vat
cash
card
change
receipt
invoice
coupon
discount
deposit
```

German:

```text
gesamt
summe
zwischensumme
mwst
ust
bar
karte
rückgeld
bon
rabatt
pfand
```

Romanian:

```text
total
subtotal
tva
bon fiscal
numerar
card
rest
reducere
lei
ron
```

Indonesian:

```text
total
subtotal
ppn
tunai
kartu
kembali
kembalian
diskon
rp
harga
jumlah
```

## First groceries to seed

```text
milk
rice
bread
eggs
banana
apple
potato
onion
garlic
tomato
chicken
fish
salmon
beef
pork
yogurt
cheese
butter
oil
flour
sugar
salt
pepper
pasta
noodles
instant noodles
coffee
tea
water
juice
coconut milk
chili
ginger
galangal
candlenut
shrimp
beans
long beans
chayote
nori
snacks
```

## Learning rule

When the user corrects an item, create a new alias.

Example:

```text
Raw receipt line: MLCH 1.5 1L
User correction: milk
Stored alias: MLCH → milk
```

Merchant-specific corrections should be scoped when the raw text is likely store-specific.
