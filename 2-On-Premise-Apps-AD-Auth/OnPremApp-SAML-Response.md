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

# Example SAML Response

```xml
<samlp:Response ID="_ffa864ef-7112-4f2c-a03a-af36357ce03d" Version="2.0" IssueInstant="2021-03-11T21:22:33.397Z" Destination="https://aadsaml.coprtech4.com"
    xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://sts.windows.net/808b7ce5-9924-441e-ae53-3e42fd8a6d43/</Issuer>
    <samlp:Status>
        <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success"/>
    </samlp:Status>
    <Assertion ID="_799e93d1-4163-4acc-a77f-51b5a3e73600" IssueInstant="2021-03-11T21:22:33.392Z" Version="2.0"
        xmlns="urn:oasis:names:tc:SAML:2.0:assertion">
        <Issuer>https://sts.windows.net/808b7ce5-9924-441e-ae53-3e42fd8a6d43/</Issuer>
        <Signature xmlns="http://www.w3.org/2000/09/xmldsig#">
            <SignedInfo>
                <CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
                <SignatureMethod Algorithm="http://www.w3.org/2000/09/xmldsig#rsa-sha1"/>
                <Reference URI="#_799e93d1-4163-4acc-a77f-51b5a3e73600">
                    <Transforms>
                        <Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature"/>
                        <Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
                    </Transforms>
                    <DigestMethod Algorithm="http://www.w3.org/2000/09/xmldsig#sha1"/>
                    <DigestValue>NJf61MNj0aCsg9bJzIUZp49xbiE=</DigestValue>
                </Reference>
            </SignedInfo>
            <SignatureValue>0Zj8/fhdAVBz7rWTkqQm+QBlA0ozqsRGCzQxx4GHTmQUl50ztwjcsX+qlS1d9l0Py8j2ixnjIidz3pJcQWF01OCFgC0HLknYvAcZ08iG/Pr8Fhc0PuQdEKXqkA6pPK4ghx4L98VLKCEShUVOvNeHcfNBNgrZqw4g6+yU4YNUHRMoX0mMkDHZs0T4d1uticgUoiY3iMXHtFAyZEGG4kyO/h/74iXfO2fiavcZaSUIxBCzkwSoujx9wzFcCkDd1vtNEXienXpwVlagvSUOx4nXXkV8e0D6fZ/HkBKQGUq8IhDKnnMzKw/K89G9A4T/oU90750U9MSFBDAYbKgAtt9AAA==</SignatureValue>
            <KeyInfo>
                <X509Data>
                    <X509Certificate>MIIC8DCCAdigAwIBAgIQScniobrmKoBL4dua6JAvYzANBgkqhkiG9w0BAQsFADA0MTIwMAYDVQQDEylNaWNyb3NvZnQgQXp1cmUgRmVkZXJhdGVkIFNTTyBDZXJ0aWZpY2F0ZTAeFw0yMTAzMDIwMjM0MTJaFw0yNDAzMDIwMjM0MDdaMDQxMjAwBgNVBAMTKU1pY3Jvc29mdCBBenVyZSBGZWRlcmF0ZWQgU1NPIENlcnRpZmljYXRlMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA2HyPqUzkeRCMNV2MsHPcDnS0MZB88gSeU+s9fybs2O+Xu+qSdWK+YyJwELJrBNXRiXfKWzAQdkUBD8TVGz2nQd2x7zLPthToLVc5nqZF2FOjKFX0JXj+XszxO2kz6vP/i+OUSvr6RqAtiSjhygaUWLTWDXwZEXhvv2jvBmjoL3C2qsyxgkQ8HBRPWGrX6Dt2mgQkaCU+qOG2hS5Impu4GJerZXdI/iFkf1zgwNX1EuVnj7rU9gqJOQNT14GSepO1j39TwKy2Zbjc+yOfbcmBN8TkC48Aee2nsJmnWCmjC5BI5BT4URSYmQXBCC0fObRMx0FIG0zWrm3jk+i/2kHyxQIDAQABMA0GCSqGSIb3DQEBCwUAA4IBAQCD/za9GNC9PDZ/5l4TFVpETpVMZ/78BagUd8WsmdVmSRF6LVheut+1AwHuwryNqF4FFztFgifetwBgyXznOR3wYfVEdwG4lDf/ZAN8ZuBuGDTCQ/oIKy+tt6LTJHY1IFLJdzXPERVluMN0na2h8xF1c1uKGuMOi1gQFE3cgj+ocX7+WXZEgTPhqmftyqb/JfQgPKNt25kIDJNqlqiSISfxJFVwgKBbt4hS5LFsOMaixUtkLyUIam1F4syzgPsCtI4CWlRUTDiV9fUSVPznKyZ96JYAbN/fhltPd2V6KjPH/1LfwGNCi+PKLMCjEZX+Hg3/MDXo5rK2RaHgt9bFrVJm</X509Certificate>
                </X509Data>
            </KeyInfo>
        </Signature>
        <Subject>
            <NameID Format="urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress">cloud@coprtech4.tk</NameID>
            <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
                <SubjectConfirmationData NotOnOrAfter="2021-03-11T22:22:33.152Z" Recipient="https://aadsaml.coprtech4.com"/>
            </SubjectConfirmation>
        </Subject>
        <Conditions NotBefore="2021-03-11T21:17:33.152Z" NotOnOrAfter="2021-03-11T22:22:33.152Z">
            <AudienceRestriction>
                <Audience>spn:9b749cc9-004a-419b-a532-31da51bff999</Audience>
            </AudienceRestriction>
        </Conditions>
        <AttributeStatement>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/tenantid">
                <AttributeValue>808b7ce5-9924-441e-ae53-3e42fd8a6d43</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
                <AttributeValue>0ab5560f-4d23-4ec1-9d7b-7bf99250d1bd</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/displayname">
                <AttributeValue>Cloud</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/role">
                <AttributeValue>75def0d7-a738-431c-88f3-94db333a5d29</AttributeValue>
                <AttributeValue>297dde60-9a16-4814-ab76-9c7a2f8c1275</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/identityprovider">
                <AttributeValue>https://sts.windows.net/808b7ce5-9924-441e-ae53-3e42fd8a6d43/</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/claims/authnmethodsreferences">
                <AttributeValue>http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress">
                <AttributeValue>cloud@coprtech4.tk</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
                <AttributeValue>cloud@coprtech4.tk</AttributeValue>
            </Attribute>
            <Attribute Name="Role Name">
                <AttributeValue>cloudistrator</AttributeValue>
            </Attribute>
        </AttributeStatement>
        <AuthnStatement AuthnInstant="2021-03-10T22:28:53.514Z" SessionIndex="_799e93d1-4163-4acc-a77f-51b5a3e73600">
            <AuthnContext>
                <AuthnContextClassRef>urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
            </AuthnContext>
        </AuthnStatement>
    </Assertion>
</samlp:Response>
```