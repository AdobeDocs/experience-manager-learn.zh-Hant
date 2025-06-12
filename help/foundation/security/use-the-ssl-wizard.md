---
title: 在 AEM 中使用 SSL 精靈
description: Adobe Experience Manager 的 SSL 設定精靈，讓您更輕鬆地設定 AEM 實例透過 HTTPS 執行。
version: Experience Manager 6.5, Experience Manager as a Cloud Service
jira: KT-13839
doc-type: Technical Video
topic: Security
role: Developer
level: Beginner
exl-id: 4e69e115-12a6-4a57-90da-b91e345c6723
last-substantial-update: 2023-08-08T00:00:00Z
duration: 564
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '448'
ht-degree: 100%

---

# 在 AEM 中使用 SSL 精靈

了解如何使用內建的 SSL 精靈，在 Adobe Experience Manager 中設定 SSL，使其透過 HTTPS 執行。

>[!VIDEO](https://video.tv.adobe.com/v/17993?quality=12&learn=on)


>[!NOTE]
>
>對於受管理的環境，IT 部門最好提供 CA 信任的憑證和金鑰。
>
>自我簽署的憑證僅能供開發使用。

## 使用 SSL 設定精靈

導覽至&#x200B;__「AEM Author」>「工具」>「安全性」>「SSL 設定」__，然後開啟「__SSL 設定精靈__」。

![SSL 設定精靈](assets/use-the-ssl-wizard/ssl-config-wizard.png)

### 建立儲存認證

若要建立與 `ssl-service` 系統使用者關聯的 _Key Store_ 以及全域 _Trust Store_，請使用「__儲存認證__」精靈步驟。

1. 輸入與 `ssl-service` 系統使用者關聯之 __Key Store__ 的密碼，並確認密碼。
1. 輸入全域 __Trust Store__ 的密碼，並確認密碼。請注意，這是一個全系統適用的 Trust Store，若先前便已建立，則會忽略所輸入的密碼。

   ![SSL 設定 - 儲存認證](assets/use-the-ssl-wizard/store-credentials.png)

### 上傳私密金鑰和憑證

若要上傳&#x200B;_私密金鑰_&#x200B;和 _SSL 憑證_，請使用「__金鑰和憑證__」精靈步驟。

通常，您的 IT 部門會提供 CA 信任的憑證和金鑰，但自我簽署憑證可供&#x200B;__開發__&#x200B;和&#x200B;__測試__&#x200B;使用。

若要建立或下載自我簽署憑證，請參閱[自我簽署的私密金鑰和憑證](#self-signed-private-key-and-certificate)。

1. 以 DER (可分辨編碼規則) 格式上傳&#x200B;__私密金鑰__。與 PEM 不同，DER 編碼的檔案不包含純文字語句，例如 `-----BEGIN CERTIFICATE-----`
1. 以 `.crt` 格式上傳相關聯的 __SSL 憑證__。

   ![SSL 設定 - 私密金鑰和憑證](assets/use-the-ssl-wizard/privatekey-and-certificate.png)

### 更新 SSL 連接器詳細資訊

若要更新&#x200B;_主機名稱_&#x200B;和&#x200B;_連接埠_，請使用「__SSL 連接器__」精靈步驟。

1. 更新或驗證「__HTTPS 主機名稱__」值，其應與憑證的 `Common Name (CN)` 相符。
1. 更新或驗證「__HTTPS 連接埠__」值。

   ![SSL 設定 - SSL 連接器詳細資訊](assets/use-the-ssl-wizard/ssl-connector-details.png)

### 驗證 SSL 設定

1. 若要驗證 SSL，請按一下「__前往 HTTPS URL__」按鈕。
1. 如果使用自我簽署憑證，您會看到 `Your connection is not private` 錯誤。

   ![SSL 設定 - 透過 HTTPS 驗證 AEM](assets/use-the-ssl-wizard/verify-aem-over-ssl.png)

## 自我簽署的私密金鑰和憑證

以下 zip 檔案包含在本機設定 AEM SSL 所需的 [!DNL DER] 和 [!DNL CRT] 檔案，僅供本機開發使用。

為方便起見提供 [!DNL DER] 和 [!DNL CERT] 檔案，這是使用下方「產生私密金鑰和自我簽署憑證」區段中所列出的步驟所產生的。

如有需要，憑證密碼為 **admin**。

localhost - private key and self-signed certificate.zip (2028 年 7 月到期)

[下載憑證檔案](assets/use-the-ssl-wizard/certificate.zip)

### 產生私密金鑰和自我簽署憑證

上述影片內容描述使用自我簽署的憑證在 AEM 作者實例上建立和設定 SSL 的過程。下方使用 [[!DNL OpenSSL]](https://www.openssl.org/) 的命令，可以產生用於精靈步驟 2 的私密金鑰和憑證。

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
