# Implicit Grant Flow
Reference [OAuth 2.0 implicit grant flow - The Microsoft identity platform | Microsoft Docs](https://docs.microsoft.com/en-us/azure/active-directory/develop/v2-oauth2-implicit-grant-flow)

### Create a new Application Registration with a redirect URI
![App Registration](/img/5-AuthFlows-ImplicitGrantFlowAppRegistration.png)


| Key  | Value |
| ------------- | ------------- |
| Client ID | b4c1990a-717a-4cf4-b545-debb74caeeff |
| Redirect URI  | https://jwt.ms |

### Get our Access Token
Request a token using HTTP GET request to the authorization endpoint
```
https://login.microsoftonline.com/808b7ce5-9924-441e-ae53-3e42fd8a6d43/oauth2/v2.0/authorize?
client_id=b749281f-ca24-423f-bf26-6c17a2d211e3
&scope=User.Read
&response_type=token
&redirect_uri=https%3A%2F%2Fjwt.ms
&response_mode=fragment
&state=12345
```
Using Postman to format our URL

![Postman](/img/5-AuthFlows-ImplicitGrantFlowPostman.png)


**Login with a Browser**

![Web Login](/img/5-AuthFlows-ImplicitGrantFlowLogin.png)

You will be redirected to https://jwt.ms where your Access Token will be visible. 

![JWT](/img/5-AuthFlows-ImplicitGrantFlowJWTDecoded.png)

```
{
  "typ": "JWT",
  "nonce": "Z28UoOrB2xefiPC8hcoJ37x6DZluwzn9r-XPoapGoAE",
  "alg": "RS256",
  "x5t": "nOo3ZDrODXEK1jKWhXslHR_KXEg",
  "kid": "nOo3ZDrODXEK1jKWhXslHR_KXEg"
}.{
  "aud": "00000003-0000-0000-c000-000000000000",
  "iss": "https://sts.windows.net/808b7ce5-9924-441e-ae53-3e42fd8a6d43/",
  "iat": 1616705114,
  "nbf": 1616705114,
  "exp": 1616709014,
  "acct": 0,
  "acr": "1",
  "acrs": [
    "urn:user:registersecurityinfo",
    "urn:microsoft:req1",
    "urn:microsoft:req2",
    "urn:microsoft:req3",
    "c1",
    "c2",
    "c3",
    "c4",
    "c5",
    "c6",
    "c7",
    "c8",
    "c9",
    "c10",
    "c11",
    "c12",
    "c13",
    "c14",
    "c15",
    "c16",
    "c17",
    "c18",
    "c19",
    "c20",
    "c21",
    "c22",
    "c23",
    "c24",
    "c25"
  ],
  "aio": "E2ZgYFh39JkY3+24JsGpD0sPWWmoMJjvdbnPkhoqOjmb3c3kTDgA",
  "amr": [
    "pwd"
  ],
  "app_displayname": "OIDCTest",
  "appid": "b749281f-ca24-423f-bf26-6c17a2d211e3",
  "appidacr": "0",
  "idtyp": "user",
  "ipaddr": "14.201.20.2",
  "name": "Cloud",
  "oid": "09a87047-4fcb-4e5e-ab48-a17ef93570ea",
  "platf": "3",
  "puid": "10032001156F680C",
  "rh": "0.AUEA5XyLgCSZHkSuUz5C_YptQx8oSbckyj9CvyZsF6LSEeNBAHk.",
  "scp": "email Mail.Read profile User.Read openid",
  "signin_state": [
    "kmsi"
  ],
  "sub": "H-LJX1Us5IkmnTJQ14rLUzQZ1q4dznGwipqTNWS3MN0",
  "tenant_region_scope": "OC",
  "tid": "808b7ce5-9924-441e-ae53-3e42fd8a6d43",
  "unique_name": "Cloud@coprtech4.tk",
  "upn": "Cloud@coprtech4.tk",
  "uti": "CNpO92tHWES5Zey5OtanAA",
  "ver": "1.0",
  "xms_st": {
    "sub": "GVXGkx-tkBvFn0M41LExpTeplOTmr-XoxARfTE-t4eg"
  },
  "xms_tcdt": 1530834368
}.[Signature]
```