---
seo: Set up public and private keys for use with AEM and Adobe I/O
description: 'AEM使用公開／私密金鑰配對，與Adobe I/O和其他網站服務進行安全通訊。 本簡短教學課程說明如何使用可搭配AEM和Adobe I/O運作的openssl命令列工具來產生相容的金鑰和鑰匙圈。 '
version: 6.4, 6.5
feature: authentication
topics: authentication, integrations
activity: setup
audience: architect, developer, implementer
doc-type: tutorial
kt: 2450
translation-type: tm+mt
source-git-commit: 3f973e36531a2d04cbaf6bb8dd70b39fef7d8b2f
workflow-type: tm+mt
source-wordcount: '768'
ht-degree: 0%

---


# 設定公開和私密金鑰以搭配Adobe I/O使用

AEM使用公開／私密金鑰配對，與Adobe I/O和其他網站服務進行安全通訊。 本簡短教學課程說明如何使用[!DNL openssl]命令列工具產生相容的金鑰和鑰匙圈，此工具可搭配AEM和Adobe I/O運作。

>[!CAUTION]
>
>本指南會建立自簽名的索引鍵，以利於開發及在較低環境中使用。 在生產場景中，密鑰通常由組織的IT安全團隊生成和管理。

## 生成公用／私用密鑰對{#generate-the-public-private-key-pair}

[[!DNL openssl]](https://www.openssl.org/docs/man1.0.2/man1/openssl.html)命令列工具的[[!DNL req] 命令](https://www.openssl.org/docs/man1.0.2/man1/req.html)可用來產生與Adobe I/O和Adobe Experience Manager相容的金鑰對。

```shell
$ openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout private.key -out certificate.crt
```

要完成[!DNL openssl generate]命令，請在請求時提供證書資訊。 Adobe I/O和AEM不關心這些值是什麼，不過這些值應與之對齊，並說明您的金鑰。

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

## 將密鑰對添加到新密鑰庫{#add-key-pair-to-a-new-keystore}

可將密鑰對添加到新的[!DNL PKCS12]密鑰庫。 作為[[!DNL openssl]'s [!DNL pcks12] 命令的一部分，](https://www.openssl.org/docs/man1.0.2/man1/pkcs12.html)密鑰庫的名稱（通過`-  caname`）、密鑰的名稱（通過`-name`）和密鑰庫的口令（通過`-  passout`）被定義。

這些值是將keystore和金鑰載入AEM時的必要項。

```shell
$ openssl pkcs12 -export -caname my-keystore -in certificate.crt -name my-key -inkey private.key -out keystore.p12 -passout pass:my-password
```

此命令的輸出是`keystore.p12`檔案。

>[!NOTE]
>
>**[!DNL my-keystore]**、**[!DNL my-key]**&#x200B;和&#x200B;**[!DNL my-password]**&#x200B;的參數值將由您自己的值替換。

## 驗證密鑰庫內容{#verify-the-keystore-contents}

Java [[!DNL keytool] 命令行工具](https://docs.oracle.com/middleware/1213/wls/SECMG/keytool-summary-appx.htm#SECMG818)可以查看密鑰庫，以確保密鑰已成功載入到密鑰庫檔案([!DNL keystore.p12])中。

```shell
$ keytool -keystore keystore.p12 -list

Enter keystore password: my-password

Keystore type: jks
Keystore provider: SUN

Your keystore contains 1 entry

my-key, Feb 5, 2019, PrivateKeyEntry,
Certificate fingerprint (SHA1): 7C:6C:25:BD:52:D3:3B:29:83:FD:A2:93:A8:53:91:6A:25:1F:2D:52
```

![驗證Adobe I/O中的金鑰存放區](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--public-keys.png)

## 將密鑰庫添加到AEM {#adding-the-keystore-to-aem}

AEM使用產生的&#x200B;**私密金鑰**&#x200B;與Adobe I/O和其他網站服務進行安全通訊。 若要讓AEM存取私密金鑰，它必須安裝在AEM使用者的金鑰庫中。

導覽至&#x200B;**AEM > [!UICONTROL Tools] > [!UICONTROL Security] > [!UICONTROL Users]**&#x200B;和&#x200B;**編輯用戶**，私密金鑰將與相關聯。

### 建立AEM金鑰庫{#create-an-aem-keystore}

![在](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--create-keystore.png)
*AEMAEM >工具>  [!UICONTROL Security]  [!UICONTROL > ] Users  >編輯使用者中建立KeyStore*

如果提示建立密鑰庫，請執行此操作。 此金鑰庫僅存在於AEM中，而非透過openssl建立的金鑰庫。 口令可以是任何內容，不必與[!DNL openssl]命令中使用的口令相同。

### 通過密鑰庫{#install-the-private-key-via-the-keystore}安裝私鑰

![在](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--add-private-key.png)
*[!UICONTROL AEMUser] >  [!UICONTROL Keystore] >從密鑰庫添 [!UICONTROL 加私鑰]*

在用戶的密鑰庫控制台中，按一下「從KeyStore檔案&#x200B;**[!UICONTROL 添加私密密鑰」並添加以下資訊：]**

* **[!UICONTROL 新別名]**:AEM中的金鑰別名。這可以是任何內容，不必與使用openssl命令建立的密鑰庫的名稱相對應。
* **[!UICONTROL KeyStore檔案]**:openssl pkcs12命令(keystore.p12)的輸出
* **[!UICONTROL KeyStore檔案密碼]**:在openssl pkcs12命令中通過參數設定的 `-passout` 口令。
* **[!UICONTROL 私密金鑰別名]**:上述openssl pkcs12命 `-name` 令中為引數提供的值(即 `my-key`)。
* **[!UICONTROL 私密金鑰密碼]**:在openssl pkcs12命令中通過參數設定的 `-passout` 口令。

>[!CAUTION]
>
>KeyStore檔案密碼和私密金鑰密碼對於這兩項輸入都相同。 輸入不相符的密碼將導致無法匯入金鑰。

### 驗證私密金鑰是否已載入AEM金鑰庫{#verify-the-private-key-is-loaded-into-the-aem-keystore}

![驗證](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--keystore.png)
*[!UICONTROL AEMUser] >密鑰 [!UICONTROL 庫]*

從提供的金鑰庫成功載入私密金鑰至AEM金鑰庫時，私密金鑰的中繼資料會顯示在使用者的金鑰庫主控台中。

## 將公開金鑰新增至Adobe I/O {#adding-the-public-key-to-adobe-i-o}

相符的公開金鑰必須上傳至Adobe I/O，才能讓AEM服務使用者（其擁有公開金鑰的相應私人金鑰）安全地通訊。

### 建立Adobe I/O新整合{#create-a-adobe-i-o-new-integration}

![建立Adobe I/O新整合](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--create-new-integration.png)

*[[!UICONTROL 建立Adobe I/O整合]](https://console.adobe.io/) >新 [!UICONTROL 整合]*

在Adobe I/O中建立新整合需要上傳公用憑證。 上傳由`openssl req`命令產生的&#x200B;**certificate.crt**。

### 驗證Adobe I/O {#verify-the-public-keys-are-loaded-in-adobe-i-o}中是否載入了公鑰

![驗證Adobe I/O中的公開金鑰](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--public-keys.png)

已安裝的公用金鑰及其到期日會列在Adobe I/O的[!UICONTROL Integrations]主控台中。可以通過&#x200B;**[!UICONTROL 添加公鑰]**&#x200B;按鈕添加多個公鑰。

現在，AEM持有私密金鑰，而Adobe I/O整合則持有對應的公開金鑰，讓AEM能夠安全地與Adobe I/O通訊。
