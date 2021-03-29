# Client Credential Flow
Reference [OAuth 2.0 client credentials flow on the Microsoft identity platform | Microsoft Docs](https://docs.microsoft.com/en-us/azure/active-directory/develop/v2-oauth2-client-creds-grant-flow)

### Create a new Application Registration and client secret
![App Registration](/img/5-AuthFlows-ClientCredentialFlowAppRegistration.png)


| Key  | Value |
| ------------- | ------------- |
| Client ID | 2bfddaf4-0673-49a7-a615-2b6bb3acbf31 |
| Client Secret | FLR0V3BhB88.sv7UhZv~y~j5r92_~S8wxn |

### Get Access Token using Postman
Request a token using HTTP POST request to the token endpoint
![App Registration](/img/5-AuthFlows-ClientCredentialFlowAccessToken.png)

**Access Token**
```
eyJ0eXAiOiJKV1QiLCJub25jZSI6IlNUdl95RWZBa0U4a3ZmZFd4QlhWak1ERkI3WHVjWW9ubUVaMVczVHJlYXciLCJhbGciOiJSUzI1NiIsIng1dCI6Im5PbzNaRHJPRFhFSzFqS1doWHNsSFJfS1hFZyIsImtpZCI6Im5PbzNaRHJPRFhFSzFqS1doWHNsSFJfS1hFZyJ9.eyJhdWQiOiJodHRwczovL2dyYXBoLm1pY3Jvc29mdC5jb20iLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC84MDhiN2NlNS05OTI0LTQ0MWUtYWU1My0zZTQyZmQ4YTZkNDMvIiwiaWF0IjoxNjE2NzAzNDk2LCJuYmYiOjE2MTY3MDM0OTYsImV4cCI6MTYxNjcwNzM5NiwiYWlvIjoiRTJaZ1lEZ2VxOC9semRpU1phTXZjUEw1bVF2ckFBPT0iLCJhcHBfZGlzcGxheW5hbWUiOiJBbkF1dGhMYWItQ2xpZW50Q3JlZGVudGlhbEZsb3ciLCJhcHBpZCI6IjJiZmRkYWY0LTA2NzMtNDlhNy1hNjE1LTJiNmJiM2FjYmYzMSIsImFwcGlkYWNyIjoiMSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzgwOGI3Y2U1LTk5MjQtNDQxZS1hZTUzLTNlNDJmZDhhNmQ0My8iLCJpZHR5cCI6ImFwcCIsIm9pZCI6ImE1ZGFmYWZjLWI2ZDAtNDlkMy1hZTZjLWU5MzNkZTkxZjJhZCIsInJoIjoiMC5BVUVBNVh5TGdDU1pIa1N1VXo1Q19ZcHRRX1RhX1N0ekJxZEpwaFVyYTdPc3Z6RkJBQUEuIiwic3ViIjoiYTVkYWZhZmMtYjZkMC00OWQzLWFlNmMtZTkzM2RlOTFmMmFkIiwidGVuYW50X3JlZ2lvbl9zY29wZSI6Ik9DIiwidGlkIjoiODA4YjdjZTUtOTkyNC00NDFlLWFlNTMtM2U0MmZkOGE2ZDQzIiwidXRpIjoiY3c4SmtxcU42VVc3ekgyM0pGaXFBQSIsInZlciI6IjEuMCIsInhtc190Y2R0IjoxNTMwODM0MzY4fQ.I66nskL-XZ3wRZmPuM7qTMKR5LJk3-Ql2YbUylHeI4E9btjN_aQ_x4d2jajG2RGUMG9m2kMRrxmH5CE5Me3oIbbfOCpidZLUiOVkW_sLa1l7ny25JRVdxjCYND5epNCnxxQgSQHC2g7rydnTgxXrCNZF-rmvgPeU_TWB7bCjWgXX99jRevZPwZABmgi8J2KrGItk-YYpu_XV32PR3KuWKp3K9Gemp6d-dXQ6QRf9kVBP-RUV3myEvZ6ElaAJDMllHjDNAvzHfbYms9QFMepWakR9XWXqRl04edLnHsmH6OJnbBRH61VctXdqYjpEZiGHDll8L6DNBm2PXG_xmGshEA
```

We now have our **Access Token** and cause use this to access the User.Read Microsoft Graph Endpoint.

We can decode our **Access Token** to see what's inside. 

![JWT Decode](/img/5-AuthFlows-AuthGrantFlowJWTDecoded.png)


```Javascript
{
  "typ": "JWT",
  "nonce": "STv_yEfAkE8kvfdWxBXVjMDFB7XucYonmEZ1W3Treaw",
  "alg": "RS256",
  "x5t": "nOo3ZDrODXEK1jKWhXslHR_KXEg",
  "kid": "nOo3ZDrODXEK1jKWhXslHR_KXEg"
}.{
  "aud": "https://graph.microsoft.com",
  "iss": "https://sts.windows.net/808b7ce5-9924-441e-ae53-3e42fd8a6d43/",
  "iat": 1616703496,
  "nbf": 1616703496,
  "exp": 1616707396,
  "aio": "E2ZgYDgeq8/lzdiSZaMvcPL5mQvrAA==",
  "app_displayname": "AnAuthLab-ClientCredentialFlow",
  "appid": "2bfddaf4-0673-49a7-a615-2b6bb3acbf31",
  "appidacr": "1",
  "idp": "https://sts.windows.net/808b7ce5-9924-441e-ae53-3e42fd8a6d43/",
  "idtyp": "app",
  "oid": "a5dafafc-b6d0-49d3-ae6c-e933de91f2ad",
  "rh": "0.AUEA5XyLgCSZHkSuUz5C_YptQ_Ta_StzBqdJphUra7OsvzFBAAA.",
  "sub": "a5dafafc-b6d0-49d3-ae6c-e933de91f2ad",
  "tenant_region_scope": "OC",
  "tid": "808b7ce5-9924-441e-ae53-3e42fd8a6d43",
  "uti": "cw8JkqqN6UW7zH23JFiqAA",
  "ver": "1.0",
  "xms_tcdt": 1530834368
}.[Signature]
```