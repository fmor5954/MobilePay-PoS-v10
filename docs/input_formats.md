# <a name="input_formats"></a> Input and Output Formats

This page gives an overview of the format and length restrictions for all input parameters used in the PoS V10 API. 

## <a name="HTTP_Headers"></a> HTTP Headers
For more information about the http headers, see [API principles](api_principles).

| Name | Format      | Description |
|------|-------------|-------------|
| `x-ibm-client-id` | Guid | Identifies an application created through MobilePay Developer Portal. |
| `x-mobilepay-merchant-vat-number` | Valid VAT number:<br>IsoCountryCodeVATNumber<br>Example: DK12345678 | Identifies the merchant the integrator is calling on behalf of |
| `x-mobilepay-client-system-version` | Valid Client-Version:<br>Major.Minor.Build<br>Example: 1.2.1 | Identifies the [version of the integrator system](api_principles#client_identification) calling the API. |
| `x-mobilepay-idempotency-key` | String with at most 36 valid characters | Used to allow calls to be [safely retried](api_principles#error_handling) in case of errors. |

## Brands
For more information about brands, see [PoS Management](pos_management).

| Name | Format      | Description |
|------|-------------|-------------|
| `merchantBrandId` | MPPOSXXXXX <br> POSDKXXXXX <br> POSFIXXXXX | Identifies a Brand in MobilePay. |
| `brandName` | String | The name of the brand. |

## Stores
For more information about stores, see [PoS Management](pos_management).

| Name | Format      | Description |
|------|-------------|-------------|
| `storeId` | Guid | Identifies a Store in MobilePay. |
| `merchantLocationId` | String with exactly 5 valid characters | MobilePay location ID.<br><br>Together with a `merchantBrandId`, this identifies a Store in MobilePay. |

## <a name="poses"></a> Point-of-Sales
For more information about a PoS, see [PoS Management](pos_management).

| Name | Format      | Description |
|------|-------------|-------------|
| `posId` | Guid | Identifies a PoS in MobilePay. |
| `merchantPosId` | String with at most 36 valid characters | Merchant defined PoS ID.<br><br>There can be at most one active PoS with a given `merchantPosId` for a given integrator and merchant. |
| `posName` | String with at most 36 valid characters | Merchant defined PoS name.<br><br>The name is visible in the app, after the customer has checked in on the PoS. |
| `callbackAlias` | String with at most 36 valid characters | Only for clients that use the [notification service](notification_service) to detect MobilePay payments. The `callbackAlias` is a key that identifies which notification endpoint to call for the given PoS. |
| `beaconId` | A GUID or 15 digits | ID of the Beacon.<br><br>In case of physical device such as the MobilePay WhiteBox or a terminal the `beaconId` is a 15 digit string.<br><br>In case of no physical device (QR) the `beaconId` is not provided during PoS creation. MobilePay will generate a string containing a random GUID as the `beaconId`. |
| `supportedBeaconTypes` | `QR` / `NFC` / `BluetoothOther` / `BluetoothMP1` / `BluetoothMP2` / `BluetoothMP3` / `BluetoothMP4`	| Beacon broadcast type.<br><br>Identifies an option for how a customer can check in on a PoS.<br><br>During the creation of a PoS, a list of Beacon Types has to be provided. |
| `calibrationType` | Integer between 0 and 65535 | Calibration Type of a physical beacon.<br><br>This is used by the MobilePay app to know the distance between the mobile device and the phycical beacon before the customer checks in on the PoS.<br><br>This is only applicable if the PoS contains any of the bluetooth `supportedBeaconTypes`. |

## Payments
For more information about payments, see [Payment Flows](payment_flows).

| Name | Format      | Description |
|------|-------------|-------------|
| `paymentId` | GUID | MobilePay defined Payment ID. |
| `orderId` | String with at most 36 valid characters | Merchant defined payment order ID.<br><br>There is no uniqueness requirement for the `orderId`, but it is highly recommended to use unique order IDs. |
| `amount` | Valid positive amount | Total amount of the payment. |
| `currencyCode` | `DKK` / `EUR` | Currency code for the currency of the payment. |
| `merchantPaymentLabel` | String with at most 36 valid characters	| Label for the payment.<br><br>This is a way for the merchant to tag a payment with a label that will be visible in the transaction reporting section on the MobilePay Portal |
| `plannedCaptureDelay` | `None` / `LessThan24Hours` / `LessThan14Days`	| How long time the client expects to wait after receiving a reservation before capturing.<br><br>See [Specify planned capture delay](best_practices#specify-planned-capture-delay). |
| <a name="restrictions"></a>`restrictions` | Json object with one or more parameters | A way to define restrictions on how a payment can be completed. See [Payment Restrictions](input_formats#payment_restrictions) for possible restriction parameters |

### <a name="payment_restrictions"></a>Payment Restrictions

| Name | Format      | Description |
|------|-------------|-------------|
| `debitCardDisallowed` | Boolean | When `debitCardDisallowed` is set to true, debit cards cannot be used for this payment |
| `creditCardDisallowed` | Boolean | When `creditCardDisallowed` is set to true, credit cards cannot be used for this payment |
| `userMinimumAge` | Positive Integer | When `userMinimumAge` is set, MobilePay will cancel the payment if attempted accepted by users younger than the `userMinimumAge` |

## Valid characters

Valid characters for PoS V10 API request fields are:
* 0 - 9
* a-zA-Z
* æÆøØåÅ
* äÄöÖšŠžŽ
* !"#$%&'()*+,-./":;<=>?@[\]^_\`{\|}~
* (space)
