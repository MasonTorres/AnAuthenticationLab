# SAML Response

## Using Chrome - Dev Tools

Reference [SAML DevTools extension - Chrome Web Store (google.com)](https://chrome.google.com/webstore/detail/saml-devtools-extension/jndllhgbinhiiddokbeoeepbppdnhhio)

1. Install Dev Tolls
2. Pres F12 and use the SAML tab
3. Login to SAML app

![TestWeb01](/img/2-OnPrem-SAML-Response-Chrome.png)

## Using FireFox - SAML Tracer

Reference [SAML-tracer – Get this Extension for 🦊 Firefox (en-US) (mozilla.org)](https://addons.mozilla.org/en-US/firefox/addon/saml-tracer/)

1. Install SAML Tracer
2. Open SAML Tracer extenssion and login to SAML app

![TestWeb01](/img/2-OnPrem-SAML-Response-Firefox.png)

## Using PowerShell 

1. Grab the SAMLResponse from your browser Developer Tools and save to file as below

![TestWeb01](/img/2-OnPrem-SAML-Response-BrowserPowershell01.png)

2. Decode the base64 SAMLResponse from the browser

```Powershell
Using SAMLResponse from browser…
$inputFilePath = "C:\Users\masontorres\Downloads\samlResponse.txt"
$outputFilePath = "C:\Users\masontorres\Downloads\samlResponse2.txt"
$PEBytes = [System.Convert]::FromBase64String([IO.File]::ReadAllText($InputFilePath))  
[System.IO.File]::WriteAllBytes($outputFilePath, $PEBytes);
```

## Using NotePad++ to decode SAMLResponse

Paste into Notepad++
Plugins -> MIME Tools -> Base64 Decode

Use XML tools plugin to prettify to xml

![TestWeb01](/img/2-OnPrem-SAML-Response-BrowserPowershell02.png)