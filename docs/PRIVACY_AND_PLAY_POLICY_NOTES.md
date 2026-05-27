# Privacy and Google Play Policy Notes

## Privacy principle

The MVP should be local-first:

```text
no account required
no cloud upload by default
no ads SDK
no analytics SDK
no unnecessary permissions
local receipt database
local pantry database
delete all data option
export data option
```

## Sensitive receipt data

Receipts may contain:

- store address/location
- purchase history
- timestamps
- payment method
- card last digits
- loyalty numbers
- order numbers
- email or customer identifiers

## MVP data behavior

The app should:

- scan/import receipts locally
- run OCR locally
- store parsed data locally
- allow user deletion
- allow user export
- avoid hidden transmission

## Permissions

Prefer:

- document scanner/photo picker instead of broad gallery access
- Storage Access Framework for PDFs
- no contacts
- no location
- no microphone
- no SMS
- no account permission

## Cloud/AI fallback later

Only add cloud parsing if:

- user explicitly opts in
- app explains what is uploaded
- app explains why
- user can disable it
- privacy policy is updated
- Play Data Safety is updated

## Play Store release checklist

- [ ] Privacy policy exists.
- [ ] Data Safety form matches actual behavior.
- [ ] User can delete receipts.
- [ ] User can delete all app data.
- [ ] User can export data.
- [ ] No hidden cloud upload.
- [ ] No unnecessary SDKs.
- [ ] No ads SDK unless monetization policy and privacy declaration are ready.
- [ ] No payment code unless purchase verification and policy plan are ready.
