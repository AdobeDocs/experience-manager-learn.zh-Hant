---
title: 電子郵件服務
description: 了解如何設定AEMas a Cloud Service，以使用輸出埠連線電子郵件服務。
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9353
thumbnail: KT-9353.jpeg
exl-id: 5f919d7d-e51a-41e5-90eb-b1f6a9bf77ba
source-git-commit: c34c27955dbc084620ac4dd811ba4051ea83f447
workflow-type: tm+mt
source-wordcount: '367'
ht-degree: 0%

---

# 電子郵件服務

透過設定AEM從AEMas a Cloud Service傳送電子郵件 `DefaultMailService` 使用高級網路輸出埠。

由於（大部分）郵件服務不會透過HTTP/HTTPS執行，因此必須代理從AEMas a Cloud Service連線至郵件服務。

+ `smtp.host` 設為OSGi環境變數時 `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` 所以它會穿過出口。
   + `$[env:AEM_PROXY_HOST]` 是AEMas a Cloud Service對應至內部的保留變數 `proxy.tunnel` 主機。
   + 請勿嘗試設定 `AEM_PROXY_HOST` 透過Cloud Manager。
+ `smtp.port` 設為 `portForward.portOrig` 映射到目標電子郵件服務的主機和埠的埠。 此範例使用對應： `AEM_PROXY_HOST:30465` → `smtp.sendgrid.com:465`.
   + 此 `smpt.port` 設為 `portForward.portOrig` 埠，而不是SMTP伺服器的實際埠。 之間的對應 `smtp.port` 和 `portForward.portOrig` 埠由Cloud Manager建立 `portForwards` 規則（如下所示）。

由於機密不得儲存在代碼中，因此最好使用以下方式提供電子郵件服務的用戶名和密碼： [機密OSGi設定變數](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#secret-configuration-values)，請使用AIO CLI或Cloud Manager API進行設定。

通常， [靈活的埠輸出](../flexible-port-egress.md) 用於滿足與電子郵件服務的整合，除非有必要 `allowlist` AdobeIP，在這種情況下 [專用的輸出ip地址](../dedicated-egress-ip-address.md) 可供使用。

此外，請檢閱AEM檔案，位於 [發送電子郵件](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines.html#sending-email).

## 高級網路支援

下列進階網路選項支援下列程式碼範例。

確保 [適當](../advanced-networking.md#advanced-networking) 在完成本教學課程之前，已設定進階網路設定。

| 無高級網路 | [靈活的埠輸出](../flexible-port-egress.md) | [專用的輸出IP地址](../dedicated-egress-ip-address.md) | [虛擬專用網](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✔ | ✔ |

## OSGi配置

此OSGi設定範例會透過下列Cloud Manager將AEM Mail OSGi服務設定為使用外部郵件服務 `portForwards` 規則 [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) 操作。

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

設定AEM [DefaulMailService](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines.html#sending-email) 如電子郵件提供者所要求(例如 `smtp.ssl`等)。

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

此 `EMAIL_USERNAME` 和 `EMAIL_PASSWORD` 您可以使用下列任一項，依環境設定OSGi變數和機密：

+ [Cloud Manager環境配置](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html)
+ 或使用 `aio CLI` 命令

   ```shell
   $ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret EMAIL_USERNAME "myApiKey" --secret EMAIL_PASSWORD "password123"
   ```
