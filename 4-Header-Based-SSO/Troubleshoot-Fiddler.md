# Troubelshooting with Fiddler

Reference [Work with existing on-premises proxy servers and Azure Active Directory | Microsoft Docs](https://docs.microsoft.com/en-us/azure/active-directory/manage-apps/application-proxy-configure-connectors-with-proxy-servers#step-1-add-the-required-registry-value-to-the-server)

1. Run a **Command Prompt** as administrator and run the command netsh winhttp set proxy http://127.0.0.1:8888
2. Run **Regedit** and set UseDefaultProxyForBackendRequests with the value 1 under HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft AAD App Proxy Connector **DWORD**
3. In the file "ApplicationProxyConnectorService.exe.config" (X:\Program Files\Microsoft AAD App Proxy Connector) you should verify, if you can find the <proxy tag. If it's there and it specifies a proxy setting, leave it as it is. In any other cases, please ensure that you add (or correct the existing entry) these lines:
```
<system.net>
    <defaultProxy enabled="false"></defaultProxy>
</system.net>
```
4. Run **Fiddler**. Click on **Tools** in the upper menu -> **Options**... -> **HTTPS tab** -> Check the checkbox **Decrypt HTTPS traffic** -> Install the certificates -> OK
5. Restart the Microsoft AAD Application Proxy Connector service.

![Regedit](/img/4-HBSSO-Troubleshoot-Regedit.png)

**Fiddler Trace**

![Regedit](/img/4-HBSSO-Troubleshoot-FiddlerTrace.png)