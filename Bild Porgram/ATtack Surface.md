
Here is the completed and updated Security Analysis Report, including the findings from the client-side analysis and a comprehensive summary of potential vulnerabilities.

---

### Security Analysis Report for Bug Bounty Hunter

#### Overview

This report provides a detailed security analysis of the identified API endpoints and client-side code from the provided application codebase. The investigation focused on potential vulnerabilities such as Insecure Storage, IDOR, SSRF, Logic Flaws, and Insecure Direct Object References. The analysis is presented in a structured manner to facilitate a bug bounty hunter's work.

---

### API Endpoints

The table has been updated to include a **"Vulnerability Potential"** column, which flags specific areas of interest for your testing.

#### Legend

- **Auth**: `true` = requires authentication (Bearer token likely), `false` = public.
    
- **Method**: HTTP method.
    
- **Path**: URL path (variables in `{}`).
    

#### 1. Bilt API Slice (User Profile)

|**Endpoint**|**Method**|**Path**|**Auth**|**Vulnerability Potential**|**Notes**|
|---|---|---|---|---|---|
|currentUserProfile|GET|`/user/profile`|true|Information Disclosure|Inspect the full response for leak of PII.|
|updateUserProfile|PUT|`/user/profile`|true|Data Tampering|Can you update fields that shouldn't be mutable?|
|closeAccount|DELETE|`/user/account/closure`|true|CRSF|High impact if non-authenticated close is possible.|
|nameChangeInitiate|POST|`/self-service/profile-names`|true|Rate Limiting|Abuse to flood system with change requests.|
|nameChangeComplete|PUT|`/self-service/profile-names`|true|Logic Flaw|Can you complete a name change without initiating it?|

#### 2. Wallet API

|**Endpoint**|**Method**|**Path**|**Auth**|**Vulnerability Potential**|**Notes**|
|---|---|---|---|---|---|
|getWallet|GET|`/wallet`|true|Information Disclosure|Leaks full card/account details.|
|removeCreditCard|DELETE|`/payment/third-party-card`|true|IDOR|Can you remove another user's card by ID?|
|hidePrefetchedCard|POST|`/wallet/cards/{cardId}/hide`|true|IDOR|Cross-user card manipulation.|
|editBankAccount|PATCH|`/user-bank/account/{id}`|true|IDOR / Data Tampering|Modify another user's bank details.|
|removeBankAccount|DELETE|`/user-bank/account/{id}`|true|IDOR|Remove another user's bank account.|
|updateEnrolledCardUser|PATCH|`/seated/account`|true|Data Tampering|Affects restaurant rewards.|
|addPlaidAccount|POST|`/plaid/account`|true|Race Condition|Try parallel requests to add the same account.|
|addManualBankAccount|POST|`/user-bank/account`|true|No-Rate Limit|Fuzz the `recaptchaInfo` to bypass protection.|
|initiateMicroDepositVerification|POST|`/user-bank/account/{accountId}/microdeposit-verification`|true|IDOR|Initiate verification on any account.|
|completeMicroDepositVerification|PUT|`/user-bank/account/{accountId}/microdeposit-verification`|true|IDOR / Bruteforce|Test if `microdeposit-verification` codes can be bruteforced.|

#### 3. Bilt Concierge API

|**Endpoint**|**Method**|**Path**|**Auth**|**Vulnerability Potential**|**Notes**|
|---|---|---|---|---|---|
|getConversations|GET|`/bilt-concierge/conversations`|true|Information Disclosure|Check `limit`/`offset` handling for leaks.|
|startConversation|POST|`/bilt-concierge/messages`|true|Information Disclosure|Leak through error messages or response.|
|sendMessage|POST|`/bilt-concierge/messages`|true|Improper Authorization|Send messages to a conversation you don't own.|
|getConversation|GET|`/bilt-concierge/conversations/{conversationId}/messages`|true|IDOR|Read any user's concierge chat.|
|createAttachment|POST|`/bilt-concierge/attachments`|true|LFI / Stored XSS|Test with malicious filenames (`../../`) or SVG/HTML files.|
|deleteAttachment|DELETE|`/bilt-concierge/attachments/{attachmentId}`|true|IDOR|Delete another user's attachments.|

#### 4. Benefits API

|**Endpoint**|**Method**|**Path**|**Auth**|**Vulnerability Potential**|**Notes**|
|---|---|---|---|---|---|
|getBenefitsCampaigns|GET|`/benefits/campaigns`|true|SSRF / Logic Flaw|Test with strange lat/long values or long query params.|

#### 5. Buy Home (Buying Power)

|**Endpoint**|**Method**|**Path**|**Auth**|**Vulnerability Potential**|**Notes**|
|---|---|---|---|---|---|
|getPropertyDetails|GET|`/buying-power/properties/{propertyId}`|true|Information Disclosure|Check if the response includes unscrubbed agent PII.|
|referAgent|POST|`/buying-power/refer-agent`|true|Mass Assignment|Check for extra parameters to set.|
|submitPreScreening|POST|`/buying-power/prescreening`|true|Data Tampering|Can you pass impossible prescreening answers?|
|convertPoints|GET|`/buying-power/points/down-payment`|true|Price Tampering / Race Condition|Check if conversion rates can be manipulated.|
|requestAgentReassignment|POST|`/buying-power/agent/reassign`|true|Unvalidated Redirection / SSRF|How is the reassignment request handled?|
|getCollection|POST|`/buying-power/collections/{collectionId}/properties`|true|IDOR|View any user's collection.|

#### 7. Fitness API

|**Endpoint**|**Method**|**Path**|**Auth**|**Vulnerability Potential**|**Notes**|
|---|---|---|---|---|---|
|getNearbyFitnessClasses|GET|`/fitness/classes/nearby`|true|Logic Flaw / Business Logic Abuse|Use high `radius` or cross-continent lat/long to find unintended data.|
|updateSoulcycleClassBooking|PUT|`/soulcycle/reserved_classes/{reservationId}`|true|IDOR|Change the `seat_id` for another user's booking.|
|cancelSoulcycleClassBooking|DELETE|`/soulcycle/reserved_classes/{reservationId}`|true|IDOR|Cancel any user's Soulcycle booking.|
|createSoulcycleBikeReservation|POST|`/soulcycle/v2/bike-reservation`|true|Data Tampering / Price Tampering|Can you pass extra params? Test the complex body.|
|cancelFitnessClassBooking|POST|`/fitness/classes/bookings/{bookingId}/cancel`|true|IDOR|Cancel any user's fitness booking.|
|deleteFavoriteFitnessLocation|DELETE|`/fitness/locations/{locationId}/favorites`|true|IDOR|Delete any user's favorite fitness location.|

#### 8. HSA/FSA API

|**Endpoint**|**Method**|**Path**|**Auth**|**Vulnerability Potential**|**Notes**|
|---|---|---|---|---|---|
|getOnboardedUser|GET|`/hsa-fsa/users/{userId}`|true|IDOR / Information Disclosure|Read other users' PII by `userId`.|
|submitSilverMigration|POST|`/hsa-fsa/users/{userId}/silver-migration`|true|Logic Flaw|High-risk process. Can you skip steps or trigger an error that reveals data?|

#### 14. Payments API

|**Endpoint**|**Method**|**Path**|**Auth**|**Vulnerability Potential**|**Notes**|
|---|---|---|---|---|---|
|confirmPayment|POST|`/payment/{id}/confirmation`|true|Price Tampering / IDOR|Can you confirm a payment with `pointsToRedeem` greater than your balance or on another user's payment?|
|declareFraud|POST|`/payment/{id}/fraud-declaration`|true|CRSF|High risk. Declare fraud on any payment.|
|updateRecurringPaymentAuthorization|PATCH|`/payment/authorization/recurring`|true|Data Tampering|Can you change payment amounts or frequencies in an invalid state?|
|getMastercardOfferUpsell|GET|`/payment/offer/mastercard`|true|Business Logic Abuse / Information Disclosure|Does it leak too much info about the card/account?|

#### 15. Pharmacy API

|**Endpoint**|**Method**|**Path**|**Auth**|**Vulnerability Potential**|**Notes**|
|---|---|---|---|---|---|
|searchPharmacies|GET|`/public/merchants`|false|SSRF / Logic Flaw|Public endpoint, abuse filters and location params.|
|postHipaaConsent|POST|`/pharmacy/hipaa/consent`|true|improper Authorization|Test parallel requests to consent multiple times.|
|getHistoricalItems|GET|`/pharmacy/hsafsa/eligible-items`|true|Information Disclosure|Leaks transaction history.|
|createHsaFsaClaimTransaction|POST|`/pharmacy/hsafsa/claim/transactions`|true|IDOR / Price Tampering|Try putting an `id` that is not yours.|
|createAutoClaimSubmissionConsent|POST|`/pharmacy/automatic-claims/consent`|true|Improper Authorization|Test parallel requests.|
|createHsaFsaClaimTransactionV2|POST|`/pharmacy/v2/hsafsa/claim/transactions`|true|IDOR / Price Tampering|Use `requestId` and check for race conditions.|

#### 19. Surveys API

|**Endpoint**|**Method**|**Path**|**Auth**|**Vulnerability Potential**|**Notes**|
|---|---|---|---|---|---|
|loadSurvey|POST|`/surveys/{surveyId}/load`|true|IDOR / Business Logic Abuse|Test for unexpected responses with known public `surveyId`.|
|answerQuestion|POST|`/surveys/responses/{responseId}/questions/{questionId}`|true|IDOR / Data Tampering|Can you answer questions in a survey response that isn't yours?|

#### 20. SVG Icon API

|**Endpoint**|**Method**|**Path**|**Auth**|**Vulnerability Potential**|**Notes**|
|---|---|---|---|---|---|
|getSvg|GET|`/{svgPath}`|false?|Path Traversal / LFI / RFI|Very high risk. Test with `../../etc/passwd` or external URLs.|

#### 21. User Agreement API

|**Endpoint**|**Method**|**Path**|**Auth**|**Vulnerability Potential**|**Notes**|
|---|---|---|---|---|---|
|enableFeature|POST|`/feature/{featureId}/subscription`|true|Logic Flaw / Business Logic Abuse|Can you re-enable features multiple times?|

#### 22. Virtual Account API

|**Endpoint**|**Method**|**Path**|**Auth**|**Vulnerability Potential**|**Notes**|
|---|---|---|---|---|---|
|createVirtualAccount|POST|`/user-bank/v2/vaccount`|true|Mass Assignment / Data Tampering|High risk. Test extra params to set immutable fields.|
|updateVirtualAccount|PATCH|`/user-bank/v2/vaccount`|true|Data Tampering / Race Condition|Check if a parallel update can cause an invalid state.|

#### 23. Dining (POS) Async Thunks

| Endpoint | Method | Path | Auth | Vulnerability Potential | Notes |

|----------|--------|------|------|-------|

| getDiningOrderDetails | GET | `/pos/order/{orderId}` | true | IDOR | View other users' detailed orders. |

| getDiningChecks | GET | `/pos/order/{orderId}/check` | true | IDOR | View other users' check details. |

| makePaymentV2 | POST | `/pos/order/{orderId}/check/{checkId}/payment` | true | Price Tampering / Logic Flaw / Race Condition | Very complex body. Try modifying amounts and using race conditions. |

| postMatchTable | POST | `/pos/order/match` | true | Improper Authorization | Test if any user can match a table to a reservation they don't own. |

| exchangePayForGuestToken | POST | `/public/pos/visits/{visitId}/pay-for-guest/exchange-token` | false | Token Theft / Reuse | Public endpoint. Test for token validity, reuse, and expiration. |

| payForGuest | POST | `/public/pos/visits/{visitId}/pay-for-guest` | false | Token Theft / Reuse / Price Tampering | Test for unauthenticated price modification. |

#### 24. Loyalty Experience Endpoints (fS)

|**Endpoint**|**Method**|**Path**|**Auth**|**Vulnerability Potential**|**Notes**|
|---|---|---|---|---|---|
|getExperienceInventory|GET|`/loyalty/experiences/{name}`|true|Information Disclosure|Leaks available experiences, potentially with details.|
|claimExperience|POST|`/loyalty/event/{name}`|true|Race Condition / Price Tampering|Try a parallel request to claim an experience twice or modify the `claimAmount`.|
|cancelExperienceEntry|PUT|`/loyalty/event/{name}/cancel`|true|IDOR|Cancel any user's experience entry.|

#### 25. Loyalty User Endpoints (hr)

| Endpoint | Method | Path | Auth | Vulnerability Potential | Notes |

|----------|--------|------|------|-------|

| postCouponCode | POST | `/loyalty/coupon-code` | true | Bruteforce / Replay | Test for brute-forceable coupon codes or replaying already-used codes. |

| getPointsActivity | GET | `/loyalty/activity` | true | Information Disclosure | Check `limit`/`offset` for data leaks. |

| linkPartnerLoyaltyId | POST | `/loyalty/partner/link` | true | Account Takeover / Improper Authorization | Link a partner account to a Bilt user who doesn't own it. |

| discoverPartnerLoyaltyId | POST | `/loyalty/partner/discover` | true | Improper Authorization | Test if any user can discover partner accounts. |

#### 30. Maintenance (Service Requests)

|**Endpoint**|**Method**|**Path**|**Auth**|**Vulnerability Potential**|**Notes**|
|---|---|---|---|---|---|
|updateRequest|PATCH|`/property/{propertyUuid}/service-requests/{id}`|true|IDOR / Data Tampering|Modify another user's service request or its status.|
|getAttachmentUploadUrl|POST|`/property/{propertyUuid}/service-requests/{id}/attachments/upload-url`|true|Path Traversal / Stored XSS|Test with malicious filenames (`../../`) or SVG/HTML files.|
|getAttachmentDownloadUrl|GET|`/property/{propertyUuid}/service-requests/{id}/attachments/{attachmentId}`|true|IDOR|Download other users' service request attachments.|
|deleteAttachment|DELETE|`/property/{propertyUuid}/service-requests/{id}/attachments/{attachmentId}`|true|IDOR|Delete any user's service request attachments.|
|uploadFile|PUT|(signedUrl)|false|LFI / RFI / SSRF|Abuse this to upload malicious files or for SSRF/RFI via `signedUrl` parameter.|

#### 32. Rent Reporting (Opt-in)

|**Endpoint**|**Method**|**Path**|**Auth**|**Vulnerability Potential**|**Notes**|
|---|---|---|---|---|---|
|deleteOptInRequestInApp|DELETE|`/rent-credit-reporting/user/opt-in/{id}`|true|IDOR|Delete any user's rent reporting opt-in request.|

#### 34. Virtual Account (Legacy)

| Endpoint | Method | Path | Auth | Vulnerability Potential | Notes |

|----------|--------|------|------|-------|

| craeteVirtualAccount | POST | `/user-bank/v2/vaccount` | true | Mass Assignment / Data Tampering | High risk. Test extra params to set immutable fields. |

| updateVirtualAccount | PATCH | `/user-bank/v2/vaccount` | true | Data Tampering / Race Condition | Check if a parallel update can cause an invalid state. |

#### 36. FusionAuth Logout

| Endpoint | Method | Path | Auth | Vulnerability Potential | Notes |

|----------|--------|------|------|-------|

| wwwLogout | POST | `/public/identity/auth/logout` | true | Account Takeover / CRSF | If `POST /public/identity/auth/logout` is possible without proper validation, this is high impact. Abuse for unexpected session termination or token-based logout of other users. |

#### 37. Exchange Engage Token

| Endpoint | Method | Path | Auth | Vulnerability Potential | Notes |

|----------|--------|------|------|-------|

| exchangeEngageToken | POST | `/loyalty/exchange-engage-token` | true | Token Theft / Reuse | Token theft of a powerful token. Check validity and reuse. |

#### 42. In-Network Auth

| Endpoint | Method | Path | Auth | Vulnerability Potential | Notes |

|----------|--------|------|------|-------|

| authInNetworkResident | POST | `/public/auth-inn-resident` or `/api/bilt/public/auth-inn-resident` | false? | Account Takeover / Bruteforce / Replay | Test for brute-forceable codes and replaying old tokens. |

#### 43. SoulCycle (additional)

| Endpoint | Method | Path | Auth | Vulnerability Potential | Notes |

|----------|--------|------|------|-------|

| fetchStudiosInRegion | GET | `/soulcycle/fetch-studios` | true | Information Disclosure | Leaks available Soulcycle studios. |

| cancelBikeReservation | DELETE | `/soulcycle/reserved_classes/{reservationId}` | true | IDOR | Cancel any user's bike reservation. |

| updateBooking | PUT | `/soulcycle/reserved_classes/{reservationId}` | true | IDOR | Update any user's Soulcycle booking, e.g., changing the `seat_id`. |

| getReservedSeats | GET | `/soulcycle/class/{classId}/reserved-seats` | true | Information Disclosure | Leaks all reserved seats for a given class. |

| reserveBike | POST | `/soulcycle/v2/bike-reservation` | true | Data Tampering / Price Tampering | Complex body with `seonFingerprint`. Pass extra params or try modifying prices. |

#### 44. Map Search

| Endpoint | Method | Path | Auth | Vulnerability Potential | Notes |

|----------|--------|------|------|-------|

| searchMerchants | GET | `/v1/catalog/merchants/search` | true | SSRF / Information Disclosure | Public search functionality. Test for SSRF, path traversal, or information disclosure in the response. |

#### 46. Partner Login (Third-Party JWT)

| Endpoint | Method | Path | Auth | Vulnerability Potential | Notes |

|----------|--------|------|------|-------|

| partnerLogin | POST | `/public/auth/third-party-jwt/{thirdPartyName}/login` | false | Improper Authorization / Token Theft | Public endpoint. Fuzz `thirdPartyName` and test JWT validation. |

#### 48. Deck (External Connections)

| Endpoint | Method | Path | Auth | Vulnerability Potential | Notes |

|----------|--------|------|------|-------|

| validateURL | POST | `/external/deck/validate-url` | true | SSRF / Unvalidated Redirection | High risk. Abuse to access internal resources or for a simple open redirect. |

| answerMfa | POST | `/external/deck/mfa` | true | Bruteforce / Improper Authorization | Bruteforce MFA codes. Try to reuse or bypass MFA with different `mfa` parameters. |

| getConnectionStatus | GET | `/external/deck/{jobId}/session-status` | true | IDOR | Read any user's connection job status. |

| getUserConnectionInfo | GET | `/external/deck/account/status` | true | Information Disclosure | Leaks connection account details. |

| deleteConnection | DELETE | `/external/deck/user/credential` | true | IDOR | Delete any user's credential connection. |

#### 49. Bilt Cash

| Endpoint | Method | Path | Auth | Vulnerability Potential | Notes |

|----------|--------|------|------|-------|

| getCheckoutSettings | GET | `/unit/{unitId}/checkout-settings` | true | Information Disclosure | Check if the response leaks PII. |

| deleteCheckoutSettings | DELETE | `/unit/{unitId}/checkout-settings` | true | IDOR | Delete any user's checkout settings. |

---

### Security Vulnerabilities & Observations

The analysis confirms and adds to your original observations. Here is the final detailed breakdown of client-side and logic vulnerabilities.

#### 1. Insecure Storage of Sensitive Data in LocalStorage

A review of the Redux state persistence configuration across various slices confirms a major vulnerability. The application uses `redux-persist` with `localStorage` for multiple slices, storing highly sensitive data in a location accessible to any JavaScript running on the page, including that from a malicious XSS attack.

- **Auth Tokens**: The `auth` slice (`frontend/src/store/slices/auth/authSlice.ts`, see `I0`) is persisted. While the specific keys were not explicitly visible in the type definition, common practice for an auth slice involves storing `accessToken`, `idToken`, and potentially `refreshToken`. You should confirm the exact keys being persisted. A hunter can use XSS to steal these tokens and gain full account access.
    
- **PII in SignUp**: The `signUp` slice (`frontend/src/store/slices/app/signUpSlice.ts`, see `IJ`) stores `firstName`, `lastName`, `email`, `phone`, and `dateOfBirth` in localStorage. This is a severe PII leak. This data can be easily stolen and used for identity theft, which is a major compliance violation (e.g., GDPR).
    
- **Amazon Linking Token**: The `amazonLinking` slice (`frontend/src/store/slices/external/amazonLinkingSlice.ts`, see `IL`) stores an `authorizationToken`. If this token provides access to an Amazon account, it must not be stored in localStorage.
    
- **Amazon Order History PII**: The `amazonOrderHistory` slice stores `city` and `zipCode` in localStorage. This is another PII leak and can be used to locate a user.
    
- **Bilt Concierge Drafts**: The `conciergeDraft` slice stores draft messages, which could contain sensitive user queries, in localStorage. This can lead to information disclosure.
    
- **Liabilities Pre-filling**: The `liabilities` slice stores `liabilitiesSearchPreFillData` in localStorage. This is a severe PII leak and could include bank account numbers or other financial details.
    

**Hunter's Actionable Advice:** To demonstrate this, construct an XSS payload that exfiltrates `localStorage.getItem('persist:signUp')` or `localStorage.getItem('persist:auth')` to an external server. Submit it with a screenshot of the exfiltrated data.

---

#### 2. Notable Client-Side Functions & Logic to Audit

These functions from the codebase present high-risk targets for a hunter, often because they make assumptions that can be subverted by a malicious client.

- **Open Redirect / SSRF in SVGIcon Component**: The `SVGIcon` component (`frontend/src/components/common/SVGIcon/SVGIcon.tsx`, see `I6`) constructs a dynamic `fetch` URL: `fetch(`${baseIconUrl}${src}`)`. The `src` prop comes from the component usage. If a user can inject a value for `src` into the application (e.g., through a profile field or comment), they can force a server-side request (SSRF) to a target of their choice, which can lead to open redirects, port scanning, and potentially full-blown SSRF of internal resources.
    
    - **Hunt Strategy**: This is a classic SSRF target. Test with a local path traversal payload (`src='../../../../etc/passwd'`), an external URL payload (`src='http://evil.com/malicious.svg'`), and an internal resource payload (`src='http://localhost:8080/internal-api'`).
        
- **Path Traversal in SVGIcon**: Based on `I6`, a user can pass `../` into the `src` parameter of `SVGIcon` and potentially read files on the server.
    
    - **Hunt Strategy**: Test a path traversal payload such as `src='../../../../etc/passwd'`.
        
- **FusionAuth Logout Logic**: The `wwwLogout` endpoint (Endpoint 36) has been identified as a high risk for account takeover and CRSF.
    
    - **Hunt Strategy**: If `POST /public/identity/auth/logout` is possible without proper validation, this is high impact. Abuse for unexpected session termination or token-based logout of other users.
        
- **Token-Based Logic in POS/Dining**: Endpoints 23 and 24 are excellent targets for token reuse and token theft.
    
    - **Hunt Strategy**: For token-based endpoints, try using a token for a different user, or reuse a token for a different endpoint. Price tampering is also high potential. Try parallel requests and using race conditions.
        

---

#### 3. Summary of Vulnerabilities for a Bug Bounty Hunter

This section groups findings into common bug classes, making it easier for you to plan your attacks.

- **Insecure Data Storage**: As detailed in section 1, multiple PII and authentication tokens are stored in localStorage. This makes the application highly susceptible to severe data breaches through XSS.
    
- **IDOR (Insecure Direct Object Reference)**: This is a major area of risk for the application. Any endpoint where a user-controlled parameter (`id`, `accountId`, `conversationId`, `bookingId`, `visitId`, `reservationId`, `externalConnectionId`, `externalConnectionOTPId`, `externalCredentialId`, `paymentHistoryId`, `rentdayFreeGameResultId`) is used to access a resource must be checked to ensure that the user has permission to access that resource. Examples of high-impact IDOR endpoints include:
    
    - Wallet (`removeCreditCard`, `hidePrefetchedCard`, `editBankAccount`, `removeBankAccount`, `initiateMicroDepositVerification`, `completeMicroDepositVerification`)
        
    - Concierge (`getConversation`, `deleteAttachment`)
        
    - Buy Home (`favoriteProperty`, `unfavoriteProperty`, `getCollection`, `submitPreScreening`)
        
    - SoulCycle (`cancelBikeReservation`, `updateBooking`)
        
    - Fitness (`cancelFitnessClassBooking`, `deleteFavoriteFitnessLocation`)
        
    - Pharmacy (`getTransactionById`, `createHsaFsaClaimTransaction`, `createAutoClaimSubmissionConsent`)
        
    - Surveys (`answerQuestion`, `getSurveyStatus`)
        
    - Maintenance (`updateRequest`, `getRequestById`, `getAttachmentDownloadUrl`, `deleteAttachment`)
        
    - Rent Reporting (`deleteOptInRequestInApp`)
        
    - Deck (`deleteConnection`)
        
- **SSRF (Server-Side Request Forgery)**:
    
    - `/external/deck/validate-url` (Endpoint 48) is a high-risk SSRF target. It is used to validate a user-supplied URL and could be abused to access internal systems.
        
    - `searchMerchants` (Endpoint 44) uses many user-supplied parameters, which is a potential SSRF target.
        
- **Improper Authorization**:
    
    - `authInNetworkResident` (Endpoint 42) is used for public authentication. Bruteforcing codes and token reuse are high potential attacks.
        
    - `linkPartnerLoyaltyId` (Endpoint 25) is used for account linking, which can lead to improper authorization if non-authenticated linking is possible.
        
- **Logic Flaws**:
    
    - `closeAccount` (Endpoint 1) has high impact if non-authenticated close is possible.
        
    - `nameChangeComplete` (Endpoint 1) has potential logic flaws. Test if you can complete a name change without initiating it.
        
    - `submitSilverMigration` (Endpoint 8) is a high-risk process. Test if you can skip steps or trigger an error that reveals data.
        
    - `confirmPayment` (Endpoint 14) has price tampering potential. Try modifying amounts and using race conditions.
        
    - `enableFeature` (Endpoint 21) is a business logic abuse target. Try re-enabling features multiple times.
        
    - `postMatchTable` (Endpoint 23) has improper authorization potential. Test if any user can match a table to a reservation they don't own.
        
- **Data Tampering**:
    
    - `updateUserProfile` (Endpoint 1) has data tampering potential. Try modifying fields that shouldn't be mutable.
        
    - `updateEnrolledCardUser` (Endpoint 2) has data tampering potential. Try modifying card user details.
        
    - `completeMicroDepositVerification` (Endpoint 2) has data tampering potential. Try modifying verification codes.
        
    - `submitPreScreening` (Endpoint 5) has data tampering potential. Test with impossible prescreening answers.
        
    - `updateSoulcycleClassBooking` (Endpoint 7) has IDOR potential. Change the `seat_id` for another user's booking.
        
    - `createSoulcycleBikeReservation` (Endpoint 7) has data tampering potential. Test the complex body and pass extra params.
        
    - `createHsaFsaClaimTransaction` (Endpoint 15) has price tampering potential. Try putting an `id` that is not yours.
        
    - `answerQuestion` (Endpoint 19) has data tampering potential. Can you answer questions in a survey response that isn't yours?
        
    - `updateVirtualAccount` (Endpoint 22) has data tampering potential. Try parallel updates.
        
    - `makePaymentV2` (Endpoint 23) has price tampering potential. Try modifying amounts and using race conditions.
        
    - `claimExperience` (Endpoint 24) has price tampering potential. Try modifying the `claimAmount`.
        
    - `updateRequest` (Endpoint 30) has data tampering potential. Modify another user's service request or its status.
        
    - `reserveBike` (Endpoint 43) has data tampering potential. Try modifying prices.
        
- **Race Conditions**:
    
    - `addPlaidAccount` (Endpoint 2) has race condition potential. Try parallel requests to add the same account.
        
    - `completeMicroDepositVerification` (Endpoint 2) has race condition potential. Try parallel requests.
        
    - `postMerchantNetworkEnrollment` (Endpoint 18) has race condition potential. Try parallel requests.
        
    - `postHistoricalItems` (Endpoint 15) has race condition potential. Try parallel requests.
        
    - `createHsaFsaClaimTransaction` (Endpoint 15) has race condition potential. Try parallel requests.
        
    - `createHsaFsaClaimTransactionV2` (Endpoint 15) has race condition potential. Try parallel requests.
        
    - `loadSurvey` (Endpoint 19) has race condition potential. Try parallel requests.
        
    - `createAutopayConfig` (Endpoint 27) has race condition potential. Try parallel requests.
        
    - `createVirtualAccount` (Endpoint 34) has race condition potential. Try parallel requests.
        
    - `provisionCheckSubledgerAccount` (Endpoint 34) has race condition potential. Try parallel requests.
        
    - `updateVirtualAccount` (Endpoint 34) has race condition potential. Try parallel requests.
        
- **Information Disclosure**:
    
    - `currentUserProfile` (Endpoint 1) has information disclosure potential. Inspect the full response for leak of PII.
        
    - `getWallet` (Endpoint 2) has information disclosure potential. Leaks full card/account details.
        
    - `getConversations` (Endpoint 3) has information disclosure potential. Check `limit`/`offset` handling for leaks.
        
    - `getPropertyDetails` (Endpoint 5) has information disclosure potential. Check if the response includes unscrubbed agent PII.
        
    - `getOnboardedUser` (Endpoint 8) has IDOR potential. Read other users' PII by `userId`.
        
    - `getHistoricalItems` (Endpoint 15) has information disclosure potential. Leaks transaction history.
        
    - `getDiningVisits` (Endpoint 23) has information disclosure potential. Does `UPCOMING` filter include other users' upcoming visits?
        
    - `getExperienceInventory` (Endpoint 24) has information disclosure potential. Leaks available experiences.
        
    - `getPointsActivity` (Endpoint 25) has information disclosure potential. Check `limit`/`offset` for data leaks.
        
    - `getAutopayConfigById` (Endpoint 27) has IDOR potential. View other users' autopay configs.
        
    - `getUserConnectionInfo` (Endpoint 48) has information disclosure potential. Leaks connection account details.
        
    - `getCheckoutSettings` (Endpoint 49) has information disclosure potential. Check if the response leaks PII.