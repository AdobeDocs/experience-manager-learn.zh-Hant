---
title: 在中使用SSL向AEM導
description: Adobe Experience Manager的SSL設定嚮導，使設定通過HTTPSAEM運行的實例更加容易。
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

# 在中使用SSL向AEM導

Adobe Experience Manager的SSL設定嚮導，使設定通過HTTPSAEM運行的實例更加容易。

>[!VIDEO](https://video.tv.adobe.com/v/17993?quality=12&learn=on)

開啟 __SSL配置嚮導__ 可以通過導航直接開啟 __AEM作者>工具>安全> SSL配置__。

>[!NOTE]
>
>對於受管環境，IT部門最好提供CA信任的證書和密鑰。
>
>自簽名證書僅用於開發目的。

## 私鑰和自簽名證書下載

以下zip包含 [!DNL DER] 和 [!DNL CRT] 在localhost上設定AEMSSL所需的檔案，僅用於本地開發。

的 [!DNL DER] 和 [!DNL CERT] 檔案是為方便起見而提供的，使用下面「生成私鑰」和「自簽名證書」部分中介紹的步驟生成。

如果需要，證書密碼短語為 **管理員**。

localhost — 私鑰和自簽名證書.zip（2028年7月到期）

[下載證書檔案](assets/use-the-ssl-wizard/certificate.zip)

## 私鑰和自簽名證書生成

上述視頻描述了使用自簽名證書的作者實AEM例上SSL的設定和配置。 以下命令使用 [[!DNL OpenSSL]](https://www.openssl.org/) 可以生成要在嚮導步驟2中使用的私鑰和證書。

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
