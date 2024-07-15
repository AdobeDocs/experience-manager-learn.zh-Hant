---
title: 在AEM中使用SSL精靈
description: Adobe Experience Manager的SSL設定精靈，讓您更輕鬆地設定AEM執行個體以透過HTTPS執行。
version: 6.5, Cloud Service
jira: KT-13839
doc-type: Technical Video
topic: Security
role: Developer
level: Beginner
exl-id: 4e69e115-12a6-4a57-90da-b91e345c6723
last-substantial-update: 2023-08-08T00:00:00Z
duration: 564
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '448'
ht-degree: 0%

---

# 在AEM中使用SSL精靈

瞭解如何在Adobe Experience Manager中設定SSL，使用內建SSL精靈讓SSL透過HTTPS執行。

>[!VIDEO](https://video.tv.adobe.com/v/17993?quality=12&learn=on)


>[!NOTE]
>
>對於受管理的環境，最好由IT部門提供CA信任的憑證和金鑰。
>
>自我簽署憑證僅用於開發目的。

## 使用SSL設定精靈

瀏覽至&#x200B;__AEM Author > Tools > Security > SSL Configuration__，然後開啟&#x200B;__SSL Configuration Wizard__。

![SSL設定精靈](assets/use-the-ssl-wizard/ssl-config-wizard.png)

### 建立存放區認證

若要建立與`ssl-service`系統使用者及全域&#x200B;_信任存放區_&#x200B;相關聯的&#x200B;_金鑰存放區_，請使用&#x200B;__存放區認證__&#x200B;精靈步驟。

1. 輸入與`ssl-service`系統使用者相關聯之&#x200B;__金鑰存放區__&#x200B;的密碼及確認密碼。
1. 輸入全域&#x200B;__信任存放區__&#x200B;的密碼及確認密碼。 請注意，這是系統範圍的信任存放區，如果已建立，則會忽略輸入的密碼。

   ![SSL設定 — 儲存認證](assets/use-the-ssl-wizard/store-credentials.png)

### 上傳私密金鑰和憑證

若要上傳&#x200B;_私密金鑰_&#x200B;和&#x200B;_SSL憑證_，請使用&#x200B;__金鑰和憑證__&#x200B;精靈步驟。

一般而言，您的IT部門會提供CA信任的憑證和金鑰，但自我簽署憑證可用於&#x200B;__開發__&#x200B;和&#x200B;__測試__&#x200B;用途。

若要建立或下載自我簽署憑證，請參閱[自我簽署私密金鑰與憑證](#self-signed-private-key-and-certificate)。

1. 以DER （辨別編碼規則）格式上傳&#x200B;__私密金鑰__。 不同於PEM，DER編碼的檔案不包含純文字陳述式，例如`-----BEGIN CERTIFICATE-----`
1. 以`.crt`格式上傳相關聯的&#x200B;__SSL憑證__。

   ![SSL設定 — 私密金鑰和憑證](assets/use-the-ssl-wizard/privatekey-and-certificate.png)

### 更新SSL聯結器詳細資料

若要更新&#x200B;_主機名稱_&#x200B;和&#x200B;_連線埠_，請使用&#x200B;__SSL聯結器__&#x200B;精靈步驟。

1. 更新或驗證&#x200B;__HTTPS主機名稱__&#x200B;值，它應該與憑證中的`Common Name (CN)`相符。
1. 更新或驗證&#x200B;__HTTPS連線埠__&#x200B;值。

   ![SSL設定 — SSL聯結器詳細資料](assets/use-the-ssl-wizard/ssl-connector-details.png)

### 驗證SSL設定

1. 若要驗證SSL，請按一下&#x200B;__移至HTTPS URL__&#x200B;按鈕。
1. 如果使用自我簽署憑證，您會看到`Your connection is not private`錯誤。

   ![SSL設定 — 透過HTTPS驗證AEM](assets/use-the-ssl-wizard/verify-aem-over-ssl.png)

## 自我簽署私密金鑰和憑證

下列zip包含在本機設定AEM SSL所需的[!DNL DER]和[!DNL CRT]檔案，且僅供本機開發使用。

提供[!DNL DER]和[!DNL CERT]檔案是為了方便起見，並使用下列[產生私密金鑰和自簽憑證]一節中概述的步驟產生。

如有需要，憑證密語為&#x200B;**admin**。

此localhost — 私密金鑰和自簽的certificate.zip （2028年7月到期）

[下載憑證檔案](assets/use-the-ssl-wizard/certificate.zip)

### 私密金鑰和自簽憑證產生

上述影片說明在AEM製作執行個體使用自我簽署憑證時，SSL的設定和設定。 使用[[!DNL OpenSSL]](https://www.openssl.org/)的下列命令可以產生私密金鑰和憑證，以用於精靈的步驟2。

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
