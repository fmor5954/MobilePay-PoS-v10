# <a name="endpoint_errors"></a> API Errors
This page contains information regarding all the non-successful status-codes and different error-codes that can occur in the V10 API. The first section contains all combinations of StatusCodes and ErrorCodes that can occur in all endpoints, and then follows sections for the individual endpoints.

## Common for all endpoints
<details>
  <summary>ALL Endpoints</summary><br>

| StatusCode | ErrorCodes  | Description |
|------------|-------------|-------------|
| 400 | 1099 <br /> 1151`` `` <br> 1152`` `` <br> 1153`` `` <br> 1155`` `` <br> 1156`` `` <br> 1157`` `` <br> 1159`` `` <br> 1160`` `` <br> 1161`` `` <br> 1182`` `` | Unknown BadRequest error <br> Missing ``x-mobilepay-merchant-vat-number`` header <br> Missing ``x-mobilepay-client-system-name`` header <br> Missing ``x-mobilepay-client-system-version`` header <br> Duplicated ``x-mobilepay-merchant-vat-number`` header <br> Duplicated ``x-mobilepay-client-system-name`` header <br> Duplicated ``x-mobilepay-client-system-version`` header <br> Invalid ``x-mobilepay-merchant-vat-number`` header <br> Invalid ``x-mobilepay-client-system-name`` header <br> Invalid ``x-mobilepay-client-system-version`` header <br> Invalid ``merchant_vat`` claim in access token |
| 401 | - | Unauthorized |
| 500 | 2000 - 2999 | Internal server error - Please attach error code when communicating with MobilePay for quicker support |

</details><br>

## Payments
<details>
  <summary>GET /v10/payments/{paymentid} - Query a Payment</summary><br>

| StatusCode | ErrorCodes  | Description |
|------------|-------------|-------------|
| 403 | 1401<br>1411 | Cannot query payments created by a different integrator<br>Cannot query payments created on behalf of a different merchant |
| 404 | - | Payment not found |

</details><br>

<details>
  <summary>GET /v10/payments - Query Payments by a filter</summary><br> 

| StatusCode | ErrorCodes  | Description |
|------------|-------------|-------------|
| 400 | 1109 | Payment filter not specific enough |

</details><br>

<details>
  <summary>POST /v10/payments - Initiate a Payment</summary><br>

| StatusCode | ErrorCodes  | Description |
|------------|-------------|-------------|
| 400 | 1102`` `` <br> 1113`` `` <br> 1117`` `` <br> 1162`` `` <br> 1163`` `` <br> 1164`` `` | Invalid ``Amount`` <br> Invalid ``OrderId`` <br> Invalid ``MerchantPaymentLabel`` <br> Invalid ``x-mobilepay-idempotency-key`` header <br> Duplicated ``x-mobilepay-idempotency-key`` header <br> Missing ``x-mobilepay-idempotency-key`` header |
| 403 | 1400 | Cannot initiate payments on a point of sale created by a different integrator |
| 409 | 1000 <br> 1301 <br> 1306`` `` <br> </p> | Point of Sale not found <br> A payment is already active. Cancel it before starting a new one <br> ``x-mobilepay-idempotency-key`` header has to be unique per request unless the request is a retry of a previous request <br>  |

</details><br>

<details>
  <summary>POST /v10/payments/prepare - Prepare a Payment</summary><br>

| StatusCode | ErrorCodes  | Description |
|------------|-------------|-------------|
| 400 | 1113`` `` <br> 1162`` `` <br> 1163`` `` <br> 1164`` `` | Invalid ``OrderId`` <br> Invalid ``x-mobilepay-idempotency-key`` header <br> Duplicated ``x-mobilepay-idempotency-key`` header <br> Missing ``x-mobilepay-idempotency-key`` header |
| 403 | 1400 | Cannot prepare payments on a point of sale created by a different integrator |
| 409 | 1000 <br> 1301 <br> 1306`` `` <br> </p> | Point of sale not found <br> A payment is already active. Cancel it before starting a new one <br> ``x-mobilepay-idempotency-key`` header has to be unique per request unless the request is a retry of a previous request |

</details><br>

<details>
  <summary>POST /v10/payments/{paymentid}/ready - Ready a Payment</summary><br> 

| StatusCode | ErrorCodes  | Description |
|------------|-------------|-------------|
| 400 | 1102`` `` <br> 1117`` `` | Invalid ``Amount`` <br> Invalid ``MerchantPaymentLabel`` |
| 403 | 1401<br>1406 | Cannot ready payments prepared by a different integrator<br>Cannot ready payments prepared on behalf of a different merchant |
| 404 | - | Payment not found |
| 409 | 1303 | Payment needs to be prepared before it can be marked as ready |

</details><br>

<details>
  <summary>POST /v10/payments/{paymentid}/capture - Capture a Payment</summary><br>

| StatusCode | ErrorCodes  | Description |
|------------|-------------|-------------|
| 400 | 1102`` `` | Invalid ``Amount`` |
| 403 | 1401<br>1407 | Cannot capture payments created by a different integrator<br>Cannot capture payments created on behalf of a different merchant |
| 404 | - | Payment not found |
| 409 | 1304 <br> 1305 <br> 1307 <br> 1308`` `` | Cannot capture payment when payment is not reserved <br> Capture ``Amount`` cannot exceed the reserved amount <br> Payment has already been captured with a different amount <br> Partial capture not possible on this payment |

</details><br>

<details>
  <summary>POST /v10/payments/{paymentid}/cancel - Cancel a Payment</summary><br>

| StatusCode | ErrorCodes  | Description |
|------------|-------------|-------------|
| 403 | 1401<br>1408 | Cannot cancel payments created by a different integrator<br>Cannot cancel payments created on behalf of a different merchant |
| 404 | - | Payment not found |
| 409 | 1300 | The payment cannot be cancelled in the current state |

</details><br>

## Point of Sales
<details>
  <summary>GET /v10/pointofsales/{posid} - Query a Point of Sale</summary><br>

| StatusCode | ErrorCodes  | Description |
|------------|-------------|-------------|
| 403 | 1400<br>1410 | Cannot query point of sales created by a different integrator<br>Cannot query point of sales created on behalf of a different merchant |
| 404 | - | Point of sale not found |

</details><br>

<details>
  <summary>GET /v10/pointofsales - Query Point of Sales by a filter</summary><br>

| StatusCode | ErrorCodes  | Description |
|------------|-------------|-------------|
| 400 | 1121 | Point of sale filter not specific enough |

</details><br>

<details>
  <summary>GET /v10/pointofsales/{posid}/checkin - Query a checkin on a Point of Sale</summary><br>

| StatusCode | ErrorCodes  | Description |
|------------|-------------|-------------|
| 403 | 1400 | Cannot query checkin on a point of sale created by a different integrator |
| 404 | - | Point of sale not found |

</details><br>

<details>
  <summary>POST /v10/pointofsales - Create a Point of Sale</summary><br>

| StatusCode | ErrorCodes  | Description |
|------------|-------------|-------------|
| 400 | 1100`` `` <br> 1111`` `` <br> 1112`` `` <br> 1116`` `` <br> 1118`` `` <br> 1162`` `` <br> 1163`` `` <br> 1164`` `` | Invalid ``BeaconId`` <br> Invalid ``MerchantPosId`` <br> Invalid ``PosName`` <br> Invalid ``CallbackAlias`` <br> Invalid ``CalibrationType`` <br> Invalid ``x-mobilepay-idempotency-key`` header <br> Duplicated ``x-mobilepay-idempotency-key`` header <br> Missing ``x-mobilepay-idempotency-key`` header |
| 403 | 1403 | Cannot create point of sale on store that does not belong to the merchant |
| 409 | 1002 <br> 1200`` `` <br> 1202`` `` <br> 1306`` `` <br> </p> | Store not found <br> A point of sale with that ``MerchantPosId`` already exist <br> A point of sale with that ``BeaconId`` already exist <br> ``x-mobilepay-idempotency-key`` header has to be unique per request unless the request is a retry of a previous request |

</details><br>

<details>
  <summary>DELETE /v10/pointofsales/{posid} - Delete a Point of Sale</summary><br>

| StatusCode | ErrorCodes  | Description |
|------------|-------------|-------------|
| 403 | 1400<br>1409 | Cannot delete point of sales created by a different integrator<br>Cannot delete point of sales created on behalf of a different merchant |
| 404 | - | Point of sale not found |

</details><br>

## Refunds
<details>
  <summary>GET /v10/refunds/{refundid} - Query a Refund</summary><br>

| StatusCode | ErrorCodes  | Description |
|------------|-------------|-------------|
| 403 | 1402 | Cannot query refunds created by a different integrator |
| 404 | - | Refund not found |

</details><br>

<details>
  <summary>GET /v10/refunds - Query Refunds by a filter</summary><br>

| StatusCode | ErrorCodes  | Description |
|------------|-------------|-------------|
| 400 | 1110 | Refund filter not specific enough |

</details><br>

<details>
  <summary>POST /v10/refunds - Create a Refund</summary><br>

| StatusCode | ErrorCodes  | Description |
|------------|-------------|-------------|
| 400 | 1102`` `` <br> 1114`` `` <br> 1162`` `` <br> 1163`` `` <br> 1164`` `` | Invalid ``Amount`` <br> Invalid ``RefundOrderId`` <br> Invalid ``x-mobilepay-idempotency-key`` header <br> Duplicated ``x-mobilepay-idempotency-key`` header <br> Missing ``x-mobilepay-idempotency-key`` header |
| 403 | 1401 | Cannot refund payments created by a different integrator |
| 409 | 1001 <br> 1306`` `` <br> <br> 1354 <br> 1365 <br> 1366 <br> 1367 | Payment not found <br> ``x-mobilepay-idempotency-key`` header has to be unique per request unless the request is a retry of a previous request <br> Refund of payment not possible when payment is not captured <br> Refund ``CurrencyCode`` is different than payment ``CurrencyCode`` <br> Payment is too old <br> Refund ``Amount`` is too high |

</details><br>

<details>
  <summary>POST /v10/refunds/{refundid}/capture - Capture a reserved Refund</summary><br>

| StatusCode | ErrorCodes  | Description |
|------------|-------------|-------------|
| 403 | 1402 | Cannot capture refunds created by a different integrator |
| 404 | 1004 | Refund not found |
| 409 | 1351 | Cannot capture refund when refund is not reserved |

</details><br>

<details>
  <summary>POST /v10/refunds/{refundid}/cancel - Cancel a reserved Refund</summary><br>

| StatusCode | ErrorCodes  | Description |
|------------|-------------|-------------|
| 403 | 1402 | Cannot cancel refunds created by a different integrator |
| 404 | - | Refund not found |
| 409 | 1352 | The refund cannot be cancelled in the current state |

</details><br>

## Stores
<details>
  <summary>GET /v10/stores/{storeid} - Query a Store</summary><br>

| StatusCode | ErrorCodes  | Description |
|------------|-------------|-------------|
| 404 | - | Store not found |

</details><br>

<details>
  <summary>GET /v10/stores - Query a Store by MerchantBrandId and MerchantLocationId</summary><br>

| StatusCode | ErrorCodes  | Description |
|------------|-------------|-------------|
| 400 | 1122 <br> 1119`` `` <br> 1120`` `` | Store filter not specific enough <br> Invalid ``MerchantBrandId`` <br> Invalid ``MerchantLocationId`` |

</details>
