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
source-git-commit: c85a59a8bd180d5affe2a5bf5939dabfb2776d73
workflow-type: tm+mt
source-wordcount: '728'
ht-degree: 0%

---


# 設定公開和私密金鑰以搭配Adobe I/O使用

AEM使用公開／私密金鑰配對，與Adobe I/O和其他網站服務進行安全通訊。 本簡短教學課程說明如何使用AEM和Adobe I/O [!DNL openssl] 的命令列工具產生相容的金鑰和鑰匙庫。

>[!CAUTION]
>
>本指南會建立自簽名的索引鍵，以利於開發及在較低環境中使用。 在生產場景中，密鑰通常由組織的IT安全團隊生成和管理。

## 生成公共／私有密鑰對 {#generate-the-public-private-key-pair}

[[!DNL openssl]](https://www.openssl.org/docs/man1.0.2/man1/openssl.html) 命令列工具的命 [[!DNL req] ](https://www.openssl.org/docs/man1.0.2/man1/req.html) 令可用於生成與Adobe I/O和Adobe Experience Manager相容的密鑰對。

```shell
$ openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout private.key -out certificate.crt
```

要完成命 [!DNL openssl generate] 令，請在請求時提供證書資訊。 Adobe I/O和AEM不關心這些值是什麼，不過這些值應與之對齊，並說明您的金鑰。

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

可將密鑰對添加到新密鑰庫 [!DNL PKCS12] 中。 作為命 [[!DNL openssl]'s [!DNL pcks12] 令的一部分](https://www.openssl.org/docs/man1.0.2/man1/pkcs12.html) ，定義密鑰庫的名稱（通過）、密鑰的名稱(通過 `-  caname`)和密鑰庫的口令（通過） `-name``-  passout`。

這些值是將keystore和金鑰載入AEM時的必要項。

```shell
$ openssl pkcs12 -export -caname my-keystore -in certificate.crt -name my-key -inkey private.key -out keystore.p12 -passout pass:my-password
```

此命令的輸出是文 `keystore.p12` 件。

>[!NOTE]
>
>和的參 **[!DNL my-keystore]**&#x200B;數值 **[!DNL my-key]** 將 **[!DNL my-password]** 由您自己的值替換。

## 驗證密鑰庫內容 {#verify-the-keystore-contents}

Java命 [[!DNL keytool] 令行工具](https://docs.oracle.com/middleware/1213/wls/SECMG/keytool-summary-appx.htm#SECMG818) ，可以查看密鑰庫，以確保密鑰已成功載入到密鑰庫檔案([!DNL keystore.p12])中。

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

## 將金鑰庫新增至AEM {#adding-the-keystore-to-aem}

AEM使用產生的 **私密金鑰** ，與Adobe I/O和其他網站服務安全地通訊。 若要讓AEM存取私密金鑰，它必須安裝在AEM使用者的金鑰庫中。

導覽至「 **AEM >工[!UICONTROL 具]>安全性[!UICONTROL >使用者]> Users****** 」並編輯與之關聯的使用者私密金鑰。

### 建立AEM金鑰庫 {#create-an-aem-keystore}

![在AEM](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--create-keystore.png)*AEM >工具[!UICONTROL >安全性]>[!UICONTROL Edit使用者中建立KeyStore]*

如果提示建立密鑰庫，請執行此操作。 此金鑰庫僅存在於AEM中，而非透過openssl建立的金鑰庫。 口令可以是任何內容，不必與命令中使用的口令相 [!DNL openssl] 同。

### 透過密鑰庫安裝私密金鑰 {#install-the-private-key-via-the-keystore}

![在「AEM使用者」>「密鑰庫」](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--add-private-key.png)*[!UICONTROL >「從密鑰庫新增私密金鑰]」(Add[!UICONTROL Private Key from Keystore)中新增私密金鑰]*

在使用者的金鑰庫主控台中，按一下「從KeyStore **[!UICONTROL 新增私密金鑰」檔案]** ，並新增下列資訊：

* **[!UICONTROL 新別名]**:AEM中的金鑰別名。 這可以是任何內容，不必與使用openssl命令建立的密鑰庫的名稱相對應。
* **[!UICONTROL 密鑰庫檔案]**:openssl pkcs12命令(keystore.p12)的輸出
* **[!UICONTROL 私密金鑰別名]**:openssl pkcs12命令中通過參數設定的口 `-  passout` 令。

* **[!UICONTROL 私密金鑰密碼]**:openssl pkcs12命令中通過參數設定的口 `-  passout` 令。

### 驗證私密金鑰是否已載入AEM金鑰庫 {#verify-the-private-key-is-loaded-into-the-aem-keystore}

![驗證AEM使用者>密鑰](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--keystore.png)*[!UICONTROL 庫中的]「私密金[!UICONTROL 鑰」]*

從提供的金鑰庫成功載入私密金鑰至AEM金鑰庫時，私密金鑰的中繼資料會顯示在使用者的金鑰庫主控台中。

## 將公開金鑰新增至Adobe I/O {#adding-the-public-key-to-adobe-i-o}

相符的公開金鑰必須上傳至Adobe I/O，才能讓AEM服務使用者（其擁有公開金鑰的相應私人金鑰）安全地通訊。

### 建立Adobe I/O新整合 {#create-a-adobe-i-o-new-integration}

![建立Adobe I/O新整合](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--create-new-integration.png)

*[[!UICONTROL 建立Adobe I/O整合]](https://console.adobe.io/)>新[!UICONTROL 整合]*

在Adobe I/O中建立新整合需要上傳公用憑證。 上傳 **由命令產生的certificate.crt**`openssl req` 。

### 驗證Adobe I/O中是否已載入公開金鑰 {#verify-the-public-keys-are-loaded-in-adobe-i-o}

![驗證Adobe I/O中的公開金鑰](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--public-keys.png)

已安裝的公用金鑰及其到期日會列在 [!UICONTROL Adobe] I/O的Integrations Console中。可以通過「添加公共密鑰」 **[!UICONTROL 按鈕添加多個公共密鑰]** 。

現在，AEM持有私密金鑰，而Adobe I/O整合則持有對應的公開金鑰，讓AEM能夠安全地與Adobe I/O通訊。
