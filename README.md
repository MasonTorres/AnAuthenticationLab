# An Authentication Lab
Let's start with an on-premises environment that synchronises accounts to Azure Active Directory. We will add some applications that authenticate against Active Directory using AD FS - SAML, WSFed and OpenID protocols.

Our lab looks a little like this and will grow as we add capabilities.

## Order of Lab Components
![Lab Process](img/Lab-Order.png)

## Overview of Lab environment
![Lab Overview](img/Lab-Overview.png)

### Prerequisits
Begin by completing the prerequisites to get our base environment setup
- 4x Virtual machines
- Domain Name
- SSL certificate 
- Azure AD tenant
- External DNS

### Build Your Environment
1. Azure Active Directory Connect
2. [On-Premise applications](2-On-Premise-Apps-AD-Auth/readme.md) - Using SAML, WSFED and OpenID with ADFS
    - [SAML App](2-On-Premise-Apps-AD-Auth/OnPremApp-SAML-ADFS.md)
        - [Fiddler Trace](2-On-Premise-Apps-AD-Auth/OnPremApp-SAML-ADFS-Fiddler.md)
        - [SAML Request / SAML Response](2-On-Premise-Apps-AD-Auth/OnPremApp-SAML-Response.md)
    - WS-Federation App
        - Fiddler Trace
    - OpenID App
        - [Fiddler Trace](2-On-Premise-Apps-AD-Auth/OnPremApp-OIDC-ADFS-Fiddler.md)
3. [On-Premise applications](3-On-Premise-Apps-Azure-AD-Auth/readme.md) - Using SAML, WSFED and OpenID with Azure AD
4. [On-Premise applications - Proxy an application using App Proxy and use Header Based SSO](4-Header-Based-SSO/readme.md)
    - [Troubleshooting with Fiddler](4-Header-Based-SSO/Troubleshoot-Fiddler.md)
5. Authentication flows - OAuth and OpenID
    - OpenID
    - Password / ROPC
    - [Client Credential](5-Authentication-Flows/Tokens/Client-Credential-Flow.md)
    - [Implicit Grant Flow](5-Authentication-Flows/Tokens/Implicit-Grant-Flow.md)
    - [Auth Grant Flow](5-Authentication-Flows/Tokens/Auth-Grant-Flow.md)
        - [Auth Grant Flow PKCE SPA (Signle Page App)](5-Authentication-Flows/Tokens/Auth-Grant-Flow-SPA-with-PKCE.md)
    - [Device Code Flow](5-Authentication-Flows/Tokens/Device-Code-Flow.md)
6. Protecting an API
7. B2C
    - Custom Policies

## Our Test Users
![Lab Users](img/Lab-Users.png)