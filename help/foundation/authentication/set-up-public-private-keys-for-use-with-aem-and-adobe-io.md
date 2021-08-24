---
seo: Set up public and private keys for use with AEM and Adobe I/O
description: 'AEM使用公開/私密金鑰組來與Adobe I/O和其他網站服務安全通訊。 本簡短教學課程說明如何使用openssl命令列工具(可同時與AEM和Adobe I/O搭配使用)產生相容的金鑰和金鑰存放區。 '
version: 6.4, 6.5
feature: 使用者和群組
topics: authentication, integrations
activity: setup
audience: architect, developer, implementer
doc-type: tutorial
kt: 2450
topic: 開發
role: Developer
level: Experienced
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '772'
ht-degree: 0%

---


# 設定公開金鑰和私密金鑰以搭配Adobe I/O

AEM使用公開/私密金鑰組來與Adobe I/O和其他網站服務安全通訊。 本簡短教學課程說明如何使用[!DNL openssl]命令列工具(可同時與AEM和Adobe I/O搭配使用)來產生相容的金鑰和金鑰存放區。

>[!CAUTION]
>
>本指南會建立自行簽署的索引鍵，以利開發及在較低環境中使用。 在生產案例中，金鑰通常由組織的IT安全團隊產生和管理。

## 產生公開/私密金鑰組 {#generate-the-public-private-key-pair}

[[!DNL openssl]](https://www.openssl.org/docs/man1.0.2/man1/openssl.html)命令列工具的[[!DNL req] command](https://www.openssl.org/docs/man1.0.2/man1/req.html)可用來產生與Adobe I/O和Adobe Experience Manager相容的金鑰組。

```shell
$ openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout private.key -out certificate.crt
```

要完成[!DNL openssl generate]命令，請在請求時提供證書資訊。 Adobe I/O和AEM不在乎這些值是什麼，但它們應與一致，並說明您的金鑰。

```
Generating a 2048 bit RSA private key
...........................................................+++
...+++
writing new private key to 'private.key'
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) []:US
State or Province Name (full name) []:CA
Locality Name (eg, city) []:San Jose
Organization Name (eg, company) []:Example Co
Organizational Unit Name (eg, section) []:Digital Marketing
Common Name (eg, fully qualified host name) []:com.example
Email Address []:me@example.com
```

## 將密鑰對添加到新密鑰庫 {#add-key-pair-to-a-new-keystore}

可將密鑰對添加到新的[!DNL PKCS12]密鑰庫。 在[[!DNL openssl]'s [!DNL pcks12] 命令中，](https://www.openssl.org/docs/man1.0.2/man1/pkcs12.html)密鑰庫的名稱（通過`-  caname`）、密鑰的名稱（通過`-name`）和密鑰庫的密碼（通過`-  passout`）被定義。

將金鑰存放區和金鑰載入AEM中時，需要這些值。

```shell
$ openssl pkcs12 -export -caname my-keystore -in certificate.crt -name my-key -inkey private.key -out keystore.p12 -passout pass:my-password
```

此命令的輸出為`keystore.p12`檔案。

>[!NOTE]
>
>**[!DNL my-keystore]**、**[!DNL my-key]**&#x200B;和&#x200B;**[!DNL my-password]**&#x200B;的參數值將替換為您自己的值。

## 驗證金鑰存放區內容 {#verify-the-keystore-contents}

Java [[!DNL keytool] 命令行工具](https://docs.oracle.com/middleware/1213/wls/SECMG/keytool-summary-appx.htm#SECMG818)提供密鑰庫的可見性，以確保密鑰庫檔案([!DNL keystore.p12])中的密鑰已成功載入。

```shell
$ keytool -keystore keystore.p12 -list

Enter keystore password: my-password

Keystore type: jks
Keystore provider: SUN

Your keystore contains 1 entry

my-key, Feb 5, 2019, PrivateKeyEntry,
Certificate fingerprint (SHA1): 7C:6C:25:BD:52:D3:3B:29:83:FD:A2:93:A8:53:91:6A:25:1F:2D:52
```

![驗證密鑰儲存在Adobe I/O中](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--public-keys.png)

## 將金鑰存放區新增至AEM {#adding-the-keystore-to-aem}

AEM使用產生的&#x200B;**私密金鑰**&#x200B;與Adobe I/O和其他網站服務安全通訊。 為了讓AEM能夠存取私密金鑰，必須將其安裝至AEM使用者的金鑰存放區。

導覽至&#x200B;**AEM > [!UICONTROL Tools] > [!UICONTROL Security] > [!UICONTROL Users]**&#x200B;和&#x200B;**編輯要關聯的用戶**&#x200B;私鑰。

### 建立AEM金鑰存放區 {#create-an-aem-keystore}

![在「AEM >工](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--create-keystore.png)
*具 [!UICONTROL  >安全] 性 [!UICONTROL >使用者]  >  [!UICONTROL 編輯使用者] 」中建立KeyStore*

如果系統提示建立金鑰存放區，請執行此操作。 此金鑰存放區僅存在於AEM中，且不是透過openssl建立的金鑰存放區。 密碼可以是任何值，不必與[!DNL openssl]命令中使用的密碼相同。

### 透過金鑰存放區安裝私密金鑰 {#install-the-private-key-via-the-keystore}

![在AEMUser](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--add-private-key.png)
*[!UICONTROL > ] 金鑰存放區 [!UICONTROL  >從金鑰存放區]  [!UICONTROL 新增私密金鑰]*

在用戶的密鑰庫控制台中，按一下「從KeyStore檔案&#x200B;]**添加私鑰」並添加以下資訊：**[!UICONTROL 

* **[!UICONTROL 新別名]**:鍵在AEM中的別名。這可以是任何值，不必與使用openssl命令建立的密鑰庫的名稱相對應。
* **[!UICONTROL KeyStore檔案]**:openssl pkcs12命令(keystore.p12)的輸出
* **[!UICONTROL KeyStore檔案密碼]**:在openssl pkcs12命令中通過參數設定的 `-passout` 密碼。
* **[!UICONTROL 私鑰別名]**:上述openssl pkcs12 `-name` 命令中的參數所提供的值(即 `my-key`)。
* **[!UICONTROL 私密金鑰密碼]**:在openssl pkcs12命令中通過參數設定的 `-passout` 密碼。

>[!CAUTION]
>
>對於這兩項輸入，KeyStore檔案密碼和私鑰密碼相同。 輸入不匹配的密碼將導致密鑰未導入。

### 驗證私密金鑰是否已載入AEM金鑰存放區中 {#verify-the-private-key-is-loaded-into-the-aem-keystore}

![驗證AEMUser](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--keystore.png)
*[!UICONTROL >金鑰] 存放區中的私 [!UICONTROL 密金鑰]*

從提供的金鑰存放區成功載入私密金鑰至AEM金鑰存放區時，私密金鑰的中繼資料會顯示在使用者的金鑰存放區主控台中。

## 新增公開金鑰至Adobe I/O {#adding-the-public-key-to-adobe-i-o}

必須將相符的公開金鑰上傳至Adobe I/O，以允許AEM服務使用者（擁有公開金鑰的對應私密金鑰）安全地通訊。

### 建立Adobe I/O新整合 {#create-a-adobe-i-o-new-integration}

![建立Adobe I/O新整合](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--create-new-integration.png)

*[[!UICONTROL 建立Adobe I/O整合]](https://console.adobe.io/) >新 [!UICONTROL 整合]*

在Adobe I/O中建立新整合需要上傳公開憑證。 上傳由`openssl req`命令產生的&#x200B;**certificate.crt**。

### 驗證公鑰是否已載入Adobe I/O {#verify-the-public-keys-are-loaded-in-adobe-i-o}

![驗證Adobe I/O中的公鑰](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--public-keys.png)

已安裝的公開金鑰及其到期日會列在Adobe I/O的[!UICONTROL Integrations]主控台中。可透過&#x200B;**[!UICONTROL 新增公開金鑰]**&#x200B;按鈕新增多個公開金鑰。

現在，AEM會保留私密金鑰，而Adobe I/O整合則保留對應的公開金鑰，讓AEM能夠安全地與Adobe I/O通訊。
