# <a name="index"></a> Introduction 

Our MobilePay Point of Sale (PoS) REST API is intended for integrators implementing MobilePay payments in a PoS system.

For information about the PoS product and details on Getting Started, Test and Certification please visit our
[Developer Portal](https://developer.mobilepay.dk/products/pos10). On the portal you will also find FAQ and support resources.

This document will explain in more detail how to implement the different payment flows, how to manage the PoS, how the Security model is built and what is needed from the integrator.

This document does not include detailed specification of the endpoints, responses and response codes. This information can be found in the [API section](https://sandbox-developer.mobilepay.dk/product) of the Developer Portal.

## MobilePay Point-of-Sale version 10.0
There are several versions of MobilePay PoS. This documentation describes version 10.0 of the API. Version 10.0 is an update with several changes from version 8.6 of the API. The changes to the latest API are made to improve the quality of the MobilePay PoS clients and to make it easier to build new features for the benefit of merchants. For an overview of the changes in 10.0, see the [changes overview](overview_of_changes).

## Payment experience
MobilePay PoS is an API for setting up a merchant's transaction requests on customers' MobilePay apps in a store. MobilePay PoS does not require the customer to manually enter any information in the MobilePay app pertaining to the transaction. A transaction request can typically be obtained by the customer by holding the mobile device near a merchant device (Terminal, BLE beacon, etc.) or scanning a QR code.

Currently MobilePay PoS uses BLE one-way and two-way beacons and QR-codes to set up the transaction requests - the technology choices are not important for the API - however the concept of a beacon ID is central to allow matching of the customer willing to pay and the merchant's transaction request.

## The MobilePay Developer Portal
The MobilePay Developer Portal is a site where you will be able to find information about the products and available APIs and their documentations.
It exposes live documentation that can be used for development. How to get access to the Developer Portal is described [here](https://developer.mobilepay.dk/products/point-of-sale/getstarted).

The MobilePay Developer Portal is available at the following addresses:

| Environment  | Endpoint |
|--------------|-------------|
| Sandbox/Test | [https://sandbox-developer.mobilepay.dk](https://sandbox-developer.mobilepay.dk)     |
| Production   | [https://developer.mobilepay.dk](https://developer.mobilepay.dk)     |

[![](assets/images/Preview-MP-logo-and-type-horizontal-blue.png)](assets/images/Preview-MP-logo-and-type-horizontal-blue.png)
