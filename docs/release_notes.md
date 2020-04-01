# <a name="release_notes"></a>PoS API Release Notes

The Point of Sale API V10 will be in production **Q2 2020**. It will be released in SandProd **Q1 2020**.

## Changelog

### 2020-03-27

- Added 403 in the API Errors section as a possible non-successful status-code when creating a Point of Sale.

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
