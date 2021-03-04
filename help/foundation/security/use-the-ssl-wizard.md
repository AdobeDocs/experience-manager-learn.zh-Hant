---
title: 使用SSL精靈AEM,
description: Adobe Experience Manager的SSL設定精靈，讓您更輕鬆地設AEM定透過HTTPS執行的例項。
seo-description: Adobe Experience Manager的SSL設定精靈，讓您更輕鬆地設AEM定透過HTTPS執行的例項。
version: 6.3, 6,4, 6.5
topics: security, operations
activity: use
audience: administrator
doc-type: technical video
uuid: 82a6962e-3658-427a-bfad-f5d35524f93b
discoiquuid: 9e666741-0f76-43c9-ab79-1ef149884686
topic: 安全性
role: 開發人員
level: 初學者
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '216'
ht-degree: 0%

---


# 使用SSL精靈AEM,

Adobe Experience Manager的SSL設定精靈，讓您更輕鬆地設AEM定透過HTTPS執行的例項。

>[!VIDEO](https://video.tv.adobe.com/v/17993/?quality=12&learn=on)

>[!NOTE]
>
>對於受管理環境，IT部門最好提供CA信任證書和密鑰。
>
>自簽證書僅用於開發用途。

## 私密金鑰和自簽證書下載

以下zip包含在localhost上設定AEMSSL所需的[!DNL DER]和[!DNL CRT]檔案，僅供本機開發之用。

提供[!DNL DER]和[!DNL CERT]檔案是為了方便起見，並依照下面「產生私密金鑰和自簽證書」一節所述的步驟產生。

如果需要，憑證密碼片語為&#x200B;**admin**。

localhost —— 私密金鑰和自簽證書。zip（於2028年7月到期）

[下載憑證檔案](assets/use-the-ssl-wizard/certificate.zip)

## 私密金鑰和自簽名憑證產生

上述視訊描述使用自簽證書在作AEM者例項上設定和設定SSL。 使用[[!DNL OpenSSL]](https://www.openssl.org/)的以下命令可生成用於嚮導步驟2的私鑰和證書。

```shell
### Create Private Key
$ openssl genrsa -aes256 -out localhostprivate.key 4096

### Generate Certificate Signing Request using private key
$ openssl req -sha256 -new -key localhostprivate.key -out localhost.csr -subj '/CN=localhost'

### Generate the SSL certificate and sign with the private key, will expire one year from now
$ openssl x509 -req -days 365 -in localhost.csr -signkey localhostprivate.key -out localhost.crt

### Convert Private Key to DER format - SSL wizard requires key to be in DER format
$ openssl pkcs8 -topk8 -inform PEM -outform DER -in localhostprivate.key -out localhostprivate.der -nocrypt
```
