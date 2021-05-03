# Authorization Code Flow With PKCE for SPA (Replaces Implicit Flow)
Reference [Microsoft identity platform and OAuth 2.0 authorization code flow - Microsoft identity platform | Microsoft Docs](https://docs.microsoft.com/en-us/azure/active-directory/develop/v2-oauth2-auth-code-flow)

### Change the Application type from Web to Spa

Option 1 Update the Manifest 

![App Registration](/img/5-AuthFlows-AuthGrantFlow-SPATypeManifest.png)

Option 2 Update using the Azure Portal UI

![App Registration](/img/5-AuthFlows-AuthGrantFlow-SPATypeUI.png)

If you have Implicit Grant or Hyrbid Flow (Access tokens / ID tokens) selected. Don't forget to untick these.

### Get our code token
Request a token using HTTP GET request to the authorization endpoint
```
https://login.microsoftonline.com/808b7ce5-9924-441e-ae53-3e42fd8a6d43/oauth2/v2.0/authorize?
client_id=53beb5ba-7615-48d1-bfc4-ab25744a0916
&response_type=code
&redirect_uri=https%3A%2F%2Fjwt.ms
&response_mode=form_post
&scope=openid profile User.Read offline_access
&state=12345
&code_challenge=ewGdtDctbdrwSyE1OOcXf9jqyqZecDoleYWfenSZl_w
&code_challenge_method=S256
```

Check out [Auth Grant Flow](/5-Authentication-Flows/Tokens/Auth-Grant-Flow.md#pkce) to create the code challenge and code verify.

Using Postman to format our URL

![Admin Consent](/img/5-AuthFlows-AuthGrantFlow-SPAPostman.png)

Using a browser we login and our code is then returned.

We used **form_post** so our code is returned inside the form data. We can see this data in the developer tools in Edge.

![Get Code](/img/5-AuthFlows-AuthGrantFlow-SPAPostmanGetCode.png)

**Code**
```
0.AUEA5XyLgCSZHkSuUz5C_YptQ7q1vlMVdtFIv8SrJXRKCRZBAHk.AQABAAIAAAD--DLA3VO7QrddgJg7Wevrs8cKSLCegT6F61ueTUF3fdE5IC3ExBjQEn5cb0uaZ6yrW2SxuaZ38TKsBppmsYVN0vbMO9-_i9ItJpZP1kajD_LILfLzxabDkE2dMeVsXyT5rpgy_ON5X21kQgicKkLx7ZswFqeesobO0vn551oB57odZVP5kepw6Js1Fxfg-rZHJ_kvbNbMGzEpdz7Ri4XqvPOK9jyXu1EPBD61-MgeNL2QbpzaAozlMZ4UZ68aJJq7k28Q85x5drO8iwr9FlKbnMOQsYLH7YGMuYpEpCwoNGGgICjckDZqv_AlILcaDhtBDacrmKjoKk3YYL44-AMjU-RWjLxW5rU70C20jhMof3mRGbyjI6_KZqheXLGpK23SFAG-gbw6_zF88EYSU4uQFYlLerWVWvrQZwSs3evdJL6MkpifHk-gOkHJw4HD-OXgKe4uOTVt0VFtcEElAR_a6mqy0qDQeHsB8EzZb2bqvDDoj3bBwtj4GtfLQlEgvwTGaM4sIedog16SDQ7PFPzeojy2k693wWwzt7-vQjY7XlhsOA-e_0bAL79JZiI-XPyVDM6SY6jVMrklzH6GYQRbiEugEKFf8KukGKvxhtcbfWfPLrc9eJNeDFHPrdJpbWGwVWB32vz1r3axA5meyGmA-KzIwkQN05hfDXmvuyLfH27tyE9sRB_8_8A9mmbknMAgAA
```

### Get Access Token
Now that we have our **code** we can request the access token from the token endpoint. 

We will simulate this in Postman using a HTTP POST request.

We must send the **Origin** header with the request

![Post Postman](/img/5-AuthFlows-AuthGrantFlow-SPAPostmanGetTokenHeader.png)

**Notice we do not exchange any client secret**

| Key  | Value |
| ------------- | ------------- |
| code  | 0.AUEA5XyLg...8A9mmbknMAgAA |
| grant_type  | authorization_code |
| client_id  | 53beb5ba-7615-48d1-bfc4-ab25744a0916 |
| redirect_uri  | https://jwt.ms |
| scope  | .default |
| code_verifier  |  S5J_4Gl5_l-...w8x5ww1 |


**Code Verifier**
```Javascript
S5J_4Gl5_l-Lw-Z7HLLGFyBkTOaURlaQ7j-ZXrkNzS3TMSK1hTReqZCLzISjQL36w9cyABE3ZsXn7jY3Ei6fBridu3Xu3PBdlHDSsijts5-78LV_dGfJaHMcEi2hw1FdFLgEDy6QfcyM52Kx40oG4pMwq1P3gl8I5SQzzw8x5ww1
```

![Post Postman](/img/5-AuthFlows-AuthGrantFlowPostPostman.png)

**Entire response including Access Token, ID Token and Access Token**
```Javascript
{
    "token_type": "Bearer",
    "scope": "profile openid email 00000003-0000-0000-c000-000000000000/User.Read 00000003-0000-0000-c000-000000000000/.default",
    "expires_in": 3599,
    "ext_expires_in": 3599,
    "access_token":         "eyJ0eXAiOiJKV1QiLCJub25jZSI6IkltZTVMdWR6N0c5bUV1aDJmQ0VlZ2RLSUwweFFqTUxHOUEzVjIzYU5VOU0iLCJhbGciOiJSUzI1NiIsIng1dCI6Im5PbzNaRHJPRFhFSzFqS1doWHNsSFJfS1hFZyIsImtpZCI6Im5PbzNaRHJPRFhFSzFqS1doWHNsSFJfS1hFZyJ9.eyJhdWQiOiIwMDAwMDAwMy0wMDAwLTAwMDAtYzAwMC0wMDAwMDAwMDAwMDAiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC84MDhiN2NlNS05OTI0LTQ0MWUtYWU1My0zZTQyZmQ4YTZkNDMvIiwiaWF0IjoxNjIwMDE5NjYwLCJuYmYiOjE2MjAwMTk2NjAsImV4cCI6MTYyMDAyMzU2MCwiYWNjdCI6MCwiYWNyIjoiMSIsImFjcnMiOlsidXJuOnVzZXI6cmVnaXN0ZXJzZWN1cml0eWluZm8iLCJ1cm46bWljcm9zb2Z0OnJlcTEiLCJ1cm46bWljcm9zb2Z0OnJlcTIiLCJ1cm46bWljcm9zb2Z0OnJlcTMiLCJjMSIsImMyIiwiYzMiLCJjNCIsImM1IiwiYzYiLCJjNyIsImM4IiwiYzkiLCJjMTAiLCJjMTEiLCJjMTIiLCJjMTMiLCJjMTQiLCJjMTUiLCJjMTYiLCJjMTciLCJjMTgiLCJjMTkiLCJjMjAiLCJjMjEiLCJjMjIiLCJjMjMiLCJjMjQiLCJjMjUiXSwiYWlvIjoiQVNRQTIvOFRBQUFBNXE3SDdHcXpGdUdLU05vTTViaklQTzVlcVU1aXhWNHFSWGV1MHJyUEllaz0iLCJhbXIiOlsicHdkIl0sImFwcF9kaXNwbGF5bmFtZSI6IkFuQXV0aExhYi1BdXRoQ29kZUZsb3ciLCJhcHBpZCI6IjUzYmViNWJhLTc2MTUtNDhkMS1iZmM0LWFiMjU3NDRhMDkxNiIsImFwcGlkYWNyIjoiMCIsImlkdHlwIjoidXNlciIsImlwYWRkciI6IjYwLjI0MS41LjE3NCIsIm5hbWUiOiJDbG91ZCIsIm9pZCI6IjA5YTg3MDQ3LTRmY2ItNGU1ZS1hYjQ4LWExN2VmOTM1NzBlYSIsInBsYXRmIjoiMyIsInB1aWQiOiIxMDAzMjAwMTE1NkY2ODBDIiwicmgiOiIwLkFVRUE1WHlMZ0NTWkhrU3VVejVDX1lwdFE3cTF2bE1WZHRGSXY4U3JKWFJLQ1JaQkFIay4iLCJzY3AiOiJVc2VyLlJlYWQgcHJvZmlsZSBvcGVuaWQgZW1haWwiLCJzaWduaW5fc3RhdGUiOlsia21zaSJdLCJzdWIiOiJILUxKWDFVczVJa21uVEpRMTRyTFV6UVoxcTRkem5Hd2lwcVROV1MzTU4wIiwidGVuYW50X3JlZ2lvbl9zY29wZSI6Ik9DIiwidGlkIjoiODA4YjdjZTUtOTkyNC00NDFlLWFlNTMtM2U0MmZkOGE2ZDQzIiwidW5pcXVlX25hbWUiOiJDbG91ZEBjb3BydGVjaDQudGsiLCJ1cG4iOiJDbG91ZEBjb3BydGVjaDQudGsiLCJ1dGkiOiJwRElLTmMwanNrdWQyblJ4SkY5NEFRIiwidmVyIjoiMS4wIiwid2lkcyI6WyJiNzlmYmY0ZC0zZWY5LTQ2ODktODE0My03NmIxOTRlODU1MDkiXSwieG1zX3N0Ijp7InN1YiI6IlZfR3hVT2RWdVJFemlsRVZnTUtsVDF3OW9PdlF0MjJSM2lvbVY5UXpnTGcifSwieG1zX3RjZHQiOjE1MzA4MzQzNjh9.IT4SvkSkLeEW1y4TEAS5uMytU9G-0eBOgTU5iixk4YZnUaI6GK7M6zXL6iOTnLyIurrcmbQbZflbCITiqjfkpAXxfZbr-Jo5WE4sBDDcAdPjNnv-DAVUw7E6onfwyVKoeQO6Rlb9AMoL6Kj2zE9HptsbQGonevpNqKHjczvBongi3RrQyVL28zjy54D4c4tL9zmeGV-jT5y5eS412RSMZIpaRnClclRFcQ4LthYbfWF-QHBbmVO_6fQWCLO67ai7Yi208Nlvp0j2xqXWass30X5MbaSK6rKhlkskGRSKrpUldYq1Ux6GcdVVH-2YbncQgxFzjjzXPUrblDrY3UZRxw",
    
    "refresh_token": "0.AUEA5XyLgCSZHkSuUz5C_YptQ7q1vlMVdtFIv8SrJXRKCRZBAHk.AgABAAAAAAD--DLA3VO7QrddgJg7WevrAgDs_wQA9P8XtPKTk0paWetIcGukt-kSrJHWAS0gcvTt7Nkf4CYeBTgKgWbzZJmqfQyiwOGeQ6RuLp1lABJJxjqesVf7AWsk291IpDVtxHPUcuT2n7Whm4oAIh1_fThzirGSzPziAc3_-uQfR-73S-z5-GgNqjznkSBuSLR-_Jvk-1tYUdpw6-hOwcFLwGgQ0jQQJftHtc9oxPszUDbwsknKKRr2HJVwn7-YYiVsm05-2jzR8fb28OIGlF2reIbWKTkwgGy819JzNQzOXn8j9w2HO1aGEatCWq4U6BanUzK0lKkOY51OBH2_5Nus7spV0JaYR_ldJy7tmnGp4bihswrGKMQBNy18ak989uMcU9mHdhpOKfMYTdSWAApf_b-F0V8-ol1rgwC5f-_1hF-hgKmxNTuPT7TyDz1rXm61Kbu2IBmKRPbB7dJQ8zzL8w2Rb8D17L_K_F3ao7ZTbUOINHuVtnWThLrIR0cc6wDYqM-dwuWVbWfbKfCc2fv_hJdjvK8UjCl9YAgF68RBN3D-hBPCA77Nx4tTxoamqfCjKl77QrZWpzCL_w8fvAn-ziyQQUQ5ggP0slw7652DbVblxTy2PKykgEcVyPM7QBeLXyry6GIE622fgFHpepEwwZz17dxWC7WHZJpvRsky_u7rkMyVS_vxolu58egfWPSeBP1O6wx4O2rX6ymz9x8FxDAcl4eB_21gYGag5WQPizMaRnFnGbs9N5tcCPJXPCsTQ-VTkzlVUGf7nNUfCNj7murj56BV2XuLm-QWLKGbDefbdziTFx7wIdaJscvZSooVu3aawUvLN1gw5lb01KbYzgOhEBcQwTOtR8xDCxTd4buARFB1chV672J0YJwRYUakqbTQjvLO44JeJPCajMaQcb0XppRW8RiHUVdjIJhkRwhw7ECOoQkOT86hH6MIH5v2gd-GuK1sq7ho",
    
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Im5PbzNaRHJPRFhFSzFqS1doWHNsSFJfS1hFZyJ9.eyJhdWQiOiI1M2JlYjViYS03NjE1LTQ4ZDEtYmZjNC1hYjI1NzQ0YTA5MTYiLCJpc3MiOiJodHRwczovL2xvZ2luLm1pY3Jvc29mdG9ubGluZS5jb20vODA4YjdjZTUtOTkyNC00NDFlLWFlNTMtM2U0MmZkOGE2ZDQzL3YyLjAiLCJpYXQiOjE2MjAwMTk2NjAsIm5iZiI6MTYyMDAxOTY2MCwiZXhwIjoxNjIwMDIzNTYwLCJuYW1lIjoiQ2xvdWQiLCJvaWQiOiIwOWE4NzA0Ny00ZmNiLTRlNWUtYWI0OC1hMTdlZjkzNTcwZWEiLCJwcmVmZXJyZWRfdXNlcm5hbWUiOiJDbG91ZEBjb3BydGVjaDQudGsiLCJyaCI6IjAuQVVFQTVYeUxnQ1NaSGtTdVV6NUNfWXB0UTdxMXZsTVZkdEZJdjhTckpYUktDUlpCQUhrLiIsInN1YiI6IlZfR3hVT2RWdVJFemlsRVZnTUtsVDF3OW9PdlF0MjJSM2lvbVY5UXpnTGciLCJ0aWQiOiI4MDhiN2NlNS05OTI0LTQ0MWUtYWU1My0zZTQyZmQ4YTZkNDMiLCJ1dGkiOiJwRElLTmMwanNrdWQyblJ4SkY5NEFRIiwidmVyIjoiMi4wIn0.b1-klgesXZkDut9Fy-dvElgKm48rB7735qazBETpj9aNiBFVSFp_kz2CvpVXTPkxzD0md5cb4TaZspME5nccVemLq_x4fS8wkuclefWFXBmz0D2Tm0KeEH3DEobTED1EshjB0JuVKT2gVSY1NwaAjNqAge6nJ9u7L19s7vi8Ccixv1_hQ1HKavB-FoBIqeSn6plMWFX7BdsIrzQJf6m2ENTzFe2vBxa0RmpqtcVR8A9nhCBl9otfoSTel4Jgu9B5KfLWCO4JxmFtYy0Wrum4yc0fYN1TyuiQn44pZWMPBXDfbUYkJm_igk44nsYmMDK_z3TZqSRcueu3XD2sHlPfRw"
}
```

We now have our **Access Token and ID Token**.

We can decode our **Tokens** to see what's inside. 

![JWT Decode](/img/5-AuthFlows-AuthGrantFlow-SPAJWTDecoded.png)

**Access Token Decoded**
```Javascript
{
  "typ": "JWT",
  "nonce": "Ime5Ludz7G9mEuh2fCEegdKIL0xQjMLG9A3V23aNU9M",
  "alg": "RS256",
  "x5t": "nOo3ZDrODXEK1jKWhXslHR_KXEg",
  "kid": "nOo3ZDrODXEK1jKWhXslHR_KXEg"
}.{
  "aud": "00000003-0000-0000-c000-000000000000",
  "iss": "https://sts.windows.net/808b7ce5-9924-441e-ae53-3e42fd8a6d43/",
  "iat": 1620019660,
  "nbf": 1620019660,
  "exp": 1620023560,
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
  "aio": "ASQA2/8TAAAA5q7H7GqzFuGKSNoM5bjIPO5eqU5ixV4qRXeu0rrPIek=",
  "amr": [
    "pwd"
  ],
  "app_displayname": "AnAuthLab-AuthCodeFlow",
  "appid": "53beb5ba-7615-48d1-bfc4-ab25744a0916",
  "appidacr": "0",
  "idtyp": "user",
  "ipaddr": "60.241.5.174",
  "name": "Cloud",
  "oid": "09a87047-4fcb-4e5e-ab48-a17ef93570ea",
  "platf": "3",
  "puid": "10032001156F680C",
  "rh": "0.AUEA5XyLgCSZHkSuUz5C_YptQ7q1vlMVdtFIv8SrJXRKCRZBAHk.",
  "scp": "User.Read profile openid email",
  "signin_state": [
    "kmsi"
  ],
  "sub": "H-LJX1Us5IkmnTJQ14rLUzQZ1q4dznGwipqTNWS3MN0",
  "tenant_region_scope": "OC",
  "tid": "808b7ce5-9924-441e-ae53-3e42fd8a6d43",
  "unique_name": "Cloud@coprtech4.tk",
  "upn": "Cloud@coprtech4.tk",
  "uti": "pDIKNc0jskud2nRxJF94AQ",
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

**ID Token Decoded**
```Javascript
{
  "typ": "JWT",
  "alg": "RS256",
  "kid": "nOo3ZDrODXEK1jKWhXslHR_KXEg"
}.{
  "aud": "53beb5ba-7615-48d1-bfc4-ab25744a0916",
  "iss": "https://login.microsoftonline.com/808b7ce5-9924-441e-ae53-3e42fd8a6d43/v2.0",
  "iat": 1620019660,
  "nbf": 1620019660,
  "exp": 1620023560,
  "name": "Cloud",
  "oid": "09a87047-4fcb-4e5e-ab48-a17ef93570ea",
  "preferred_username": "Cloud@coprtech4.tk",
  "rh": "0.AUEA5XyLgCSZHkSuUz5C_YptQ7q1vlMVdtFIv8SrJXRKCRZBAHk.",
  "sub": "V_GxUOdVuREzilEVgMKlT1w9oOvQt22R3iomV9QzgLg",
  "tid": "808b7ce5-9924-441e-ae53-3e42fd8a6d43",
  "uti": "pDIKNc0jskud2nRxJF94AQ",
  "ver": "2.0"
}.[Signature]
```