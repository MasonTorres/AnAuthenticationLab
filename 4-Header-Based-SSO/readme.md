# Header-based Single sign-on

## Overview
Reference [Header-based single sign-on for on-premises apps with Azure AD App Proxy | Microsoft Docs](https://docs.microsoft.com/en-us/azure/active-directory/manage-apps/application-proxy-configure-single-sign-on-with-headers)

![Overview](/img/4-HBSSO-Overview.png)

## App Proxy Setup

![App Proxy Setup](/img/4-HBSSO-AppProxySetup.png)

### Add some claims

![App Proxy Claims](/img/4-HBSSO-AppProxyClaims.png)

## Configure our sample IIS application

1. Create new site
2. Create index.php
```
<?php
    $headers =  getallheaders();
    foreach($headers as $key=>$val){
        echo $key . ': ' . $val . '<br>';
    }
?>
```

**Load our test website**

![Inspect Our Headers](/img/4-HBSSO-TestWebsite.png)