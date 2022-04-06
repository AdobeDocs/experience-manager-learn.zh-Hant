---
title: 電子郵件服務
description: 瞭解如何配置AEMas a Cloud Service以使用出口端連接電子郵件服務。
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9353
thumbnail: KT-9353.jpeg
exl-id: 5f919d7d-e51a-41e5-90eb-b1f6a9bf77ba
source-git-commit: 4d3256cee67183803692cccc7f17ca1a0820e05d
workflow-type: tm+mt
source-wordcount: '268'
ht-degree: 0%

---

# 電子郵件服務

通過配置從AEMas a Cloud Service發送電子郵AEM件 `DefaultMailService` 使用高級網路出口端。

由於（大多數）郵件服務不通過HTTP/HTTPS運行，因此必須代理從AEMas a Cloud Service到郵件服務的連接。

+ `smtp.host` 設定為OSGi環境變數 `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` 所以它會穿過出口。
+ `smtp.port` 設定為 `portForward.portOrig` 映射到目標電子郵件服務的主機和埠的埠。 此示例使用映射： `AEM_PROXY_HOST:30002` → `smtp.sendgrid.com:465`。

由於機密不能儲存在代碼中，因此最好使用以下方法提供電子郵件服務的用戶名和密碼 [秘密OSGi配置變數](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#secret-configuration-values)，使用AIO CLI或Cloud Manager API設定。

通常， [柔性埠出口](../flexible-port-egress.md) 用於滿足與電子郵件服務的整合，除非 `allowlist` AdobeIP，在這種情況下 [專用出口ip地址](../dedicated-egress-ip-address.md) 可使用。

## 高級網路支援

以下高級網路選項支援以下代碼示例。

| 無高級網路 | [靈活的埠出口](../flexible-port-egress.md) | [專用出口IP地址](../dedicated-egress-ip-address.md) | [虛擬專用網路](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✔ | ✔ |

## OSGi配置

此OSGi配置示例通AEM過以下Cloud Manager將Mail OSGi服務配置為使用外部郵件服務 `portForwards` 規則 [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) 的下界。

```json
...
"portForwards": [{
    "name": "smtp.sendgrid.com",
    "portDest": 465,
    "portOrig": 30002
}]
...
```

+ `ui.config/src/jcr_root/apps/wknd-examples/osgiconfig/config/com.day.cq.mailer.DefaultMailService.cfg.json`

配AEM置 [預設郵件服務](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines.html#sending-email) 按您的電子郵件提供商的要求(例如 `smtp.ssl`等)。

```json
{
    "smtp.host": "$[env:AEM_PROXY_HOST;default=proxy.tunnel]",
    "smtp.port": "30002",
    "smtp.user": "$[env:EMAIL_USERNAME;default=emailapikey]",
    "smtp.password": "$[secret:EMAIL_PASSWORD]",
    "from.address": "noreply@wknd.site",
    "smtp.ssl": true,
    "smtp.starttls": false, 
    "smtp.requiretls": false,
    "debug.email": false,
    "oauth.flow": false
}
```

以下 `aio CLI` 命令可用於根據每個環境設定OSGi機密：

```shell
$ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret EMAIL_USERNAME "apikey" --secret EMAIL_PASSWORD "password123"
```
