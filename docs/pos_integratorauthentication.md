# **Integrator Authentication:**

In order for Integrators to be able to use MobilePay APIs, first they'll have to obtain an access token from the Integrator Authentication service. 

### **Environments:**

 - SandBox: [https://api.sandbox.mobilepay.dk](https://api.sandbox.mobilepay.dk/)[/integrator-authentication](https://api.mobilepay.dk/integrator-authentication)
 - Production: [https://api.mobilepay.dk/integrator-authentication](https://api.mobilepay.dk/integrator-authentication)

### **Credentials Flow:**

[![](assets/images/clientcredentials.png)](assets/images/clientcredentials.png)

The Integrator Authentication solution is based on the OpenID/OAuth 2.0 specification. Currently, the only flow supported is the Client Credentials grant type. Credentials Flow (defined in OAuth 2.0 RFC 6749, section 4.4), in which Integrators pass along their Client ID and Client Secret to authenticate themselves and get a token.

[![](assets/images/clientcredentialsdiagram.png)](assets/images/clientcredentialsdiagram.png)

 1. The client app authenticates with the Authorization Server using its Client ID and Client Secret /token endpoint
 2. The Authorization Server validates the Client ID and Client Secret.
 3. The Authorization Server responds with an Access Token.
 4. The Client application can use the Access Token to call the API
 5. The API responds with requested data.

# **Endpoint description:**

This document does not include a compete specification of the endpoints, responses and response codes. This information can be found in the API section of the Developer Portal.


##  `GET /integrator-authentication/.well-known/openid-configuration`
The OpenID Connect protocol requires the use of multiple endpoints for authenticating users, and for requesting resources.

To simplify implementations and increase flexibility, OpenID Connect allows the use of a "Discovery document," a JSON document found at a well-known location containing key-value pairs which provide details about the OpenID Connect provider's configuration, including the URIs of the authorization, token, scopes, claims claims, scopes, grant types and endpoints are to be used upon authentication.

Headers:

X-IBM-Client-Id: Client_Id supplied upon certification.

Here is an example of such a document from MobilePay SandProd environment; the field names are those specified in OpenID Connect Discovery 1.0 (refer to that document for their meanings). The values are purely illustrative and might change, although they are copied from from a recent version of the actual Integrator Authentication Discovery document:

```
{
    "issuer": "https://api.mobilepay.dk/integrator-authentication",
    "jwks_uri": "https://api.sandbox.mobilepay.dk/integrator-authentication/.well-known/openid-configuration/jwks",
    "token_endpoint": "https://api.sandbox.mobilepay.dk/integrator-authentication/connect/token",
    "scopes_supported": [
        "integrator_scope",
        "offline_access"
    ],
    "claims_supported": [],
    "grant_types_supported": [
        "client_credentials"
    ],
    "response_types_supported": [
        "token"
    ],
    "token_endpoint_auth_methods_supported": [
        "client_secret_basic",
        "client_secret_post"
    ],
    "id_token_signing_alg_values_supported": [
        "RS256"
    ],
    "subject_types_supported": [
        "public"
    ],
    "code_challenge_methods_supported": [
        "plain",
        "S256"
    ]
}
 
```

### <a name="statuscodes"></a>Expected status codes

You might encounter the following status codes :

1. `200 - OK`  

2. `401 - Unauthorized` , if the client is not authorized/authenticated through the API Gateway
 
 
### cURL example:

```console 
curl --location --request GET 'https://api.sandbox.mobilepay.dk/integrator-authentication/.well-known/openid-configuration' \
--header 'X-IBM-Client-Id: {YOUR_CLIENT_ID}''
```

##  `GET /integrator-authentication/.well-known/openid-configuration/jwks`

A JSON Web Key (JWK) is a standard method for representing a cryptographic key using JSON. The spec can be found [here](https://tools.ietf.org/html/rfc7517)

The Integrator Authentication solutions signs all JWT access token with a private key. The public key can be obtained in the jwks endpoint to verify the authenticity of the access token.

Headers:

 - `X-IBM-Client-Id` supplied upon certification.

Example of response body from SandProd environment:


```
{
    "keys": [
        {
            "kty": "RSA",
            "use": "sig",
            "kid": "A9A7ACCF1884D01D4AB0FFD1049124B74B1E8BAD",
            "x5t": "qaeszxiE0B1KsP_RBJEkt0sei60",
            "e": "AQAB",
            "n": "peYBOoky5YBl2O_SUCLUlzoc2rzoDqlYS8Tha9rmV0SaTlpRm41LK5dAOTFZSoZqQVcUKoSpGtyg2Kpjr5DdMhN59XzAALjVETeEuLbUjthDkzQWXCck3WytzHxiKrwP59MFFosP75k2xh-05WYaSTlrATesNXblj33DG7okv9wCZqidQUBVHyn7vscOk_mTigZMrsTxpclr5fdrtGVa-tHg_97k7YOdsyurjLCzT7IjX4ekyuOJnzNwoEc5I2cSpNSu0tpTlI_6SaTr8Y9hJvM8REUvruh0vJUSyyo2OfFlGQ5TKsGaaNYzJNwycxh5UIwY5v4reWRDxG8TZ-yRxQ",
            "x5c": [
                "MIIFIDCCAwigAwIBAgIFAMaFE1EwDQYJKoZIhvcNAQELBQAwgZgxEDAOBgNVBAMTB0RCR1JPT1QxCzAJBgNVBAYTAkRLMRMwEQYDVQQHEwpDb3BlbmhhZ2VuMRAwDgYDVQQIEwdEZW5tYXJrMRowGAYDVQQKExFEYW5za2UgQmFuayBHcm91cDEaMBgGA1UECxMRRGFuc2tlIEJhbmsgR3JvdXAxGDAWBgNVBAUTDzYxMTI2MjI4MTExMDAwMzAeFw0yMDAyMjQwMDAwMDBaFw0yMjAyMjQwMDAwMDBaMIG1MSwwKgYDVQQDEyNNb2JpbGVQYXkgSW50ZWdyYXRvciBBdXRoZW50aWNhdGlvbjELMAkGA1UEBhMCREsxEzARBgNVBAcTCkNvcGVuaGFnZW4xEDAOBgNVBAgTB0Rlbm1hcmsxGjAYBgNVBAoTEURhbnNrZSBCYW5rIEdyb3VwMRowGAYDVQQLExFEYW5za2UgQmFuayBHcm91cDEZMBcGA1UEBRMQNjExMjYyMjgzMDYxMDAwMTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAKXmATqJMuWAZdjv0lAi1Jc6HNq86A6pWEvE4Wva5ldEmk5aUZuNSyuXQDkxWUqGakFXFCqEqRrcoNiqY6+Q3TITefV8wAC41RE3hLi21I7YQ5M0FlwnJN1srcx8Yiq8D+fTBRaLD++ZNsYftOVmGkk5awE3rDV25Y99wxu6JL/cAmaonUFAVR8p+77HDpP5k4oGTK7E8aXJa+X3a7RlWvrR4P/e5O2DnbMrq4yws0+yI1+HpMrjiZ8zcKBHOSNnEqTUrtLaU5SP+kmk6/GPYSbzPERFL67odLyVEssqNjnxZRkOUyrBmmjWMyTcMnMYeVCMGOb+K3lkQ8RvE2fskcUCAwEAAaNSMFAwHwYDVR0jBBgwFoAU61V/xZhpcCaqEN34Mo9XCeNAPzowHQYDVR0OBBYEFIIFlKd9KiBOtaU4DdKrOWXIxYk3MA4GA1UdDwEB/wQEAwIGwDANBgkqhkiG9w0BAQsFAAOCAgEAdNd+N9b/He3X/vNCBv1gErfKGANCmWRWmDIJ8nxx/zOOEM6/699qdHobS0kq5I5N9LFalf2m+7HJml2x0K0zXtYQiyPIqXhNjmV+wA0xCJwoUZSg5zBcMTLPXGpA3tm6sit7VNIGUwaJO1AScfyXJnoFKARQjBAx4u/OCL6fiCY21UN/w6Kuw2MO7KX4ZaYSf4+zg84yWwtiw+PUvVr3Unh5AUtxABfs4IXa/kPwSY0B39BunhOl5vAUefPWFaF1xnhz/TMnlh8Eah2gGbI8Ylw5sf1S+zd2y7Td4xfldms74wP8zNv+2oUpqidqhs/vBBD9gz3wSp/AvDMqdd0LBK/U6cHFur0Jtpgd9wX8Git5VVTgMBi6oAbTNouqirxm28qteunaUZZmReToSV2HMD8LZO+V3UYstsGienLQVCpXz6cIOpM2mLAZrXfAgbo/GEbocp4M7qTHipcJkQLFNKEYAr1ijwVwi8jEgfTtWnus54dRImdzVo1YVX1oTNRmqlNGUOuZfZC0lckXJny4fcFookB+hUlk2HqPcZ00/ytVh1c4wfXdOdCokVlxW8GLzsB1iopV4B5a1tPs41ZpDTy936CmQa44Emd2Edclra8h0PwVaGI5p9WXxPyxdK0uk/xc+aSLQpvlJO8To9kUNpVH7uJMmbc3wWR2NE9Y9Ss="
            ],
            "alg": "RS256"
        }
    ]
}
```

### Expected status codes

You might encounter the following status codes :

1. `200 - OK`  
 

2. `401 - Unauthorized` , if the client is not authorized/authenticated through the API Gateway

    

### cURL example:

```console 
curl --location --request GET 'https://api.sandbox.mobilepay.dk/integrator-authentication/.well-known/openid-configuration/jwks' \
--header 'X-IBM-Client-Id: {YOUR_CLIENT_ID}'
```



## `POST /connect/token`

Used when requesting an access token for an onboarded integrator client.

Headers:

 - **Content-Type**: x-www-urlencoded
 - **X-IBM-Client-Id**: Client_Id supplied upon certification.
 - **Authorization**: Basic ({CLIENT_ID}:{CLIENT_SECRET}).toBase64EncodedString().

The Client_id and client_secret will be sent to the integrator in a closed zip file from developer@mobilepay.dk to integrators e-mail 

Parameter description:
|grant_type  | client_credentials |
|--|--|
| vat_number | VAT Number of the Merchant the integrator is integration on behalf. Will be applied to the JWT access token, if supplied. Can be left empty. (OPTIONAL). |


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
--header 'X-IBM-Client-Id: {YOUR_CLIENT-ID}' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--header 'Authorization: Basic ({YOUR_CLIENT_ID}:{YOUR_CLIENT_SECRET}).toBase64EncodedString()' \
--data-urlencode 'grant_type=client_credentials' \
--data-urlencode 'vat_number=DK123456'
```
 
    
