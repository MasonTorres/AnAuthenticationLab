Reference [Microsoft identity platform and OAuth 2.0 authorization code flow - Microsoft identity platform | Microsoft Docs](https://docs.microsoft.com/en-us/azure/active-directory/develop/v2-oauth2-auth-code-flow)

### Get Token
Request a token using HTTP GET
```
https://login.microsoftonline.com/808b7ce5-9924-441e-ae53-3e42fd8a6d43/oauth2/v2.0/authorize?
client_id=e2a55bd5-ae5c-41c0-8243-de710e5bf8a2
&response_type=code
&redirect_uri=https%3A%2F%2Fjwt.ms
&response_mode=query
&scope=User.Read Mail.Send Mail.Read
&state=12345
```

![Lab Overview](https://github.com/MasonTorres/AnAuthenticationLab/blob/master/img/AuthFlows-AuthGrantFlowGet.png)

Our code is then returned
![Lab Overview](https://github.com/MasonTorres/AnAuthenticationLab/blob/master/img/AuthFlows-AuthGrantFlowCode.png)

### Get Access Token
Post
![Lab Overview](https://github.com/MasonTorres/AnAuthenticationLab/blob/master/img/AuthFlows-AuthGrantFlowGetAccessToken01.png)

![Lab Overview](https://github.com/MasonTorres/AnAuthenticationLab/blob/master/img/AuthFlows-AuthGrantFlowGetAccessToken02.png)

## PKCE
### Create Code Verity using PowerShell
```powershell
Add-Type -AssemblyName System.Web
$RandomNumberGenerator = New-Object System.Security.Cryptography.RNGCryptoServiceProvider
$Bytes = New-Object Byte[] 128
$RandomNumberGenerator.GetBytes($Bytes)
$codeVerify = [System.Web.HttpServerUtility]::UrlTokenEncode($Bytes)
```

### Create Code Challenge using PowerShell
```powershell
$hasher = [System.Security.Cryptography.HashAlgorithm]::Create('sha256')
$hash = $hasher.ComputeHash([System.Text.Encoding]::UTF8.GetBytes($codeVerify))
$codeChallenge = [System.Convert]::ToBase64String($hash)
$codeChallenge = $codeChallenge.Split('=')[0]
$codeChallenge = $codeChallenge.Replace('+', '–')
$codeChallenge = $codeChallenge.Replace('/', '_')
```