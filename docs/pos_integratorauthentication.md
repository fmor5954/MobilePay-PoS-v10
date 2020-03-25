# **Onboarding to PoS**


1. **Read API documentation**. You'll find it in the  [APIs menu](https://developer.mobilepay.dk/product).  

2.  **Log-in** Go to  [Sandbox developer portal](https://sandbox-developer.mobilepay.dk/ ) and log in with your credentials.

 3.  **Create app** - My Apps > Create new App to register a new application.   You need to supply the `x-ibm-client-id`  when calling the api. You should always store the `x-ibm-client-id` in a secure location, and never reveal it publicly. If you suspect that the secret key has been compromised, you may reset it immediately by clicking the link on the application details page. More details about the usage of `x-ibm-client-id` below in the authentication section. 

4.  **Subscribe to APIs.**  To implement MobilePay PoS, go to  [APIs](https://sandbox-developer.mobilepay.dk/product)  and subscribe to the following APIs:
-  PoS for Integrators  
-  PoS User simulation for Integrators
-  Integrator Authentication 

### Step 2 - Authentication for PoS

----------
 
1. Receive OAuth Client Credentials via zip file. The Client Credentials are to be used when calling the token endpoint. The zip file will be sent via e-mail. The zip file is locked with a password. DeveloperSupport will provide the password via text message, due to security reasons.
 
Now you are ready to move on to the authentication section below.  

# **Integrator Authentication:**

In order for Integrators to be able to use MobilePay APIs, first they'll have to obtain an `access_token` from the Integrator Authentication service. This way, MobilePay knows who is calling our service. 

  **Environments:**
The following URL are the environment routes for the Integrator Authentication API

 - SandBox: `https://api.mobilepay.dk/integrator-authentication`
 - Production: `https://api.mobilepay.dk/integrator-authentication`

### **Credentials Flow:**


The Integrator Authentication solution is based on the OpenID/OAuth 2.0 specification. By following the OpenID Connect protocol, MobilePay makes it easy for integrators to integrate with MobilePay. Currently, the  flow supported is the Client Credentials grant type. Credentials Flow (defined in OAuth 2.0 RFC 6749, section 4.4), in which Integrators pass along their `client_id` and `client_secret` to authenticate themselves and get a token.

[![](assets/images/clientcredentialsdiagram.png)](assets/images/possekvensdiagram.png)


 1. The client app authenticates with the Authorization Server using its Client ID and Client Secret /token endpoint
 2. The Authorization Server validates the `client_id` and `client_secret`.
 3. The Authorization Server responds with an `access_token`.
 4. The Client application can use the `access_token` to call the API
 5. The API responds with requested data.

[![](assets/images/possekvensdiagram.png)](assets/images/possekvensdiagram.png)

# **Endpoint description:**

This document does not include a compete specification of the endpoints, responses and response codes. This information can be found in the [APIs section](https://developer.mobilepay.dk/product). of the Developer Portal.

 

## `POST /connect/token`

The token endpoint is when requesting an `access token` for an onboarded integrator client.

Headers:

 - **``Content-Type``**: x-www-urlencoded
 - **``x-ibm-client-id``**: Client_Id supplied upon certification.
 - **``Authorization``**: Basic ({CLIENT_ID}:{CLIENT_SECRET}).toBase64EncodedString().

The OAuth `client_id`and `client_secret` will be sent to the integrator in a closed zip file from developer@mobilepay.dk to integrators e-mail 

 
| Parameter | Value  | Description  |
| :---         |     :---:      |          :---:  |
| grant_type    | client_credentials     | The Client Credentials grant type is used by clients to obtain an `access_token` outside of the context of a user.     |
| merchant_vat     | DK12345678 or FI12345678       | VAT Number of the Merchant the integrator is integrating on behalf. It will be applied to the JWT access token, if supplied. We support FI and DK vat numbers. The vat number consists of country prefix (either FI or DK) and 8 digits.      |

Example of response body from SandProd environment:

Response Body

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

1. `200 - OK`  
 

2. `401 - Unauthorized` , if the client is not authorized/authenticated through the API Gateway


### cURL example:

```
curl --location --request POST 'https://api.sandbox.mobilepay.dk/integrator-authentication/connect/token' \
--header 'x-ibm-client-id: {YOUR_GENERATED_CLIENT-ID}' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--header 'Authorization: Basic ({YOUR_CLIENT_ID}:{YOUR_CLIENT_SECRET}).toBase64EncodedString()' \
--data-urlencode 'grant_type=client_credentials' \
--data-urlencode 'vat_number=DK12345678'
```
