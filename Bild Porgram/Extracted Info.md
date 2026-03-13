
## Domains/URL's explicity Mentioned

- `https://api.biltrewards.com`
- `https://static.biltrewards.com`
- `biltrewards://auth/callback`
- `https://www.datocms-assets.com/43819/`
- `https://www.datocms-assets.com/88005/`
- `https://policies.google.com/privacy`
- `https://policies.google.com/terms`
- `https://www.experian.com/blogs/ask-experian/how-to-unfreeze-your-credit-report/`
- `https://www.knotapi.com/privacy/`
- `mailto:privacy.office@walgreens.com`
- `mailto:support@biltrewards.c...` (truncated in the provided snippet)

---

## Domains and hosts listed in `h` (production-like) and `m` (staging-like) arrays:

- `biltrewards.com` (and subdomains via `(^|\.)biltrewards\.com$`)
- `auth.point.me`
- `bilt.awayz.com`
- `bilt.com`
- `bilt.goquiq.com`
- `bilt.page.link`
- `bilt.point.me`
- `biltcard.typeform.com`
- `biltrewards.zendesk.com`
- `connect.secure.wellsfargo.com`
- `link.bilt.page`
- `my.bilt.page`
- `www.amazon.com`
- `www.bilt.com`
- `www.blade.com`
- `www.wellsfargo.com`
- `www.walgreens.com`
- `mycommunity.americancampus.com`
- `www.google.com`
- `com.prod.loyalty.virgin.com`
- `virgin.com`
- `account.virgin.com`
- `virginred.app.link`
- `cardless-legal-agreements.s3.us-east-2.amazonaws.com`
- `corecard-statements-prod.s3.us-east-2.amazonaws.com`
- `verify.plaid.com`
- `verify.socure.com`
- `eappp.okta-emea.com`
- `eap.okta-emea.com`
- `secure.plaid.com`
- `id.uwm.com`
- `id.int.uwm.com`
- `api.workos.com`
- `www.united.com`
- `www.britishairways.com`
- `www.iberia.com`
- `www.aerlingus.com`
- `legal.cardless.com`
- `column.com`
- `www.prioritypass.com`
- `api.onefootprint.com`
- `www.lyft.com`
- `www.peacocktv.com`
- `www.instacart.com`
- `www.mycardbenefits.com`
- `mastercardus.idprotectiononline.com`
- `concierge.mastercard.com`
- `biltrewards.dev` (and subdomains via `(^|\.)biltrewards\.dev$`)
- `bilt-stage.goquiq.com`
- `staging-bilt.point.me`
- `biltuat.awayz.com`
- `localhost`
- `americancampus--stg.sandbox.my.site.com`
- `mycommunity-stg.americancampus.com`
- `com.model.loyalty.virgin.com`
- `virginred.test-app.link`
- `web.uat.sandbox.account.virgin.com`
- `cardless-legal-agreements-staging.s3.us-east-2.amazonaws.com`
- `corecard-statements-staging.s3.us-east-2.amazonaws.com`
- `verify-sandbox.plaid.com`
- `verify-sandbox.socure.com`
- `uwm.com` (and subdomains via `(^|\.)uwm\.com$`)

---

## Anoterj links 

The following potential endpoints/URLs are extracted from the code:
- `https://static.biltrewards.com/assets/lottie/` (for production Lottie animations)
- `https://staging.static.biltrewards.com/assets/lottie/` (for staging Lottie animations)
- `/_next/` (used as a prefix for dynamically loaded assets/chunks like `_next/static/chunks/` or `_next/[encodedURIPath]`)
- `/rewards` (from `t.clientHost/rewards`)
- `/login/enter-email` (Login path)
- `/signup/complete-profile` (Signup path)
----

### **Lottie Animation JSON Files:**
    - `https://static.biltrewards.com/assets/lottie/Bilt_LoadingAnimation_DarkBlue.json`
    - `https://static.biltrewards.com/assets/lottie/Bilt_LoadingAnimation_LightBlue.json`
    - `https://staging.static.biltrewards.com/assets/lottie/Bilt_LoadingAnimation_DarkBlue.json`
    - `https://staging.biltrewards.com/assets/lottie/Bilt_LoadingAnimation_LightBlue.json`
    - The specific one used depends on the `isProduction` flag and the theme palette mode.


## Entry Point ( Possible )

>**BRUTE-FOFCE**
https://staging.biltrewards.com/assets/lottie/Bilt_LoadingAnimation_LightBlue.json  

---


## Extracted Info
### Extracted Endpoints

The following are the API endpoints (relative URLs) directly referenced in this file through the `walletApi` service, likely targeting the `id.biltrewards.com` domain:

- **GET Wallet Data:**
    - `/wallet`
- **DELETE Credit Card:**
    - `/payment/third-party-card` (for `removeCreditCard` mutation)
- **POST Hide Prefetched Card:**
    - `/wallet/cards/${e}/hide` (where `${e}` is the card ID, for `hidePrefetchedCard` mutation)
- **PATCH Bank Account:**
    - `/user-bank/account/${e}` (where `${e}` is the account ID, for `editBankAccount` mutation)
- **DELETE Bank Account:**
    - `/user-bank/account/${e}` (where `${e}` is the account ID, for `removeBankAccount` mutation)
- **PATCH Update Enrolled Card User:**
    - `/seated/account` (for `updateEnrolledCardUser` mutation)
- **POST Add Plaid Account:**
    - `/plaid/account` (for `addPlaidAccount` mutation)
- **POST Add Manual Bank Account:**
    - `/user-bank/account` (for `addManualBankAccount` mutation)
- **POST Initiate Micro-deposit Verification:**
    - `/user-bank/account/${e}/microdeposit-verification` (where `${e}` is the account ID, for `initiateMicroDepositVerification` mutation)
- **PUT Complete Micro-deposit Verification:**
    - `/user-bank/account/${e}/microdeposit-verification` (where `${e}` is the account ID, for `completeMicroDepositVerification` mutation)

Additionally, there's an implied call to a Google Places API wrapper:

- **Google Places API:**
    - An internal function `o.getAddressDetailsWeb` which likely maps to a backend endpoint that then calls the Google Places API, or directly calls the Google Places API from the client (less likely for sensitive keys). This is invoked within the `getUnits` thunk. The actual Google Places API endpoint would be `https://maps.googleapis.com/maps/api/place/details/json?...`.