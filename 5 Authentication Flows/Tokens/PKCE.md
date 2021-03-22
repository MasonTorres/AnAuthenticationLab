## Create Code Verity using PowerShell
```
Add-Type -AssemblyName System.Web
$RandomNumberGenerator = New-Object System.Security.Cryptography.RNGCryptoServiceProvider
$Bytes = New-Object Byte[] 128
$RandomNumberGenerator.GetBytes($Bytes)
$codeVerify = [System.Web.HttpServerUtility]::UrlTokenEncode($Bytes)
```

## Create Code Challenge using PowerShell
```
$hasher = [System.Security.Cryptography.HashAlgorithm]::Create('sha256')
$hash = $hasher.ComputeHash([System.Text.Encoding]::UTF8.GetBytes($codeVerify))
$codeChallenge = [System.Convert]::ToBase64String($hash)
$codeChallenge = $codeChallenge.Split('=')[0]
$codeChallenge = $codeChallenge.Replace('+', '–')
$codeChallenge = $codeChallenge.Replace('/', '_')
```