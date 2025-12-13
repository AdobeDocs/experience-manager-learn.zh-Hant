---
title: 電子郵件服務
description: 瞭解如何設定AEM as a Cloud Service以使用輸出埠連線電子郵件服務。
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
jira: KT-9353
thumbnail: KT-9353.jpeg
exl-id: 5f919d7d-e51a-41e5-90eb-b1f6a9bf77ba
duration: 76
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '330'
ht-degree: 0%

---

# 電子郵件服務

透過設定AEM的`DefaultMailService`使用進階網路輸出埠，從AEM as a Cloud Service傳送電子郵件。

由於（大部分）郵件服務不會透過HTTP/HTTPS執行，因此必須代理從AEM as a Cloud Service連線至郵件服務。

+ `smtp.host`已設定為OSGi環境變數`$[env:AEM_PROXY_HOST;default=proxy.tunnel]`，因此會透過輸出進行路由。
   + `$[env:AEM_PROXY_HOST]`是AEM as a Cloud Service對應到內部`proxy.tunnel`主機的保留變數。
   + 請勿嘗試透過Cloud Manager設定`AEM_PROXY_HOST`。
+ `smtp.port`設定為對應到目的地電子郵件服務主機和連線埠的`portForward.portOrig`連線埠。 此範例使用對應： `AEM_PROXY_HOST:30465` → `smtp.sendgrid.com:465`。
   + `smpt.port`設定為`portForward.portOrig`連線埠，而不是SMTP伺服器的實際連線埠。 `smtp.port`與`portForward.portOrig`連線埠之間的對應是由Cloud Manager `portForwards`規則所建立（如下所示）。

由於密碼不得儲存在程式碼中，因此最好使用[機密OSGi設定變數](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#secret-configuration-values)、使用AIO CLI或Cloud Manager API來設定電子郵件服務的使用者名稱和密碼。

一般會使用[彈性連線埠輸出](../flexible-port-egress.md)來滿足與電子郵件服務整合，除非需要`allowlist` Adobe IP，在這種情況下可以使用[專用輸出IP位址](../dedicated-egress-ip-address.md)。

此外，檢閱[傳送電子郵件](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines.html#sending-email)的AEM檔案。

## 進階網路支援

下列進階網路選項支援下列程式碼範例。

在執行本教學課程之前，請確定已設定[適當的](../advanced-networking.md#advanced-networking)進階網路設定。

| 沒有進階網路 | [彈性連線埠輸出](../flexible-port-egress.md) | [專用輸出IP位址](../dedicated-egress-ip-address.md) | [虛擬私人網路](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✔ | ✔ |

## OSGi設定

此OSGi設定範例透過`portForwards`enableEnvironmentAdvancedNetworkingConfiguration[作業的下列Cloud Manager ](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration)規則，將AEM的Mail OSGi Service設定為使用外部郵件服務。

```json
...
"portForwards": [{
    "name": "smtp.mymail.com",
    "portDest": 465,
    "portOrig": 30465
}]
...
```

+ `ui.config/src/jcr_root/apps/wknd-examples/osgiconfig/config/com.day.cq.mailer.DefaultMailService.cfg.json`

根據您的電子郵件提供者（例如[等）要求，設定AEM的](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines.html#sending-email)DefaulMailService`smtp.ssl`。

```json
{
    "smtp.host": "$[env:AEM_PROXY_HOST;default=proxy.tunnel]",
    "smtp.port": "30465",
    "smtp.user": "$[env:EMAIL_USERNAME;default=myApiKey]",
    "smtp.password": "$[secret:EMAIL_PASSWORD]",
    "from.address": "noreply@wknd.site",
    "smtp.ssl": true,
    "smtp.starttls": false, 
    "smtp.requiretls": false,
    "debug.email": false,
    "oauth.flow": false
}
```

可以使用下列其中一項，為每個環境設定`EMAIL_USERNAME`和`EMAIL_PASSWORD` OSGi變數和密碼：

+ [Cloud Manager環境設定](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html)
+ 或使用`aio CLI`命令

  ```shell
  $ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret EMAIL_USERNAME "myApiKey" --secret EMAIL_PASSWORD "password123"
  ```
