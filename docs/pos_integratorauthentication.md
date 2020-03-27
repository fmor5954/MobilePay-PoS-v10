
The MobilePay PoS V10 API uses TLS for communication security and data integrity (secure channel between the client and the backend) and uses access tokens to authenticate integrator clients. The guide below describe the steps to onboard an integrator client and generate access tokens to authenticate the integrator client. 

# <a name="client_onboarding"></a>**Onboarding a PoS integrator client**

1. **Read API documentation.** You'll find it in the  [APIs menu](https://developer.mobilepay.dk/product).  

2.  **Log-in on the developer portal.** Go to [Sandbox developer portal](https://sandbox-developer.mobilepay.dk/) and log in with your credentials.

 3.  **Create an app in the developer portal.** - My Apps > Create new App to register a new application.   You need to supply the `x-ibm-client-id`  when calling the API. You should always store the `x-ibm-client-id` in a secure location, and never reveal it publicly.  More details about the usage of `x-ibm-client-id` below in the authentication section. 

4.  **Subscribe the app to APIs.**  Go to [APIs](https://sandbox-developer.mobilepay.dk/product) and subscribe to the following APIs:
-  PoS for Integrators  
-  PoS User simulation for Integrators
-  Integrator Authentication 
 
 5.  **Receive OAuth Client Credentials via zip file.** The Client Credentials will be used when calling the token endpoint (described below) to generate an access token. The zip file will be sent via e-mail. The zip file is locked with a password. DeveloperSupport will provide the password via text message to ensure the password protected file and the password is not transmitted together.

Now you are ready to move on to the authentication section below.  

# **Integrator Authentication:**

The PoS V10 API uses access tokens to authenticate calls from integrator clients. In order for an integrator client to use the PoS V10 API, it must first obtain an access token using the Integrator Authentication API. The access tokens used in the PoS V10 solution identifies both an integrator client and the integrator and may optionally identify the merchant on which the client is calling on behalf of. 

### **Credentials Flow:**

The Integrator Authentication solution is based on the OpenID/OAuth 2.0 specification. By following the OpenID Connect protocol, MobilePay makes it easy for integrators to integrate with MobilePay. Currently, the flow supported is the Client Credentials grant type. In the Credentials Flow (defined in OAuth 2.0 RFC 6749, section 4.4), integrators pass along their `client_id` and `client_secret` (received in Step 5 above) to authenticate themselves and obtain an access token. The Credentials Flow is illustrated in the diagram below.

[![](assets/images/possekvensdiagram.png)](assets/images/possekvensdiagram.png)

 1. The client app authenticates with the Authorization Server using its `client_id` and `client_secret` using the token endpoint.
 2. The Authorization Server validates the `client_id` and `client_secret`.
 3. The Authorization Server responds with an `access_token`.
 4. The Client application can use the `access_token` to call the PoS V10 API.
 5. The PoS V10 API responds.

# **Obtaining an access token:**

This document only describes the token endpoint used to request an access token. A complete specification of the endpoints, responses and response codes for the Integrator Authentication API can be found in the [APIs section](https://developer.mobilepay.dk/product) of the Developer Portal.
 
The token endpoint (`POST /connect/token`) is used when requesting an access token for an onboarded integrator client. The following
headers must be set:

 - **``Content-Type``**: x-www-urlencoded
 - **``x-ibm-client-id``**: `client_id`
 - **``Authorization``**: Basic (`client_id`:`client_secret`).toBase64EncodedString().

The OAuth `client_id`and `client_secret` will be sent to the integrator in a closed zip file from [developer@mobilepay.dk](mailto:developer@mobilepay.dk) to integrators e-mail (step 5 in the [Client onboarding guide](pos_integratorauthentication#client_onboarding)).

In addition, the `grant_type` parameter must be set and a `merchant_vat` parameter may optionally be set as described below:
 
| Parameter | Value  | Description  |
| :---         |     :---:      | :---  |
| `grant_type` | client_credentials | The Client Credentials grant type is used by clients to obtain an `access_token` outside of the context of a user.     |
| `merchant_vat` | DK12345678 or FI12345678 | VAT number of the merchant the integrator client is calling on behalf of. The VAT number consists of country prefix (either FI or DK) and 8 digits. |

If the `merchant_vat` parameter is supplied, the VAT number will be added as a claim on the access token, and it will only be possible to use the access token to perform calls on behalf of the given merchant. If it is not supplied, the access token will not be restricted to a fixed merchant. Instead, clients will have to include a header on all calls to the PoS V10 API that includes the VAT number of the merchant the client is acting on behalf of, for the given call. 

Example of response body from SandBox environment:
```
{
    "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c",
    "expires_in": 3700,
    "token_type": "Bearer",
    "scope": "integrator_scope"
}
```

### Expected status codes

You might encounter the following status codes :

* `200 - OK`  
* `401 - Unauthorized` if the client is not authorized/authenticated through the API Gateway

### cURL example:

```
curl --location --request POST 'https://api.sandbox.mobilepay.dk/integrator-authentication/connect/token' \
--header 'x-ibm-client-id: {YOUR_GENERATED_CLIENT-ID}' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--header 'Authorization: Basic ({YOUR_CLIENT_ID}:{YOUR_CLIENT_SECRET}).toBase64EncodedString()' \
--data-urlencode 'grant_type=client_credentials' \
--data-urlencode 'merchant_vat=DK12345678'
```

 **Environments:**
The following URLs are the environment routes for the Integrator Authentication API

 - SandBox: `https://api.sandbox.mobilepay.dk/integrator-authentication`
 - Production: `https://api.mobilepay.dk/integrator-authentication`
