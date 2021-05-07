# Troubelshooting with Fiddler

**Access the WSFED application**

![WSFED-Fiddler](/img/2-OnPrem-WSFED-Fiddler06.png)

![WSFED-Fiddler](/img/2-OnPrem-WSFED-Fiddler01.png)

**Get redirected to ADFS to login**

![WSFED-Fiddler](/img/2-OnPrem-WSFED-Fiddler02.png)


**Submit ADFS login form**

![WSFED-Fiddler](/img/2-OnPrem-WSFED-Fiddler03.png)

![WSFED-Fiddler](/img/2-OnPrem-WSFED-Fiddler04.png)

**ADFS creates a webpage with a JavaScript redirect and automatically calls it on page load**

![WSFED-Fiddler](/img/2-OnPrem-WSFED-Fiddler05.png)

Below is an example HTML forms based POST request which gets called automatically when the page loads `window.setTimeout('document.forms[0].submit()', 0);`

You can see all the XML claims being sent with the POST request.


```html
<html>

<head>
    <title>Working...</title>
</head>

<body>
    <form method="POST" name="hiddenform" action="https://wsfed.coprtech4.com:443/"><input type="hidden" name="wa"
            value="wsignin1.0" /><input type="hidden" name="wresult"
            value="
                <t:RequestSecurityTokenResponse xmlns:t="http://schemas.xmlsoap.org/ws/2005/02/trust">
                    <t:Lifetime>
                        <wsu:Created xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">2021-05-07T04:05:03.552Z</wsu:Created>
                        <wsu:Expires xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">2021-05-07T05:05:03.552Z</wsu:Expires>
                    </t:Lifetime>
                    <wsp:AppliesTo xmlns:wsp="http://schemas.xmlsoap.org/ws/2004/09/policy">
                        <wsa:EndpointReference xmlns:wsa="http://www.w3.org/2005/08/addressing">
                            <wsa:Address>https://wsfed.coprtech4.com/</wsa:Address>
                        </wsa:EndpointReference>
                    </wsp:AppliesTo>
                    <t:RequestedSecurityToken>
                        <saml:Assertion MajorVersion="1" MinorVersion="1" AssertionID="_66d59392-1213-4c8e-a9b8-32cea94ce616" Issuer="http://adfs.coprtech4.com/adfs/services/trust" IssueInstant="2021-05-07T04:05:03.568Z"
                            xmlns:saml="urn:oasis:names:tc:SAML:1.0:assertion">
                            <saml:Conditions NotBefore="2021-05-07T04:05:03.552Z" NotOnOrAfter="2021-05-07T05:05:03.552Z">
                                <saml:AudienceRestrictionCondition>
                                    <saml:Audience>https://wsfed.coprtech4.com/</saml:Audience>
                                </saml:AudienceRestrictionCondition>
                            </saml:Conditions>
                            <saml:AttributeStatement>
                                <saml:Subject>
                                    <saml:SubjectConfirmation>
                                        <saml:ConfirmationMethod>urn:oasis:names:tc:SAML:1.0:cm:bearer</saml:ConfirmationMethod>
                                    </saml:SubjectConfirmation>
                                </saml:Subject>
                                <saml:Attribute AttributeName="name" AttributeNamespace="http://schemas.xmlsoap.org/ws/2005/05/identity/claims">
                                    <saml:AttributeValue>Paige Turner</saml:AttributeValue>
                                </saml:Attribute>
                                <saml:Attribute AttributeName="upn" AttributeNamespace="http://schemas.xmlsoap.org/ws/2005/05/identity/claims">
                                    <saml:AttributeValue>paige.turner@coprtech4.com</saml:AttributeValue>
                                </saml:Attribute>
                            </saml:AttributeStatement>
                            <saml:AuthenticationStatement AuthenticationMethod="urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport" AuthenticationInstant="2021-05-07T04:05:03.521Z">
                                <saml:Subject>
                                    <saml:SubjectConfirmation>
                                        <saml:ConfirmationMethod>urn:oasis:names:tc:SAML:1.0:cm:bearer</saml:ConfirmationMethod>
                                    </saml:SubjectConfirmation>
                                </saml:Subject>
                            </saml:AuthenticationStatement>
                            <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
                                <ds:SignedInfo>
                                    <ds:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
                                    <ds:SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256" />
                                    <ds:Reference URI="#_66d59392-1213-4c8e-a9b8-32cea94ce616">
                                        <ds:Transforms>
                                            <ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature" />
                                            <ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
                                        </ds:Transforms>
                                        <ds:DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256" />
                                        <ds:DigestValue>WTq2QjPIGiYRDcWL2u4eHzpxkWv/GSb5PAiJvvE5zG0=</ds:DigestValue>
                                    </ds:Reference>
                                </ds:SignedInfo>
                                <ds:SignatureValue>QLsLUwOH7FC1R6GA0g6CE0ROgw/JMfYJ74C6QvunCI+NBkkkT9Ik3TqwAACbnlvrt9DngIYbP3UrthPhoW8MDqfeDVL9RfJq1JcVrfHV6e7geZGD2y7MJ8/8KM6lGWOS+KyOLMpPV4GxSBmroP0hq7wzKwgAFQjtmzH7jk7SIwdxSl6sA4+2VWSylrE1fjIeqMoNQ+CuoSxvd4GnTJNbeY8eNK4qt7KoTWAD0vCUSIiPvVp0jIhX21uVwTwNhGgZt5fnR9ZL66uedNDID/CMNDf3luLG/Ry9DPf8guuG26J9Dw2i6q42RSTBHT/V8q8iarVxfu/okdS44FjdiCA1BQ==</ds:SignatureValue>
                                <KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
                                    <X509Data>
                                        <X509Certificate>MIIC4DCCAcigAwIBAgIQGDzEXHIXuLZGXKBy8y7uwzANBgkqhkiG9w0BAQsFADAsMSowKAYDVQQDEyFBREZTIFNpZ25pbmcgLSBhZGZzLmNvcHJ0ZWNoNC5jb20wHhcNMjEwMjA0MDAxODA4WhcNMjIwMjA0MDAxODA4WjAsMSowKAYDVQQDEyFBREZTIFNpZ25pbmcgLSBhZGZzLmNvcHJ0ZWNoNC5jb20wggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQC0IIZrloTX9A4PUG6S08D0hUK+YKFZUyH4Mvi7PWmnulOD7BWpV8wgZnYdBabswvtLhy9b6P1P9NRyxsDXOAnffi+y8uHlonpM6qIjmjGsFd8lUoy98SKDJUUD7M1oa2vYFG+rB7enhnbYKk2QTXN9JukJP6F5aNmgAf05lCeNlKnc0bT6h5LsgVF7JXAK1chPMD1/dHM4Z6wzwsUnct4cyExEMc+FuaBr1KDEDxBujYDV7EfRcQ1+IrAG/JiH1NYCqfWgYpDNY6jMJnMN3jOuDAk9gOYWWajIO1xdgQNpzhnqjKJZOcfoIorT+J0A4xUDyP0Bsl5dh28SIdBkuVEvAgMBAAEwDQYJKoZIhvcNAQELBQADggEBAFfiUrzklWC8bC77zCUfx89I+pMTDwDUIg5lkM4+17Q6nITbm6TjUe62luVZT8q4U2YlWQQReQ2wamtbhPN60WUZxsI1Dp+85wil8YDconD3Gqnl9cAcMHp4tyYuSu2YxtFCxNItbBvjKM9g3W525W8rn6/92Kd/sdecFjFuoAXbFiNNSq+Zbq6w8V1dFfasq43w/gJlhgRmYckAYnRsXeG/SVw8CxZ+tYlhOxPPj71AbSxx2cJfDti5ZhuciTmE94Tc8URUeW96+pYgAFvqFWdJJegpTB5ihy8gfHfzG9mI5ivWjGex7ddIig6QQC3TEkrIVtifhSozYTpYWYtlYzQ=</X509Certificate>
                                    </X509Data>
                                </KeyInfo>
                            </ds:Signature>
                        </saml:Assertion>
                    </t:RequestedSecurityToken>
                    <t:TokenType>urn:oasis:names:tc:SAML:1.0:assertion</t:TokenType>
                    <t:RequestType>http://schemas.xmlsoap.org/ws/2005/02/trust/Issue</t:RequestType>
                    <t:KeyType>http://schemas.xmlsoap.org/ws/2005/05/identity/NoProofKey</t:KeyType>
                </t:RequestSecurityTokenResponse>
            " /><input
            type="hidden" name="wctx"
            value="WsFedOwinState=oCoDJ9DKhNDN0AGEUs-qofDwzlgCoDhrhRRx1jZ4LKFEIAKYVt8bde3aGKSVcGu8XlJMhshudy5Nmdoe7lqlIUfLkWuNtgKcq9Iy86jAAxMM8Z9ODtD_NVRkPrt2dMls" /><noscript>
            <p>Script is disabled. Click Submit to continue.</p><input type="submit" value="Submit" />
        </noscript></form>
    <script language="javascript">window.setTimeout('document.forms[0].submit()', 0);</script>
</body>

</html>
```

**The ADFS redirect takes us back to the WSFED application, we can see the claims that were passed.**

![WSFED-Fiddler](/img/2-OnPrem-WSFED-Fiddler07.png)