# SAML On-Premises App with ADFS

## Install IIS and SimpleSAML
1. Download SimpleSAMLphp 
2. Extract to a new directory e.g. C:\inetpub\simplesaml
3. Create a new site e.g. saml
4. Create a virtual directory and point to simplesaml\www

![Install IIS](/img/2-AnPrem-SAML-IISInstall.png)

5. Update config.php

```
'technicalcontact_name' => 'Administrator',
'technicalcontact_email' => 'admin@coprtech4.com',
'timezone' => 'Australia/Sydney',
'secretsalt' => 'some random string',
'auth.adminpassword' => 'some other random string :P ',
'admin.protectindexpage' => true, 
'admin.protectmetadata' => false,
'session.cookie.secure' => true,
```

6. Browse to [https://saml.coprtech4.com/simplesaml](https://saml.coprtech4.com/simplesaml) to confirm SimpleSAML is accessible.

    ![SimpleSAMLSuccess](/img/2-AnPrem-SAML-SimpleSAMLSuccess.png)

## Confirgure SimpleSAML and ADFS

1. Get ADFS xml - [https://adfs.coprtech4.com/FederationMetadata/2007-06/FederationMetadata.xml](https://adfs.coprtech4.com/FederationMetadata/2007-06/FederationMetadata.xml)
2. Convert the XML to a format SimpleSAML can use
    - Open SimpleSAML federation tab

    ![SimpleSAMLFederation](/img/2-AnPrem-SAML-SimpleSAMLFederation.png)

    - Click XML to SimpleSAMLphp metadata converter
    - Upload or paste FederationMetadata.xml and parse
    - Copy the **saml20-idp-remote** code
    - Edit the metadata file **saml20-idp-remote.php** in the SimpleSaml directory
    - Paste the parsed code and save

    ![SimpleSAMLFederationXML](/img/2-AnPrem-SAML-SimpleSAMLFederationXML.png)

### Configure SimpleSAML Service Provider - Our App!    

1. Edit authsources.php from simplesaml\config directory
2. Use the examples in the file to create the new SP

    ![SimpleSAMLAuthSources](/img/2-AnPrem-SAML-SimpleSAMLAuthSources.png)

3. Create a new cert to be used by the SP
    - openssl req -x509 -nodes -sha256 -days 730 -newkey rsa:2048 -keyout my.key -out my.pem
4. Copy **my.key** and **my.pem** to the SimpleSAML\cert folder
5. Return to the SimpleSAML web interface to confirm setup https://saml.coprtech4.com/simplesaml/module.php/core/frontpage_federation.php#
    Notice the new metadata **saml-app-coprtech4-sp**

    ![SimpleSAMLFederationSP](/img/2-AnPrem-SAML-SimpleSAMLFederationSP.png)

    Make note of the URL https://saml.coprtech4.com/simplesaml/module.php/saml/sp/metadata.php/saml-app-coprtech4-sp

### Configure ADFS Relying Party Trust

1. Open ADFS management
2. Add Replying Part Trust

    ![ADFSRelyingParty](/img/2-AnPrem-SAML-ADFSRelyingParty.png)

3. Create some claims

    ![ADFSClaims](/img/2-AnPrem-SAML-ADFSClaims.png)

4. Create **Transform an Incoming Claim** rule

    ![ADFSTransformations](/img/2-AnPrem-SAML-ADFSTransformations.png)

    ![ADFSIssuance](/img/2-AnPrem-SAML-ADFSIssuance.png)

## Create Our App

1. Create index.php in our IIS website root directory. Note the saml-app-coprtech4-sp is referenced
```php
<?php
require_once (dirname(__FILE__) . '/../simplesaml/lib/_autoload.php');
$as = new SimpleSAML_Auth_Simple('saml-app-coprtech4-sp');
$as->requireAuth();
 
$attributes = $as->getAttributes();
 
?>
<!DOCTYPE html>
<html>
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Index Page</title>
</head>
<body>
    <h2>Index Page</h2>
    <h3>Welcome <strong>Authenticated User</strong>!</h3>
    <h4>Claim list:</h4>
<?php
echo '<pre>';
print_r($attributes);
echo '</pre>';
 
echo '<a href="/logout.php">Logout</a>';
 
?>
</body>
</html>
```

2. 	2. Create logout.php in the IIS website root directory
```php
<?php
 
require_once (dirname(__FILE__) . '/../simplesaml/lib/_autoload.php');
 
$as = new SimpleSAML_Auth_Simple('saml-app-coprtech4-sp');
 
$as->logout(array(
    'ReturnTo' => 'https://saml.coprtech4.com',
    'ReturnStateParam' => 'LogoutState',
    'ReturnStateStage' => 'MyLogoutState',
));
?>
```
3. Test the app - Navigate to https://saml.coprtech4.com/

    ![TestWeb01](/img/2-AnPrem-SAML-TestWeb01.png)

4. You will be redirected to ADFS. Login.

    ![TestWeb02](/img/2-AnPrem-SAML-TestWeb02.png)

4. 	5. On successful login, ADFS will redirect you back to the app with the claims
     The app prints out the claims

    ![TestWeb03](/img/2-AnPrem-SAML-TestWeb03.png)