# <a name="release_notes"></a>PoS API Release Notes

The Point of Sale API V10 is now in production.

## Changelog

### 2020-09-08
- Removed Integrator ID from [Self Certification](self_certification)
- Updated screenshots in [Self Certification](self_certification)

---
### 2020-09-02
- Changed and updated how Refunds is planned to work. See description in [Payment Flows](payment_flows#refunds). Release of Refunds are planned to Ultimo September.

---
### 2020-09-02
- Clarified how to choose a beaconId in [Best Practices](best_practices#choosing_a_beaconid)  

---
### 2020-08-19
- Clarified the requirements for using long lived access tokens in [Authentication](pos_integratorauthentication)  
- Reduced the number of test cases in Self Certification by e.g. having more than one error or timeout scenario in one test case. Categories remain the same and this does not require a re-certification since this has not changed what is being tested.
- Updated picture of Onboarding cases in [Self Certification](self_certification)
- Removed x-mobilepay-client-system-name header from table in [Input Formats](input_formats)
- Please do not hesitate to provide feedback

---
### 2020-08-07

- Added a section on [Best Practices](best_practices) regarding Merchant onboarding.
- Moved information regarding Partial Capture from [Best Practices](best_practices) to [Payment Flows](payment_flows#partial_capture) and added some diagrams describing the flow.
- Removed the 403 forbidden statuscode when calling ``POST /payments/{paymentId}/capture`` from [API ERRORS](endpoint_errors)
- Added a 409 conflict statuscode when calling ``POST /payments/{paymentId}/capture`` from [API ERRORS](endpoint_errors) that descibes if a partial capture is attempted on a payment where that is not possible.

---
### 2020-07-13

- Added a 403 error code for payments/{payid}/capture in [API ERRORS](endpoint_errors) which is returned when trying to make a partial capture which is still not supported. When it will be possible to do partial captures it will be listed here in the release notes.

---
### 2020-07-10

- Clarify handling timeout for POST/DELETE/PUT in [API principles](api_principles#error_handling)

---
### 2020-06-29

- Updated payment lifetime in [Payment Flow](payment_flows#payment_flow)

---
### 2020-06-24

- Updated description and added configuration details in [Self Certification](self_certification)

---
### 2020-05-14

- Added error 1182 in [API ERRORS](endpoint_errors)

---
### 2020-05-13

- Added a note on partial capture handling in [Best Practices](best_practices)

---
### 2020-04-28

- Added a note on the possibility of long-lived access tokens for PoS devices

---
### 2020-04-21

- Added 403 in the API Errors section as a possible non-successful status-code when creating a Point of Sale.
- Added a conflict case to the capture payment endpoint in the API Errors section when trying to capture an already captured payment with a different amount.

---

### 2020-03-27

- Dropped the `x-ibm-client-system-name` header and modified the [API principles](api_principles) page to explain how the `x-ibm-client-system-version` header will be used for client versioning.

- Renamed all `X-IBM-*` and `X-MobilePay-*` headers to be lowercase in the documentation (the actual headers are case-insensitive).

---

### 2020-03-26

- Renamed `vat_number` parameter to `merchant_vat` for [Authentication](pos_integratorauthentication) endpoint and changed the format to remove the dash between country code and VAT (e.g., "DK12345678" instead of "DK-12345678"). 

- Removed 'api/' as part of the endpoint route for all endpoints.

---

### 2020-03-25

- Added a *RejectedByMobilePayDueToAgeRestrictions* [payment state](payment_flows#payment_flow_states), to indicate that a payment could not be completed because of age restrictions on the payment. 

- Added section Authentication  [Authentication](pos_integratorauthentication) section so Integrators can use MobilePay APIs, as they'll have to obtain an access_token from the Integrator Authentication service. This way, MobilePay knows who is calling our service.


---

### 2020-03-04

- Added section Handling Timeouts in [API Principles](api_principles) section.

- Removed a 409 errorcode from POST /api/v10/refunds regarding refund amount in the [API Errors](endpoint_errors) section and added an extra one regarding call to POST /api/v10/refunds if payment is not captured.

---

### 2020-03-03

- Renamed header from X-MobilePay-MerchantVatNumber to X-MobilePay-Merchant-VAT-Number in the [Input Formats](input_formats) section.

- Rewrote parameters to camelCase instead of PascalCase in the [Input Formats](input_formats) section to reflect the right casing in the API

---
### 2020-02-27

- Added HTTP StatusCodes and ErrorCodes pr. endpoint in the API. Response codes have also slightly changed in the API on the basis of this documentation so be aware of minor changes to the API as well. See more at [API Errors](endpoint_errors).

- Added header to all endpoints in the API called X-MobilePay-MerchantVatNumber. For more information see [Input Formats](input_formats#HTTP_Headers)

- Fixed wrong header name from X-IBM-ClientId to X-IBM-Client-Id. See [Input Formats](input_formats#HTTP_Headers)

---
### 2020-02-07

- Added documentation on payment restrictions under [Input Formats](input_formats#payment_restrictions) and expanded on best practices regarding payment restrictions [Best Practices](best_practices)

---
### 2020-02-04

- Specified expiration length of Refund reservations to 24 hours. [Refunds](payment_flows#refunds)

---
### 2020-01-31

- Added a description of Self-Certification to the documention under [Self Certification](self_certification)

---

### 2019-12-23

- Changed the month that news will come out regarding self-certification from December 2019 to January 2020

---

### 2019-12-10

- Added `BrandName` to the subpage **Input and Output Formats** in the Brand section

---

### 2019-12-10

- Renamed subpage **Input Formats** to **Input and Output Formats**.
- Adjusted content of subpage **Input and Output Formats**
  - **HTTP Headers**
    - Renamed from `X-MP-\*` to `X-MobilePay-\*`.
    - Removed `X-MP-IntegratorId`.
  - **Payments**
    - Added `PlannedCaptureDelay`, `CustomerToken` and `CustomerReceiptToken`.
