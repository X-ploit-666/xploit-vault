
## Security Analysis Report for Bug Bounty Hunter

### Overview
This report documents the API endpoints, client-side functions, and potential security vulnerabilities identified in the provided codebase. The application appears to be a large React/Redux-based web app with multiple backend services. The analysis focuses on:

- All API endpoints (REST and GraphQL) with methods, paths, and authentication status.
- Client-side storage of sensitive data.
- Potential IDOR, open redirect, and insecure direct object references.
- Notable functions that could be abused.

---

### API Endpoints

#### Legend
- **Auth**: `true` = requires authentication (Bearer token likely), `false` = public.
- **Method**: HTTP method.
- **Path**: URL path (variables in `{}`).

#### 1. Bilt API Slice (User Profile)
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| currentUserProfile | GET | `/user/profile` | true | Returns user profile |
| updateUserProfile | PUT | `/user/profile` | true | Updates profile |
| closeAccount | DELETE | `/user/account/closure` | true | Deletes account |
| nameChangeInitiate | POST | `/self-service/profile-names` | true | Initiates name change (with seonFingerprint) |
| nameChangeComplete | PUT | `/self-service/profile-names` | true | Completes name change |

#### 2. Wallet API
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| getWallet | GET | `/wallet` | true | Fetches wallet (credit cards, bank accounts) |
| removeCreditCard | DELETE | `/payment/third-party-card` | true | Body: `{ id }` |
| hidePrefetchedCard | POST | `/wallet/cards/{cardId}/hide` | true | |
| editBankAccount | PATCH | `/user-bank/account/{id}` | true | Body: `{ name, defaultAccount }` |
| removeBankAccount | DELETE | `/user-bank/account/{id}` | true | |
| updateEnrolledCardUser | PATCH | `/seated/account` | true | |
| addPlaidAccount | POST | `/plaid/account` | true | |
| addManualBankAccount | POST | `/user-bank/account` | true | Body includes `recaptchaInfo` |
| initiateMicroDepositVerification | POST | `/user-bank/account/{accountId}/microdeposit-verification` | true | |
| completeMicroDepositVerification | PUT | `/user-bank/account/{accountId}/microdeposit-verification` | true | |

#### 3. Bilt Concierge API
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| getConversations | GET | `/bilt-concierge/conversations` | true | Query: `offset`, `limit` |
| startConversation | POST | `/bilt-concierge/messages` | true | Body with message, conversationId, etc. |
| sendMessage | POST | `/bilt-concierge/messages` | true | Same as start |
| closeConversation | - | - | true | Stubbed (no actual request) |
| getConversation | GET | `/bilt-concierge/conversations/{conversationId}/messages` | true | Query: `offset`, `limit` |
| createAttachment | POST | `/bilt-concierge/attachments` | true | Body: `fileName`, `contentType`, `conversationId` |
| deleteAttachment | DELETE | `/bilt-concierge/attachments/{attachmentId}` | true | |
| submitFeedback | POST | - | true | Stubbed |

#### 4. Benefits API
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| getBenefitsCampaigns | GET | `/benefits/campaigns` | true | Query params: `category`, `subcategory`, `tag`, `latitude`, `longitude`, etc. |
| getBenefitsCategories | GET | `/benefits/categories` | true | |

#### 5. Buy Home (Buying Power)
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| getPropertyBucket | POST | `/buying-power/search/property/recommended` | true | Body includes filters |
| getDefaultPropertyBucket | POST | `/buying-power/search/property/recommended/default` | true | |
| searchProperties | POST | `/buying-power/search/property` | true | |
| getFavorites | POST | `/buying-power/search/property/favorite` | true | |
| getPropertyDetails | GET | `/buying-power/properties/{propertyId}` | true | Query: `mortgageOptimization` |
| favoriteProperty | POST | `/buying-power/properties/{propertyId}/favorite` | true | |
| unfavoriteProperty | DELETE | `/buying-power/properties/{propertyId}/favorite` | true | |
| referAgent | POST | `/buying-power/refer-agent` | true | |
| contactAgent | POST | `/buying-power/properties/contact-agent` | true | |
| submitPreScreening | POST | `/buying-power/prescreening` | true | |
| convertPoints | GET | `/buying-power/points/down-payment` | true | Query: `pointAmount` |
| getBuyingPowerProfile | GET | `/buying-power/profile` | true | |
| setBuyingPowerProfile | PUT | `/buying-power/profile` | true | |
| updateBuyingPowerPreferences | PUT | `/buying-power/preference` | true | |
| getAutocomplete | GET | `/buying-power/search/location` | true | Query: `input`, `sessionToken`, `limit` |
| getAccessStatus | GET | `/buying-power/access` | true | Query params |
| getAssignedAgent | GET | `/buying-power/agent` | true | |
| requestAgentReassignment | POST | `/buying-power/agent/reassign` | true | |
| getCollections | POST | `/buying-power/collections` | true | |
| getCollection | POST | `/buying-power/collections/{collectionId}/properties` | true | |

#### 6. Credit Report API
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| getCreditScore | POST | `/experian/get-credit-score` | true | |
| getRentalSimulationScore | GET | `/experian/simulation/rental` | true | |

#### 7. Fitness API
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| getFitnessBrands | GET | `/public/fitness/brands` | true | |
| getNearbyFitnessClasses | GET | `/fitness/classes/nearby` | true | Query params (brand, lat, lng, radius, etc.) |
| getFitnessClassDetails | GET | `/fitness/classes/{biltClassId}/details` | true | |
| getFitnessLocation | GET | `/fitness/locations/{biltLocationId}` | true | |
| getSoulcycleClasses | GET | `/soulcycle/fetch-classes` | true | Query: `startDate`, `endDate`, `regionId`, `studioId`, etc. |
| getSoulcycleClassDetails | GET | `/soulcycle/class/{classId}` | true | |
| getSoulcycleRentDayRides | GET | `/soulcycle/class/{classId}/rent-day-rides` | true | |
| getSoulcycleSeatmap | GET | `/soulcycle/rooms/{roomId}` | true | |
| getSoulcycleClassBookings | GET | `/soulcycle/reserved_classes` | true | |
| updateSoulcycleClassBooking | PUT | `/soulcycle/reserved_classes/{reservationId}` | true | Body: `{ seat_id }` |
| cancelSoulcycleClassBooking | DELETE | `/soulcycle/reserved_classes/{reservationId}` | true | |
| createSoulcycleBikeReservation | POST | `/soulcycle/v2/bike-reservation` | true | Body includes `seonFingerprint` |
| getFitnessClassBookings | GET | `/fitness/classes/bookings` | true | Query: `brand` |
| getFitnessClassBookingsV2 | GET | `/fitness/classes/bookings` | true | (same path, different response) |
| createFitnessClassBooking | POST | `/fitness/classes/bookings` | true | |
| cancelFitnessClassBooking | POST | `/fitness/classes/bookings/{bookingId}/cancel` | true | |
| postFavoriteFitnessLocation | POST | `/fitness/locations/{locationId}/favorites` | true | |
| deleteFavoriteFitnessLocation | DELETE | `/fitness/locations/{locationId}/favorites` | true | |
| getNearestFitnessRegion | GET | `/fitness/regions/nearby` | true | Query params |
| getFitnessLocations | GET | `/fitness/brands/{brand}/regions/{regionId}/nearestlocations` | true | |
| getFitnessClassSeatMap | GET | `/fitness/classes/{biltClassId}/seatMap` | true | |
| getFitnessClassesByMerchantCatalogId | GET | `/fitness/studio/{merchantCatalogId}/classes` | true | |
| getRawFitnessClassesByLocation | GET | `/fitness/locations/{biltLocationId}/classes` | true | |

#### 8. HSA/FSA API
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| getAccountBalance | GET | `/hsa-fsa/balance` | true | |
| getOnboardedUser | GET | `/hsa-fsa/users/{userId}` | true | |
| getListTpas | GET | `/hsa-fsa/tpas` | true | |
| upsertUser | POST | `/hsa-fsa/users/{userId}` | true | |
| submitSilverMigration | POST | `/hsa-fsa/users/{userId}/silver-migration` | true | |
| unlinkUser | DELETE | `/hsa-fsa/users` | true | |

#### 9. Knot API
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| getMerchantList | POST | `/knot/fetch/merchant-list` | true | |
| getSession | POST | `/knot/session` | true | |

#### 10. Leases API
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| getHouseholdInformationByLeaseId | GET | `/lease/{leaseId}/household` | true | |
| getResidentConnections | GET | `/lease/{leaseId}/resident-connections` | true | |

#### 11. Liabilities API
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| getLiabilities | GET | `/liabilities` | true | Query params (array format) |
| payLiability | POST | `/wallet/liabilities/{accountId}/payments` | true | |
| getOpenLiabilityPayments | GET | `/liabilities/payments/open` | true | |

#### 12. Location API
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| getLocationDetails | POST | `/neighborhoods/search` | true | |
| getNearbyNeighborhoods | POST | `/neighborhoods/search/nearby` | true | |
| getReverseGeocode | POST | `/geocode/reverse` | true | |
| getForwardGeocode | POST | `/geocode/forward` | true | |
| getSearchAddressAutocomplete | POST | `/search/address/autocomplete` | true | |
| getGeocodeLocation | GET | `/geocode/location/{locationId}` | true | Query: `sessionToken`, `locationProvider`, `enrichmentVersion` |

#### 13. Offers API
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| getOffers | POST | `/benefits/offers/display` | true | |
| checkFeatureFlags | POST | `/benefits/features` | true | |
| registerUserLocation | POST | `/benefits/location/register` | true | |
| getReasonForMerchants | POST | `/benefits/merchants/reason` | true | |
| getScreenPlacements | GET | `/benefits/screens/{screenId}` | true | |
| getMultipleScreenPlacements | GET | `/benefits/screens` | true | Query: `screenIds` (multiple) |
| getCompositeDisplay | POST | `/benefits/composite/display` | true | |

#### 14. Payments API
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| getPostbackUrlForPayment | GET | `/payment/rent/{id}/post-back-url` | true | Query: `basePostBackUrl` |
| getPaymentHistory | GET | `/payment/history/v2` | true | Query params (startDate, endDate) |
| getCurrentExpectedAiPayment | GET | `/payment/expected-ai-payment/latest` | true | |
| getExpectedAiPaymentHistory | GET | `/payment/expected-ai-payment/history` | true | Query: `includeReceived` |
| getPaymentLatestRequestingConfirmation | GET | `/payment/history/latest-requesting-confirmation` | true | |
| cancelRentPayment | DELETE | `/payment/rent/{id}` | true | |
| cancelCheckPayment | DELETE | `/payment/{id}/check` | true | |
| confirmPayment | POST | `/payment/{id}/confirmation` | true | Body: `{ pointsToRedeem }` |
| declareFraud | POST | `/payment/{id}/fraud-declaration` | true | Body: `{ explanation }` |
| getPaymentAuthorization | GET | `/payment/authorization/latest` | true | |
| getPaymentAuthorizationHistory | GET | `/payment/authorization/history` | true | Query params |
| createPaymentAuthorization | POST | `/payment/authorization/latest` | true | Body: `{ authorizedAmount, authorizationFlowType }` |
| updatePaymentAuthorization | PATCH | `/payment/authorization/latest` | true | Body similar |
| deletePaymentAuthorization | DELETE | `/payment/authorization/latest` | true | |
| getBillPayStatusForUser | GET | `/user-bank/bill-pay-status` | true | |
| getRecurringPaymentAuthorization | GET | `/payment/authorization/recurring` | true | |
| createRecurringPaymentAuthorization | POST | `/payment/authorization/recurring` | true | |
| updateRecurringPaymentAuthorization | PATCH | `/payment/authorization/recurring` | true | |
| deleteRecurringPaymentAuthorization | DELETE | `/payment/authorization/recurring` | true | |
| getMastercardOfferUpsell | GET | `/payment/offer/mastercard` | true | |
| addThirdPartyCard | POST | `/third-party-card` | true | |
| getRentFeeSchedule | GET | `/payment/rent/fee-schedule` | true | Query: `chargeAmount` |
| getCheckShippingFeeSchedule | GET | `/payment/check/fee` | true | |
| getResidenceCheckShippingFeeSchedule | GET | `/payment/check/fee` | true | |

#### 15. Pharmacy API
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| getTransactions | GET | `/pharmacy/transactions` | true | Query: `allClaimStatuses` |
| getTransactionById | GET | `/pharmacy/transactions/{id}` | true | |
| searchPharmacies | GET | `/public/merchants` | false | Query: `merchantType=pharmacy`, `latitude`, `longitude`, etc. |
| postTransaction | POST | `/pharmacy/hsafsa/transfers/transactions` | true | |
| postHipaaConsent | POST | `/pharmacy/hipaa/consent` | true | |
| getHipaaConsent | GET | `/pharmacy/hipaa/consent` | true | |
| deleteHipaaConsent | DELETE | `/pharmacy/hipaa/consent` | true | |
| getHistoricalItems | GET | `/pharmacy/hsafsa/eligible-items` | true | |
| postHistoricalItems | POST | `/pharmacy/hsafsa/orders` | true | |
| createHsaFsaClaimTransaction | POST | `/pharmacy/hsafsa/claim/transactions` | true | |
| getAutoClaimSubmissionConsent | GET | `/pharmacy/automatic-claims/consent` | true | |
| createAutoClaimSubmissionConsent | POST | `/pharmacy/automatic-claims/consent` | true | Body includes `type`, `hsaFsaCardId` |
| revokeAutoClaimSubmissionConsent | DELETE | `/pharmacy/automatic-claims/consent` | true | Query: `type` |
| getItemsEligibleForAutomaticHealthcareSavings | GET | `/pharmacy/automatic-claims/eligible-items` | true | |
| createHsaFsaClaimTransactionV2 | POST | `/pharmacy/v2/hsafsa/claim/transactions` | true | Includes `requestId` |

#### 16. Rakuten API
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| postRakutenCookie | POST | `/cookies/rakuten` | true | |
| getRakutenCookie | GET | `/cookies/rakuten` | true | Query: `user_id` |

#### 17. Rent Credit Reporting API
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| uploadUrl | PUT | `/rent-credit-reporting/lease/upload-url` | true | Query: `replace` |
| extractLease | POST | `/rent-credit-reporting/lease/extractor` | true | |
| getTransactions | POST | `/home-history/user-payment-verification/transactions/search` | true | Body with `legacyRentCreditReportId`, `bankAccountId`, etc. |
| verifyTransactions | POST | `/home-history/user-payment-verification/transactions/verify` | true | |
| getPaymentVerificationFlow | POST | `/home-history/flows` | true | |
| updateMonthlyRent | POST | `/home-history/user-payment-verification/update-contract` | true | |
| paymentAmendment | POST | `/home-history/user-payment-verification/payment-amendment` | true | |

#### 18. Restaurant Queries
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| getCities | GET | `/public/merchant/cities` | true | Query: `latitude`, `longitude` |
| getRestaurants | GET | `/public/merchants` | true | Many query params: `city_id`, `exclusive`, `query`, `cuisine`, `price_rating`, `sort_by`, `page`, `size`, `latitude`, `longitude`, `party_size`, `datetime`, `houseAccountEligible`, `radius` |
| getRestaurantById | GET | `/public/merchants/{id}` | true | Query: `datetime`, `party_size`, `latitude`, `longitude`, `useCatalogId` |
| getFilters | GET | `/public/merchant/filters` | true | Query: `page`, `size`, `category`, `city_id`, `latitude`, `longitude` |
| getMerchantNetworkEnrollment | GET | `/neighborhood-rewards-enrollment` | true | |
| postMerchantNetworkEnrollment | POST | `/neighborhood-rewards-enrollment/card` | true | |

#### 19. Surveys API
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| loadSurvey | POST | `/surveys/{surveyId}/load` | true | |
| answerQuestion | POST | `/surveys/responses/{responseId}/questions/{questionId}` | true | Body: `answerValue`, `comment` |
| getSurveyStatus | GET | `/surveys/{surveyId}` | true | |

#### 20. SVG Icon API
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| getSvg | GET | `/{svgPath}` | false? | Base URL "/", returns SVG text. Auth not specified. |

#### 21. User Agreement API
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| getFeatures | GET | `/feature/subscription` | true | |
| enableFeature | POST | `/feature/{featureId}/subscription` | true | |
| disableFeature | DELETE | `/feature/{featureId}/subscription` | true | |

#### 22. Virtual Account API
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| getVirtualAccount | GET | `/user-bank/v2/vaccount` | true | |
| getVirtualAccountConfig | GET | `/user-bank/v2/vaccount/config` | true | |
| createVirtualAccount | POST | `/user-bank/v2/vaccount` | true | Body includes address, etc. |
| updateVirtualAccount | PATCH | `/user-bank/v2/vaccount` | true | |

#### 23. Dining (POS) Async Thunks
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| getDiningOrderDetails | GET | `/pos/order/{orderId}` | true | |
| getDiningChecks | GET | `/pos/order/{orderId}/check` | true | |
| getDiningOrders | GET | `/pos/order` | true | Query: `memberId` |
| getDiningVisits | GET | `/pos/visits` | true | Query: `visitStatus=ONGOING&visitStatus=UPCOMING` |
| getVisitById | GET | `/pos/visits/{visitId}` | true | |
| getVisitById (public) | GET | `/public/pos/visits/{visitId}` | true | |
| makePaymentV2 | POST | `/pos/order/{orderId}/check/{checkId}/payment` | true | Complex body |
| getBonusPoints | POST | `/merchant/{merchantId}/points` | true | |
| postMatchTable | POST | `/pos/order/match` | true | Body: `tableId`, `reservationId` |
| exchangePayForGuestToken | POST | `/public/pos/visits/{visitId}/pay-for-guest/exchange-token` | false | |
| payForGuest | POST | `/public/pos/visits/{visitId}/pay-for-guest` | false | |

#### 24. Loyalty Experience Endpoints (fS)
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| getExperienceInventory | GET | `/loyalty/experiences/{name}` | true | |
| getExperienceUserEntry | GET | `/loyalty/experiences/{name}/entries` | true | |
| subscribeUserToExperience | POST | `/loyalty/event/{name}/subscribe` | true | |
| claimExperience | POST | `/loyalty/event/{name}` | true | Body includes `claimAmount`, `paymentDetails`, etc. |
| unsubscribeUserFromExperience | POST | `/loyalty/event/{name}/unsubscribe` | true | |
| cancelExperienceEntry | PUT | `/loyalty/event/{name}/cancel` | true | |

#### 25. Loyalty User Endpoints (hr)
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| getLoyaltyUserBasic | GET | `/loyalty/user/basic` | true | |
| getLoyaltyUser | GET | `/loyalty/user` | true | |
| postCouponCode | POST | `/loyalty/coupon-code` | true | |
| getFilteredPointActivity | GET | `/loyalty/user/reward/activity` | true | Query: `month`, `year`, `category`, `state`, `start`, `end` |
| getPointsActivity | GET | `/loyalty/activity` | true | Query: `month`, `year`, `start`, `end` |
| getConversionRate | GET | `/loyalty/points/conversion-rate` | true | Query: `dollarAmount`, `pointAmount`, `useCase`, `partnerId` |
| getAddititonalPointsEarned | GET | `/public/loyalty/points/earn` | false | Query: `dollarAmount`, `useCase` |
| getRentCreditTotal | GET | `/loyalty/credits/rent` | true | |
| getPartnerLoyaltyId | POST | `/loyalty/partner/discover` | true | |
| discoverPartnerLoyaltyId | POST | `/loyalty/partner/discover` | true | |
| linkPartnerLoyaltyId | POST | `/loyalty/partner/link` | true | |
| optOutPartnerLoyaltyId | POST | `/loyalty/partner/opt-out` | true | |

#### 26. Application Fees (Student Housing)
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| getStudentHousingCharges | GET | `/student-housing/charges` | true | Query: `propertyManagerId`, `externalPropertyId`, `externalApplicationNo`, `externalUserId` |
| initiateApplicationPayment | POST | `/student-housing/payment` | true | Body includes `creditCardId`, `charges`, etc. |

#### 27. Autopay (Alliance Property)
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| createAutopayConfig | POST | `/property/rentals/autopay` | true | |
| listAutopayConfigs | GET | `/property/rentals/autopay` | true | Query: `leaseId` |
| getAutopayConfigById | GET | `/property/rentals/autopay/{id}` | true | |
| deleteAutopayConfigByLease | DELETE | `/property/rentals/autopay` | true | Query: `leaseId` |
| deleteAutopayConfigById | DELETE | `/property/rentals/autopay/{id}` | true | |

#### 28. Browser Orchestration (Bos)
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| createBosConnection | POST | `/bos/connection` | true | |
| getBosUserInfo | GET | `/bos/user-info` | true | |
| getBosWorkflowStatus | GET | `/bos/workflow-status` | true | Query: `workflow_name`, `invocation_id` |

#### 29. Barrys Fitness
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| getSeatMap | GET | `/fitness/classes/{biltClassId}/seatMap` | true | |
| getAddons | GET | `/fitness/classes/{biltClassId}/addOns` | true | |

#### 30. Maintenance (Service Requests)
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| getRequests | GET | `/property/{propertyUuid}/service-requests` | true | Query: `unitUuid`, `limit`, `offset` |
| createRequest | POST | `/property/{propertyUuid}/service-requests` | true | Body includes `unitUuid`, `briefDescription`, etc. |
| updateRequest | PATCH | `/property/{propertyUuid}/service-requests/{id}` | true | Body: `internalStatus` |
| getRequestById | GET | `/property/{propertyUuid}/service-requests/{id}` | true | |
| getCustomFields | GET | `/property/{propertyUuid}/service-requests/custom-fields` | true | |
| getAttachmentUploadUrl | POST | `/property/{propertyUuid}/service-requests/{id}/attachments/upload-url` | true | Body: `fileName` |
| getAttachments | GET | `/property/{propertyUuid}/service-requests/{id}/attachments` | true | |
| getAttachmentDownloadUrl | GET | `/property/{propertyUuid}/service-requests/{id}/attachments/{attachmentId}` | true | |
| deleteAttachment | DELETE | `/property/{propertyUuid}/service-requests/{id}/attachments/{attachmentId}` | true | |
| uploadFile | PUT | (signedUrl) | false | Uses fetch to upload to GCS |
| updateServiceRequestStatus | PATCH | `/property/{propertyUuid}/service-requests/{id}` | true | |

#### 31. Packages
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| getPackages | GET | `/properties/{propertyUuid}/residents/{residentId}/packages` | true | Query: `page`, `pageSize` |

#### 32. Rent Reporting (Opt-in)
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| getPrefillDataForOptIn | GET | `/rent-credit-reporting/user/opt-in/prefill` | true | |
| getAppUserRentReportHistory | GET | `/rent-credit-reporting/history` | true | Query: `rentCreditReportingId` |
| getAppUserPaymentHistory | POST | `/rent-credit-reporting/payment/history` | true | |
| optInForRentCreditReport | POST | `/rent-credit-reporting/user/opt-in` | true | |
| getAllUserOptInRequestInfo | GET | `/rent-credit-reporting/user/opt-in` | true | |
| updateOptInRequestInfo | PUT | `/rent-credit-reporting/user/opt-in` | true | |
| deleteOptInRequestInApp | DELETE | `/rent-credit-reporting/user/opt-in/{id}` | true | |

#### 33. Third-Party Cards (Fee)
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| getFeeSchedule | GET | `/payment/rent/fee-schedule` | true | Query: `chargeAmount` |
| getFeePreview | POST | `/payment/rent/fee-preview` | true | Body: `cardId`, `chargeAmount`, `bankAccountId`, `isBiltCard` |

#### 34. Virtual Account (Legacy)
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| getVirtualAccountWithFraudCheck | GET | `/user-bank/vaccount` | true | |
| provisionCheckSubledgerAccount | POST | `/user-bank/physical-check-account` | true | Body includes address |
| craeteVirtualAccount | POST | `/user-bank/v2/vaccount` | true | |
| getVirtualAccount | GET | `/user-bank/v2/vaccount` | true | |
| getVirtualAccountConfig | GET | `/user-bank/v2/vaccount/config` | true | |
| updateVirtualAccount | PATCH | `/user-bank/v2/vaccount` | true | |

#### 35. Wells Fargo Statement Credit
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| getStatementCreditHistory | GET | `/wf/statement-credit` | true | |
| submitStatementCreditRequest | POST | `/wf/statement-credit` | true | |

#### 36. FusionAuth Logout
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| wwwLogout | POST | `/public/identity/auth/logout` | true | |

#### 37. Exchange Engage Token
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| exchangeEngageToken | POST | `/loyalty/exchange-engage-token` | true | Body: `dataExchangeToken`, `biltDeviceInformation` |

#### 38. All Bookings
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| getAllBookings | GET | `/merchants/bookings` | true | |

#### 39. Rent Payment Initiation
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| initiateRentPayment | POST | `/payment/rent` | true | Body includes `amount`, `requestId`, `paymentMethodType`, etc. |
| getTenantLedger | GET | `/payment/tenant-ledger` | true | Query: `forceLiveFetch` |
| getPaymentHistory | GET | `/payment/history/v2` | true | Query: `startDate`, `endDate` |

#### 40. RentFree Games
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| getUserSubmissions | GET | `/rentday/rentfree/{game}/user/submissions` | true | |
| submitUserAnswers | POST | `/rentday/rentfree/{game}/user/submissions` | true | |
| getUserResult | GET | `/rentday/rentfree/{game}/user/result` | true | |
| getCorrectAnswers | GET | `/rentday/rentfree/{game}/answers` | true | |
| claimAward | PUT | `/rentday/rentfree/{game}/user/claim` | true | |
| updateNotificationPreference | PUT | `/rentday/rentfree/{game}/user/notification-preference` | true | |
| getNotificationPreference | GET | `/rentday/rentfree/{game}/user/notification-preference` | true | |

#### 41. RentDay Matchups
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| castVote | POST | `/rentday/matchups/vote` | true | Body: `votes` |
| getUserVotes | POST | `/rentday/user/votes` | true | Body: `matchUpIds` |
| getMatchUps | POST | `/rentday/matchups` | true | Body: `matchUpIds` |

#### 42. In-Network Auth
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| authInNetworkResident | POST | `/public/auth-inn-resident` or `/api/bilt/public/auth-inn-resident` | false? | Public, used for PWB login |
| authInNetworkResidentVerifyOtp | PUT | same path | false? | |

#### 43. SoulCycle (additional)
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| fetchClassDetails | GET | `/soulcycle/class/{classId}` | true | |
| fetchClasses | GET | `/soulcycle/fetch-classes` | true | |
| fetchRegions | GET | `/soulcycle/fetch-regions` | true | |
| fetchStudiosInRegion | GET | `/soulcycle/fetch-studios` | true | Query: `regionId` |
| getLinkedAccount | GET | `/soulcycle/get-linked-account` | true | |
| linkAccount | PUT | `/soulcycle/link-account` | true | Body: `authorizationCode`, `redirectUri` |
| unlinkAccount | POST | `/soulcycle/unlink-account` | true | |
| getRiderTermsAndConditions | GET | `/soulcycle/legal/{regionId}/rider-terms-and-conditions` | true | |
| getSCTermsAndConditions | GET | `/soulcycle/legal/{regionId}/terms-and-conditions` | true | |
| reserveBike | POST | `/soulcycle/v2/bike-reservation` | true | Includes `seonFingerprint` |
| cancelBikeReservation | DELETE | `/soulcycle/reserved_classes/{reservationId}` | true | |
| updateBooking | PUT | `/soulcycle/reserved_classes/{reservationId}` | true | Body: `seat_id` |
| getReservedSeats | GET | `/soulcycle/class/{classId}/reserved-seats` | true | |
| getSeatMap | GET | `/soulcycle/rooms/{roomId}` | true | |
| getRentDayBikes | GET | `/soulcycle/class/{classId}/rent-day-rides` | true | |
| getBookedClasses | GET | `/soulcycle/reserved_classes` | true | |

#### 44. Map Search
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| searchMerchants | GET | `/v1/catalog/merchants/search` | true | Many query params (bounding box, radius, category, etc.) |

#### 45. Geolocation
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| getGeolocation | GET | `/user/geolocation` | true | |

#### 46. Partner Login (Third-Party JWT)
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| partnerLogin | POST | `/public/auth/third-party-jwt/{thirdPartyName}/login` | false | Body: `token` |

#### 47. User Credits
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| getUserCredits | GET | `/loyalty/credits/instances` | true | Query: `category=1` |

#### 48. Deck (External Connections)
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| validateURL | POST | `/external/deck/validate-url` | true | |
| createConnection | POST | `/external/deck/connection` | true | |
| createConnectionOTP | POST | `/external/deck/connection/otp` | true | |
| getPayment | GET | `/external/deck/payment` | true | |
| createPayment | POST | `/external/deck/payment` | true | |
| getConnectionStatus | GET | `/external/deck/{jobId}/session-status` | true | Query: `completedOnly` |
| getUserConnectionInfo | GET | `/external/deck/account/status` | true | |
| answerMfa | POST | `/external/deck/mfa` | true | |
| updateConnection | PUT | `/external/deck/user/credential` | true | |
| deleteConnection | DELETE | `/external/deck/user/credential` | true | |
| refreshConnection | GET | `/external/deck/balance` | true | |
| recordUrl | POST | `/external/deck/url` | true | |

#### 49. Bilt Cash
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| getBiltCash | GET | `/bilt-cash` | true | Query params |
| getBulkEligibility | GET | `/bilt-cash/eligibility/bulk` | true | Query params |
| getCheckoutSettings | GET | `/unit/{unitId}/checkout-settings` | true | |
| createCheckoutSettings | POST | `/unit/{unitId}/checkout-settings` | true | |
| patchCheckoutSettings | PATCH | `/unit/{unitId}/checkout-settings` | true | |
| deleteCheckoutSettings | DELETE | `/unit/{unitId}/checkout-settings` | true | |

#### 50. Seated (Restaurants) Async
| Endpoint | Method | Path | Auth | Notes |
|----------|--------|------|------|-------|
| getGeolocation | GET | `/user/geolocation` | true | (duplicate) |
| fetchSeatedRestaurants | (dispatches getRestaurants) | - | - | |

---

### Security Vulnerabilities & Observations

#### 1. **Insecure Storage of Sensitive Data in LocalStorage**
   - **Auth Tokens**: The `auth` slice (accessToken, idToken, refreshToken) is persisted in localStorage (see `I0`). This makes tokens vulnerable to XSS attacks. An attacker who can inject JavaScript can steal tokens and impersonate users.
   - **PII in SignUp**: `signUp` slice stores email, phone, first name, last name, date of birth in localStorage (see `IK`). This violates data privacy regulations (e.g., GDPR) and exposes users to identity theft.
   - **Amazon Linking Token**: `amazonLinking` slice stores `authorizationToken` (from exchangeEngageToken) in localStorage (see `Iq`). This token could be used to link Amazon accounts.
   - **PayWithBilt Tokens**: `payWithBilt` slice stores `verificationToken`, `verificationTokenForPhone`, `tenantInfo` in localStorage (see `I2`).
   - **Redirect URLs**: `ui` slice stores `loginRedirectUrl` in localStorage (see `IJ`). An attacker could modify this to cause open redirects.
   - **PostBack URL**: `temp` slice stores `postBackUrl` in localStorage (see `IX`). Could be used for open redirect.

   **Recommendation**: Move sensitive tokens to `sessionStorage` or HTTP-only cookies. Avoid storing PII in client-side storage.

#### 2. **Potential IDOR in URL Path Parameters**
   - Many endpoints use resource IDs in the URL path (e.g., `/user-bank/account/{id}`, `/payment/rent/{id}`, `/property/{propertyUuid}/service-requests/{id}`, `/soulcycle/class/{classId}`, `/pharmacy/transactions/{id}`, `/cookies/rakuten?user_id=`). If the server does not verify that the authenticated user owns the resource, an attacker could manipulate the ID to access or modify other users' data.
   - Example: `editBankAccount` (PATCH `/user-bank/account/{id}`) could allow an attacker to change another user's bank account details by supplying a different `id`.
   - Example: `getConversation` (GET `/bilt-concierge/conversations/{conversationId}/messages`) – guessing conversation IDs could leak private messages.

   **Recommendation**: Implement strict ownership checks on the server for all endpoints with user-supplied IDs.

#### 3. **Potential Open Redirect in Redirect Functions**
   - `redirectNewUser` (in file 347544) constructs a redirect URL using `paramCallback` (from `_U.buildCallbackHost`) and user-supplied parameters (email, phoneNumber, contactVerificationToken). If `paramCallback` is not validated and can be controlled by an attacker (e.g., via query parameters), this could lead to open redirect to malicious sites.
   - The login function `_w` redirects to URLs based on query parameters; if an attacker can control the `redirectPath`, they could cause open redirect.
   - The `useRToken` hook removes `rtoken` from the URL and replaces the path; this seems safe but should be reviewed for any manipulation.

   **Recommendation**: Validate that the redirect domain is whitelisted. Avoid using user input directly in redirect URLs.

#### 4. **Insecure Direct Object References in Query Parameters**
   - `getRakutenCookie` uses `user_id` in query string. If the server does not verify that the requesting user matches that `user_id`, it could leak cookie data.
   - `getMultipleScreenPlacements` uses `screenIds` array in query. If screen IDs are enumerable, an attacker could fetch arbitrary screen data.
   - `getStudentHousingCharges` accepts `externalUserId` and `externalPropertyId` in query. Without proper authorization, an attacker could retrieve charges for other users.

   **Recommendation**: Ensure that query parameters that reference resources are validated against the authenticated user.

#### 5. **Lack of Rate Limiting on Sensitive Endpoints**
   - Many mutation endpoints (e.g., OTP sending, payment initiation, account closure) do not appear to have client-side rate limiting. If the server lacks rate limiting, an attacker could brute-force OTPs, cause financial abuse, or perform DoS.

   **Recommendation**: Implement rate limiting on all sensitive endpoints, especially those involving payments, authentication, and data modification.

#### 6. **CSRF on Cookie-Based Authentication (if any)**
   - Most requests use `auth: !0` which likely adds an `Authorization: Bearer` header. This is not automatically sent by browsers, so CSRF is not an issue for those endpoints. However, if any endpoints rely on session cookies (e.g., for `wwwLogout`), they might be vulnerable. The logout endpoint uses POST and may be protected by CORS or anti-CSRF tokens, but it's worth verifying.

   **Recommendation**: Use anti-CSRF tokens for any cookie-based endpoints.

#### 7. **Sensitive Data Exposure in URL Parameters**
   - GET requests sometimes include sensitive data in query strings (e.g., `pointAmount`, `userId`, `verificationToken`). These can be logged by proxies, browsers, or servers, and may appear in browser history.

   **Recommendation**: Use POST requests for sensitive data, or encrypt parameters.

#### 8. **Missing Authentication on Some Endpoints**
   - `searchPharmacies` has `auth: !1`, which is appropriate for public search. However, we should verify that no internal endpoints are accidentally public. For example, `getAddititonalPointsEarned` is public (GET `/public/loyalty/points/earn`). That's fine.

   **Recommendation**: Regularly audit endpoints to ensure they have correct authentication.

#### 9. **GraphQL Introspection and Abuse**
   - The app uses GraphQL (e.g., `PartnerSignupQuery`). If the GraphQL endpoint is not properly secured, it could allow introspection, exposing the entire schema. Additionally, deeply nested queries could cause DoS.

   **Recommendation**: Disable introspection in production, implement query depth limiting, and use authentication.

#### 10. **Insecure Handling of reCAPTCHA**
    - `addManualBankAccount` includes `recaptchaInfo` in the request. If the server does not validate the reCAPTCHA token properly, an attacker could bypass it.

    **Recommendation**: Ensure server-side validation of reCAPTCHA tokens.

#### 11. **Logging of Sensitive Data**
    - The code uses `console.error` in some catch blocks (e.g., in `getAddressAutocomplete`). While not shown extensively, if these logs are exposed in production, they could leak sensitive data.

    **Recommendation**: Disable console logging in production.

#### 12. **Potential XSS in React Components**
    - The code includes JSX with user-provided data (e.g., `_N.Body.Md` children with `i`). If any of these values are not properly escaped, XSS could occur. However, React escapes by default, so this is less likely. Still, we should check for `dangerouslySetInnerHTML` usage (not seen).

    **Recommendation**: Ensure all user input is properly escaped and avoid `dangerouslySetInnerHTML`.

#### 13. **Insecure Randomness for Request IDs**
    - The app uses `(0, pz.default)()` (likely `uuid`) for request IDs. This is fine.

#### 14. **Weak Password Policy?**
    - Not applicable as passwordless login is used.

---

### Important Functions for Bug Hunters

| Function | Location | Description | Potential Attack Vector |
|----------|----------|-------------|-------------------------|
| `useRToken` | file 28575 | Handles `rtoken` from URL, validates, and processes connection. | If `rtoken` validation is flawed, could allow unauthorized unit connections. |
| `handleInitiateOTPLogin` | file 987674 | Initiates OTP login, checks in-network attempts. | Could be used to bypass in-network checks. |
| `redirectNewUser` | file 347544 | Redirects new users after signup/login. | Open redirect via `paramCallback`. |
| `_w` (login redirect) | file 446751 | Performs login redirect with PKCE. | Open redirect if `redirectPath` is controlled. |
| `exchangeEngageToken` | file 446751 | Exchanges a token for Amazon linking. | Token could be stolen if leaked. |
| `logout` (Ea) | file 446751 | Clears storage and cookies. | Improper clearing could leave sensitive data. |
| `getFingerprintHeader` | used in many places | Adds `X-Seon-Fingerprint` header. | If fingerprint is not validated, could be spoofed. |

---

### Conclusion
The application exposes a large number of API endpoints, many of which handle sensitive user data. The most critical issues are:

1. **Persistent storage of authentication tokens and PII in localStorage** – high risk of theft via XSS.
2. **Potential IDOR vulnerabilities** due to insufficient server-side authorization checks on resource IDs.
3. **Potential open redirect** in redirect functions that use user-controlled parameters.
4. **Exposure of sensitive data in URL query strings** and localStorage.

These issues should be prioritized for remediation. Additionally, all endpoints should be reviewed for proper authorization and rate limiting.