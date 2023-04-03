---
title: 在AEM中使用SSL精靈
description: Adobe Experience Manager的SSL設定精靈，讓設定AEM執行個體透過HTTPS執行變得更輕鬆。
seo-description: Adobe Experience Manager's SSL setup wizard to make it easier to set up an AEM instance to run over HTTPS.
version: 6.4, 6.5
topics: security, operations
activity: use
audience: administrator
doc-type: technical video
uuid: 82a6962e-3658-427a-bfad-f5d35524f93b
discoiquuid: 9e666741-0f76-43c9-ab79-1ef149884686
topic: Security
role: Developer
level: Beginner
exl-id: 4e69e115-12a6-4a57-90da-b91e345c6723
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 0%

---

# 在AEM中使用SSL精靈

Adobe Experience Manager的SSL設定精靈，讓設定AEM執行個體透過HTTPS執行變得更輕鬆。

>[!VIDEO](https://video.tv.adobe.com/v/17993?quality=12&learn=on)

開啟 __SSL配置嚮導__ 可導覽至 __AEM製作>工具>安全性> SSL設定__.

>[!NOTE]
>
>對於托管環境，IT部門最好提供CA信任的證書和密鑰。
>
>自簽名證書僅用於開發用途。

## 私密金鑰和自簽名證書下載

下列zip包含 [!DNL DER] 和 [!DNL CRT] 在localhost上設定AEM SSL所需，且僅用於本機開發用途的檔案。

此 [!DNL DER] 和 [!DNL CERT] 提供檔案是為了方便，並使用以下產生私密金鑰和自簽名憑證一節中概述的步驟產生。

如有需要，憑證密碼片語為 **管理員**.

localhost — 私密金鑰和自行簽署的certificate.zip（2028年7月到期）

[下載憑證檔案](assets/use-the-ssl-wizard/certificate.zip)

## 私鑰和自簽名證書生成

上述影片說明使用自行簽署憑證之AEM製作執行個體上SSL的設定和設定。 以下命令使用 [[!DNL OpenSSL]](https://www.openssl.org/) 可產生要在精靈的步驟2中使用的私密金鑰和憑證。

```shell
### Create Private Key
$ openssl genrsa -aes256 -out localhostprivate.key 4096

### Generate Certificate Signing Request using private key
$ openssl req -sha256 -new -key localhostprivate.key -out localhost.csr -subj '/CN=localhost'

### Generate the SSL certificate and sign with the private key, will expire one year from now
$ openssl x509 -req -extfile <(printf "subjectAltName=DNS:localhost") -days 365 -in localhost.csr -signkey localhostprivate.key -out localhost.crt

### Convert Private Key to DER format - SSL wizard requires key to be in DER format
$ openssl pkcs8 -topk8 -inform PEM -outform DER -in localhostprivate.key -out localhostprivate.der -nocrypt
```
