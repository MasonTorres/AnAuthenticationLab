# Authorization Code Flow
Reference [Microsoft identity platform and OAuth 2.0 authorization code flow - Microsoft identity platform | Microsoft Docs](https://docs.microsoft.com/en-us/azure/active-directory/develop/v2-oauth2-auth-code-flow)

### Create a new Application Registration and client secret
![App Registration](/img/5-AuthFlows-AuthGrantFlowAppRegistration.png)


| Key  | Value |
| ------------- | ------------- |
| Client ID | 53beb5ba-7615-48d1-bfc4-ab25744a0916 |
| Client Secret | -2V~PY_35799_nNCumu_-j1oeE0lpHy1Ar |

### Grant Admin Consent

![Admin Consent](/img/5-AuthFlows-AuthGrantFlowAdminConsent.png)

### Get our code token
Request a token using HTTP GET request to the authorization endpoint
```
https://login.microsoftonline.com/808b7ce5-9924-441e-ae53-3e42fd8a6d43/oauth2/v2.0/authorize?
client_id=53beb5ba-7615-48d1-bfc4-ab25744a0916
&response_type=code
&redirect_uri=https%3A%2F%2Fjwt.ms
&response_mode=form_post
&scope=User.Read offline_access
&state=12345
```
Using Postman to format our URL

![Admin Consent](/img/5-AuthFlows-AuthGrantFlowGetPostman.png)

Using a browser we login and our code is then returned.

We used **form_post** so our code is returned inside the form data. We can see this data in the developer tools in Edge.

![Get Code](/img/5-AuthFlows-AuthGrantFlowGetCode.png)

**Code**
```
0.AUEA5XyLgCSZHkSuUz5C_YptQ7q1vlMVdtFIv8SrJXRKCRZBAHk.AQABAAIAAAD--DLA3VO7QrddgJg7WevrII7T4y-8rkpXBxDXlr8Dr8fLvCCVXX91P9FU8LKdXP12Fv2r6lOw6ozQssfwWoyrLdqYeZ4Bxa2FPV7yvVMM2HacfRt_Z5ZQZ4Xd2ZgaXCaFHOwcCEr5skb94i0CLAK3qF361tzgknGA_iRXOInrNS60vzgfW5q3lxYJPf7cT0B_2JM75E6JGTGPUh5hD12n4hluhk6YXtCiXpcuQhqUien7TPPX63vl-zIukS0GnluDgGOMSnNUkvtrHMhT-tUbXJkGyxIyQfunCcsPRutb1gEOTsxdHzVCdTzipA1zZlP523KF3HEErRMxsWkgUOVd8znuFwT0S9o4i5gAQ1VrB03UhtLeol6Te5UILicfIAVBrIYrng7PgFLTHWTEwSuylUBd0-MXlImj7KtUOCase576WQaRcHquG4PmG37OLrZKQJS_YnGloqfIG-kpQmxChMPLXQyhirDLaMeVQMO0VLfCJc6C2jCjzDEDKxbYO5TQM2izm8ac_BZ6-wvesNvuiVYM5IHPf3aBRL7mAmUwxth8rylhEgaKtsot1nvvHs6kFvXJC79-HkMBFNF81SfEIAA
```

### Get Access Token
Now that we have our **code** we can request the access token from the token endpoint. 

We will simulate this in Postman using a HTTP POST request.

| Key  | Value |
| ------------- | ------------- |
| code  | 0.AUEA5XyLg...AbSxiQIAA |
| grant_type  | authorization_code |
| client_id  | 53beb5ba-7615-48d1-bfc4-ab25744a0916 |
| redirect_uri  | https://jwt.ms |
| client_secret  | -2V~PY_35799_nNCumu_-j1oeE0lpHy1Ar |
| scope  | .default |

![Post Postman](/img/5-AuthFlows-AuthGrantFlowPostPostman.png)

**Access Token**
```
eyJ0eXAiOiJKV1QiLCJub25jZSI6Im5ndjd3OE8ydUt4bTIxVUR6WHowbW13dzhlM0xCRXltc2NUUUFrTl9XOVkiLCJhbGciOiJSUzI1NiIsIng1dCI6Im5PbzNaRHJPRFhFSzFqS1doWHNsSFJfS1hFZyIsImtpZCI6Im5PbzNaRHJPRFhFSzFqS1doWHNsSFJfS1hFZyJ9.eyJhdWQiOiIwMDAwMDAwMy0wMDAwLTAwMDAtYzAwMC0wMDAwMDAwMDAwMDAiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC84MDhiN2NlNS05OTI0LTQ0MWUtYWU1My0zZTQyZmQ4YTZkNDMvIiwiaWF0IjoxNjE2NDc2MzA0LCJuYmYiOjE2MTY0NzYzMDQsImV4cCI6MTYxNjQ4MDIwNCwiYWNjdCI6MCwiYWNyIjoiMSIsImFjcnMiOlsidXJuOnVzZXI6cmVnaXN0ZXJzZWN1cml0eWluZm8iLCJ1cm46bWljcm9zb2Z0OnJlcTEiLCJ1cm46bWljcm9zb2Z0OnJlcTIiLCJ1cm46bWljcm9zb2Z0OnJlcTMiLCJjMSIsImMyIiwiYzMiLCJjNCIsImM1IiwiYzYiLCJjNyIsImM4IiwiYzkiLCJjMTAiLCJjMTEiLCJjMTIiLCJjMTMiLCJjMTQiLCJjMTUiLCJjMTYiLCJjMTciLCJjMTgiLCJjMTkiLCJjMjAiLCJjMjEiLCJjMjIiLCJjMjMiLCJjMjQiLCJjMjUiXSwiYWlvIjoiQVNRQTIvOFRBQUFBViszMndsbEJCQ3lnQ2hQdThKL0c1ZGJTd1pTWStHQktRcWZUaFFMVFJEOD0iLCJhbXIiOlsicHdkIl0sImFwcF9kaXNwbGF5bmFtZSI6IkFuQXV0aExhYi1BdXRoQ29kZUZsb3ciLCJhcHBpZCI6IjUzYmViNWJhLTc2MTUtNDhkMS1iZmM0LWFiMjU3NDRhMDkxNiIsImFwcGlkYWNyIjoiMSIsImlkdHlwIjoidXNlciIsImlwYWRkciI6IjE0LjIwMS4yMC4yIiwibmFtZSI6IkNsb3VkIiwib2lkIjoiMDlhODcwNDctNGZjYi00ZTVlLWFiNDgtYTE3ZWY5MzU3MGVhIiwicGxhdGYiOiIzIiwicHVpZCI6IjEwMDMyMDAxMTU2RjY4MEMiLCJyaCI6IjAuQVVFQTVYeUxnQ1NaSGtTdVV6NUNfWXB0UTdxMXZsTVZkdEZJdjhTckpYUktDUlpCQUhrLiIsInNjcCI6IlVzZXIuUmVhZCBwcm9maWxlIG9wZW5pZCBlbWFpbCIsInN1YiI6IkgtTEpYMVVzNUlrbW5USlExNHJMVXpRWjFxNGR6bkd3aXBxVE5XUzNNTjAiLCJ0ZW5hbnRfcmVnaW9uX3Njb3BlIjoiT0MiLCJ0aWQiOiI4MDhiN2NlNS05OTI0LTQ0MWUtYWU1My0zZTQyZmQ4YTZkNDMiLCJ1bmlxdWVfbmFtZSI6IkNsb3VkQGNvcHJ0ZWNoNC50ayIsInVwbiI6IkNsb3VkQGNvcHJ0ZWNoNC50ayIsInV0aSI6ImdqYjBUWHMyVWtTT040ME1hXzBVQUEiLCJ2ZXIiOiIxLjAiLCJ3aWRzIjpbImI3OWZiZjRkLTNlZjktNDY4OS04MTQzLTc2YjE5NGU4NTUwOSJdLCJ4bXNfc3QiOnsic3ViIjoiVl9HeFVPZFZ1UkV6aWxFVmdNS2xUMXc5b092UXQyMlIzaW9tVjlRemdMZyJ9LCJ4bXNfdGNkdCI6MTUzMDgzNDM2OH0.m8nmD9MsSnwdfBVuoinCMRHBikL08Gurd6dr7Bj6HtN-cZe4G7UXNXCs42fZRYmTHXDcs5Y4QgQZrSBNRR-X3fDijBQgzl7zBtCy0XcAYgSEbRAI87Qt9PQXm5vlo7h471JAW9UBffL9OXcnxXXyAK7uPSlIdZWMGQ3Ot6unTRhwjHaagnktC1kJvhohZ6NeCrOYiCu35xpV13QPbFVsW_BQtoUP2RtYyxGRlO3RLTkm8X6a42v5hrm5c5vr-a_EKAl7c6PUC1uZAGpIEHBQyFxWr_W-VMsFCbGjw01ika59qd6T-Mklx0PDwtkVFe7MavKweypvDOACgjxVLSE61A
```

We now have our **Access Token** and can use this to access the User.Read Microsoft Graph Endpoint.

We can decode our **Access Token** to see what's inside. 

![JWT Decode](/img/5-AuthFlows-AuthGrantFlowJWTDecoded.png)


```Javascript
{
  "typ": "JWT",
  "nonce": "ngv7w8O2uKxm21UDzXz0mmww8e3LBEymscTQAkN_W9Y",
  "alg": "RS256",
  "x5t": "nOo3ZDrODXEK1jKWhXslHR_KXEg",
  "kid": "nOo3ZDrODXEK1jKWhXslHR_KXEg"
}.{
  "aud": "00000003-0000-0000-c000-000000000000",
  "iss": "https://sts.windows.net/808b7ce5-9924-441e-ae53-3e42fd8a6d43/",
  "iat": 1616476304,
  "nbf": 1616476304,
  "exp": 1616480204,
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
  "aio": "ASQA2/8TAAAAV+32wllBBCygChPu8J/G5dbSwZSY+GBKQqfThQLTRD8=",
  "amr": [
    "pwd"
  ],
  "app_displayname": "AnAuthLab-AuthCodeFlow",
  "appid": "53beb5ba-7615-48d1-bfc4-ab25744a0916",
  "appidacr": "1",
  "idtyp": "user",
  "ipaddr": "14.201.20.2",
  "name": "Cloud",
  "oid": "09a87047-4fcb-4e5e-ab48-a17ef93570ea",
  "platf": "3",
  "puid": "10032001156F680C",
  "rh": "0.AUEA5XyLgCSZHkSuUz5C_YptQ7q1vlMVdtFIv8SrJXRKCRZBAHk.",
  "scp": "User.Read profile openid email",
  "sub": "H-LJX1Us5IkmnTJQ14rLUzQZ1q4dznGwipqTNWS3MN0",
  "tenant_region_scope": "OC",
  "tid": "808b7ce5-9924-441e-ae53-3e42fd8a6d43",
  "unique_name": "Cloud@coprtech4.tk",
  "upn": "Cloud@coprtech4.tk",
  "uti": "gjb0TXs2UkSON40Ma_0UAA",
  "ver": "1.0",
  "wids": [
    "b79fbf4d-3ef9-4689-8143-76b194e85509"
  ],
  "xms_st": {
    "sub": "V_GxUOdVuREzilEVgMKlT1w9oOvQt22R3iomV9QzgLg"
  },
  "xms_tcdt": 1530834368
}.[Signature]
```

## Refresh Token
Our initial HTTP GET request also contained **offline_access** in the **scope**. When we received the **Access Token** we also received a **Refresh Token**. This **Refresh Token** can be used to generate a new **Access Token** when the initial one expires by passing it as the **code** argument.

![Post Postman Refresh](/img/5-AuthFlows-AuthGrantFlowPostPostmanRefresh.png)

```
0.AUEA5XyLgCSZHkSuUz5C_YptQ7q1vlMVdtFIv8SrJXRKCRZBAHk.AgABAAAAAAD--DLA3VO7QrddgJg7WevrAgDs_wQA9P8fpJxb_KsKERfDte10kpmynDAoNdJ4BF2nffBL5g5HWOHt-ukFPkzho-qcs3A_xczPr22fccLhezHQe8BRkjPe-RyU6vnYkiY39zoOob6baXId2LVdbYOJPdmZ6MdN_PwuyxEBJl7frxq4AQ_JfFLvt7UzSa-O4sOG2Z8uHeGIP2cTAMJM5l4T4FbttRsAfVDgqBSXR8zN4SJ4hgBoyh3OqNTyRZB_4vP6yWHXGTksXlx6HVQRSbV7eDQ4vFeJWuQHnI161ZOBp9_UPtmS_wlRdT0u2cdSUuOToktgIPvkfOSrILzQf4-A3_8BT5AnvsB4F1Vm20NxjjFu6kmGZVjgB0vqFINmILg-eewCxa4ASl_nMGPZwn4WklY-NdACw3fh5Ju5ROwwlSiC_nFsJ5wZjMPvVKujdkeQF-3ch_1dKZaOzayeiG2_qxZDO5xlwtFq5nQnO8E80_UMkDkfjGX2okShV2RwJU--D5p6ZsqeiV2I8gxU36-cIYbr68jRKUy77DvX9W72hOK0KhGlKIgW87X3Mt85q56aSlsxxobXn-1MbN7KQaJGX38XqIfwo43JXGnsv50j0pOfJBzBetC8x4iDL94j5Wu1AbzQBKAdB8lO1O12W-2SZU4x037ny5KV3WhYWrlO6eQIKDaSz0T_xe0jv2gv0-SD_rMHvaYDQ1-GjG2QrUPD5iKXkZyoSeh_LOByF981nh2LNr6yWB8GZv6vE2oqqxDNB_QV2riRhZgIlLS7MDE2DwfK6bCswzj_ZcZQvkO_nsZ6I-pehVWm6QA4OQ_suuM65ggT6XUtHnJtXyEnQJ83CWblFB-1z2GXtJew4wqOmNyLeXtAxrPgD7xDhLG4NR_h8HSV0BpdsbM5ZK0qBL64lclDSh6aJWXJg4rWdSn2wzNK0-g
```

# Adding Extra Security<a name="pkce"></a>

[PKCE RFC](https://tools.ietf.org/html/rfc7636)

## Proof Key for Code Exchange (PKCE)

Use an online tool [https://tonyxu-io.github.io/pkce-generator/](https://tonyxu-io.github.io/pkce-generator/)

### Create Code_Verify using PowerShell
```powershell
Add-Type -AssemblyName System.Web
$RandomNumberGenerator = New-Object System.Security.Cryptography.RNGCryptoServiceProvider
$Bytes = New-Object Byte[] 128
$RandomNumberGenerator.GetBytes($Bytes)
$codeVerify = [System.Web.HttpServerUtility]::UrlTokenEncode($Bytes)
```

### Create Code_Challenge using PowerShell
```powershell
$hasher = [System.Security.Cryptography.HashAlgorithm]::Create('sha256')
$hash = $hasher.ComputeHash([System.Text.Encoding]::UTF8.GetBytes($codeVerify))
$codeChallenge = [System.Convert]::ToBase64String($hash)
$codeChallenge = $codeChallenge.Split('=')[0]
$codeChallenge = $codeChallenge.Replace('+', '–')
$codeChallenge = $codeChallenge.Replace('/', '_')
```

![Powershell PKCE](/img/5-AuthFlows-AuthGrantFlowPowerShellPKCE.png)

**Code_Verify**
```
S5J_4Gl5_l-Lw-Z7HLLGFyBkTOaURlaQ7j-ZXrkNzS3TMSK1hTReqZCLzISjQL36w9cyABE3ZsXn7jY3Ei6fBridu3Xu3PBdlHDSsijts5-78LV_dGfJaHMcEi2hw1FdFLgEDy6QfcyM52Kx40oG4pMwq1P3gl8I5SQzzw8x5ww1
```

**Code_Challenge**
```
ewGdtDctbdrwSyE1OOcXf9jqyqZecDoleYWfenSZl_w
```

### Resend our HTTP GET request with PKCE 
We've added the **code_challenege** and **code_challenge_method** arguments

```
https://login.microsoftonline.com/808b7ce5-9924-441e-ae53-3e42fd8a6d43/oauth2/v2.0/authorize?
client_id=53beb5ba-7615-48d1-bfc4-ab25744a0916
&response_type=code
&redirect_uri=https%3A%2F%2Fjwt.ms
&response_mode=form_post
&scope=User.Read offline_access
&state=12345
&code_challenge=ewGdtDctbdrwSyE1OOcXf9jqyqZecDoleYWfenSZl_w
&code_challenge_method=S256
```

Get our new **code**

### Send our HTTP POST request PKCE
Notice the **code_verifier** and of course our new **Access Token**

![Powershell PKCE](/img/5-AuthFlows-AuthGrantFlowPKCEAccessToken.png)
