---
seo: Set up public and private keys for use with AEM and Adobe I/O
description: '使AEM用公開／私密金鑰配對，與Adobe I/O和其他web services安全通訊。 本簡短教學課程說明如何使用openssl命令列工具產生相容的金鑰和AEM鑰匙庫。 '
version: 6.4, 6.5
feature: 身份驗證
topics: authentication, integrations
activity: setup
audience: architect, developer, implementer
doc-type: tutorial
kt: 2450
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '769'
ht-degree: 0%

---


# 設定公用和私用金鑰以搭配Adobe I/O

使AEM用公開／私密金鑰配對，與Adobe I/O和其他web services安全通訊。 本簡短的教學課程說明如何使用[!DNL openssl]命令列工具產生相容的金鑰和鑰匙存放區，此工具可搭配使用AEM和Adobe I/O。

>[!CAUTION]
>
>本指南會建立自簽名的索引鍵，以利於開發及在較低環境中使用。 在生產場景中，密鑰通常由組織的IT安全團隊生成和管理。

## 生成公用／私用密鑰對{#generate-the-public-private-key-pair}

[[!DNL openssl]](https://www.openssl.org/docs/man1.0.2/man1/openssl.html)命令行工具的[[!DNL req] 命令](https://www.openssl.org/docs/man1.0.2/man1/req.html)可用於生成與Adobe I/O和Adobe Experience Manager相容的密鑰對。

```shell
$ openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout private.key -out certificate.crt
```

要完成[!DNL openssl generate]命令，請在請求時提供證書資訊。 Adobe I/O,AEM不管這些值是什麼，但它們應與哪些值對齊，並描述您的金鑰。

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

將密鑰庫和密鑰載入到中時需要這些值AEM。

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

![驗證Adobe I/O中的密鑰儲存](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--public-keys.png)

## 將密鑰庫添AEM加到{#adding-the-keystore-to-aem}

使AEM用產生的&#x200B;**私密金鑰**&#x200B;與Adobe I/O和其他web services安全通訊。 要使私有密鑰可以訪問，AEM必須將其安裝到用戶AEM的密鑰庫中。

導覽至&#x200B;**AEM > [!UICONTROL Tools] > [!UICONTROL Security] > [!UICONTROL Users]**&#x200B;和&#x200B;**編輯用戶**，私密金鑰將與相關聯。

### 建立AEM密鑰庫{#create-an-aem-keystore}

![在](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--create-keystore.png)
*AEMAEM >工具>  [!UICONTROL Security]  [!UICONTROL > ] Users  >編輯使用者中建立KeyStore*

如果提示建立密鑰庫，請執行此操作。 此密鑰庫僅存在於AEM中，不是通過openssl建立的密鑰庫。 口令可以是任何內容，不必與[!DNL openssl]命令中使用的口令相同。

### 通過密鑰庫{#install-the-private-key-via-the-keystore}安裝私鑰

![在](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--add-private-key.png)
*[!UICONTROL AEMUser] >  [!UICONTROL Keystore] >從密鑰庫添 [!UICONTROL 加私鑰]*

在用戶的密鑰庫控制台中，按一下「從KeyStore檔案&#x200B;]**添加私密密鑰」並添加以下資訊：**[!UICONTROL 

* **[!UICONTROL 新別名]**:鑰匙的別名AEM。這可以是任何內容，不必與使用openssl命令建立的密鑰庫的名稱相對應。
* **[!UICONTROL KeyStore檔案]**:openssl pkcs12命令(keystore.p12)的輸出
* **[!UICONTROL KeyStore檔案密碼]**:在openssl pkcs12命令中通過參數設定的 `-passout` 口令。
* **[!UICONTROL 私密金鑰別名]**:上述openssl pkcs12命 `-name` 令中為引數提供的值(即 `my-key`)。
* **[!UICONTROL 私密金鑰密碼]**:在openssl pkcs12命令中通過參數設定的 `-passout` 口令。

>[!CAUTION]
>
>KeyStore檔案密碼和私密金鑰密碼對於這兩項輸入都相同。 輸入不相符的密碼將導致無法匯入金鑰。

### 驗證私鑰是否已載入到密鑰AEM庫{#verify-the-private-key-is-loaded-into-the-aem-keystore}中

![驗證](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--keystore.png)
*[!UICONTROL AEMUser] >密鑰 [!UICONTROL 庫]*

當私鑰從提供的密鑰庫成功載入到密鑰庫AEM中時，私鑰的元資料將顯示在用戶的密鑰庫控制台中。

## 將公共密鑰添加到Adobe I/O{#adding-the-public-key-to-adobe-i-o}

必須將匹配的公鑰上傳到Adobe I/O，以便服AEM務用戶能夠安全地通信，該服務用戶具有公鑰的相應私鑰。

### 建立Adobe I/O新整合{#create-a-adobe-i-o-new-integration}

![建立Adobe I/O新整合](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--create-new-integration.png)

*[[!UICONTROL 建立Adobe I/O整合]](https://console.adobe.io/) >新 [!UICONTROL 的整合]*

在Adobe I/O中建立新整合需要上傳公用憑證。 上傳由`openssl req`命令產生的&#x200B;**certificate.crt**。

### 驗證公鑰是否已載入到Adobe I/O{#verify-the-public-keys-are-loaded-in-adobe-i-o}

![驗證Adobe I/O中的公鑰](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--public-keys.png)

已安裝的公鑰及其過期日期列在Adobe I/O的[!UICONTROL Integrations]控制台中。可以通過&#x200B;**[!UICONTROL 添加公鑰]**&#x200B;按鈕添加多個公鑰。

現在AEM握有私密金鑰，Adobe I/O整合握有對應的公開金鑰，讓您可AEM與Adobe I/O安全通訊。
