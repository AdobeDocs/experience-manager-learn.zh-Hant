---
title: 使用AEM設定OKTA
description: 瞭解使用OKTA使用單一登入的各種組態設定。
version: Experience Manager 6.5
topic: Integrations, Security, Administration
feature: Integrations
role: Admin
level: Experienced
jira: KT-12305
last-substantial-update: 2023-03-01T00:00:00Z
doc-type: Tutorial
exl-id: 460e9bfa-1b15-41b9-b8b7-58b2b1252576
duration: 157
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '753'
ht-degree: 0%

---

# 使用OKTA驗證AEM作者

> 如需如何使用AEM as a Cloud Service設定OKTA的說明，請參閱[SAML 2.0驗證](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/authentication/saml-2-0.html?lang=zh-Hant)。

第一步是在OKTA入口網站上設定您的應用程式。 您的應用程式獲得OKTA管理員核准後，您就能存取IdP憑證和單一登入URL。 以下是註冊新應用程式時通常會使用的設定。

* **應用程式名稱：**&#x200B;這是您的應用程式名稱。 請務必為應用程式指定唯一的名稱。
* **SAML收件者：**&#x200B;從OKTA驗證之後，這個URL將會在您的AEM執行個體上以SAML回應點選。 SAML驗證處理常式通常會截獲所有含有/ saml_login的URL，但最好將它附加至應用程式根之後。
* **SAML對象**：這是您應用程式的網域URL。 請勿在網域URL中使用通訊協定（http或https）。
* **SAML名稱ID：**&#x200B;從下拉式清單中選取電子郵件。
* **環境**：選擇適當的環境。
* **屬性**：這些是您在SAML回應中取得有關使用者的屬性。 視需要指定。


![okta-application](assets/okta-app-settings-blurred.PNG)


## 將OKTA (IdP)憑證新增至AEM信任存放區

由於SAML宣告已加密，因此我們需要將IdP (OKTA)憑證新增到AEM信任存放區，以允許OKTA和AEM之間的安全通訊。
[初始化信任存放區](http://localhost:4502/libs/granite/security/content/truststore.html) （如果尚未初始化）。
記住信任存放區密碼。 我們稍後將需要在此程式中使用這個密碼。

* 瀏覽至[全域信任存放區](http://localhost:4502/libs/granite/security/content/truststore.html)。
* 按一下「從CER檔案新增憑證」。 新增OKTA提供的IdP憑證，然後按一下「提交」。

  >[!NOTE]
  >
  >請勿將憑證對應到任何使用者

將憑證新增到信任存放區時，您應該取得憑證別名，如下方熒幕擷圖所示。 您的案例中，別名可能不同。

![憑證別名](assets/cert-alias.PNG)

**記下憑證別名。 您需要在後續步驟中執行此動作。**

### 設定SAML驗證處理常式

瀏覽至[configMgr](http://localhost:4502/system/console/configMgr)。
搜尋並開啟「Adobe Granite SAML 2.0驗證處理常式」。
提供下列指定的屬性
以下是需要指定的主要屬性：

* **path** — 這是觸發驗證處理常式的路徑
* **IdP Url**：這是您的IdP URL，由OKTA提供
* **IDP憑證別名**：這是您將IdP憑證新增至AEM信任存放區時取得的別名
* **服務提供者實體識別碼**：這是您的AEM伺服器的名稱
* **金鑰庫的密碼**：這是您使用的信任庫密碼
* **預設重新導向**：這是成功驗證時重新導向到的URL
* **UserID屬性**：uid
* **使用加密**：false
* **自動建立CRX使用者**：true
* **新增至群組**：true
* **預設群組**：oktausers(這是新增使用者的群組。 您可以在AEM中提供任何現有群組)
* **NamedIDPolicy**：指定要用來代表要求之主體的名稱識別碼上的限制。 複製並貼上下列醒目提示的字串&#x200B;**urn:oasis:name:tc:SAML：2.0:nameidformat:emailAddress**
* **已同步屬性** — 這些是從AEM設定檔中的SAML判斷提示儲存的屬性

![saml-authentication-handler](assets/saml-authentication-settings-blurred.PNG)

### 設定Apache Sling查閱者篩選器

瀏覽至[configMgr](http://localhost:4502/system/console/configMgr)。
搜尋並開啟「Apache Sling反向連結篩選器」。設定下列屬性，如下所示：

* **允許空白**： false
* **允許主機**： IdP的主機名稱（您的情況不同）
* **允許Regexp主機**： IdP的主機名稱（您的情況不同）
Sling查閱者篩選查閱者屬性熒幕擷圖

![反向連結篩選器](assets/okta-referrer.png)

#### 設定OKTA整合的DEBUG記錄

在AEM上設定OKTA整合時，檢閱AEM的SAML驗證處理常式的DEBUG記錄會很有幫助。 若要將記錄層級設定為DEBUG，請透過AEM OSGi Web Console建立新的Sling記錄器設定。

請記得在「中繼與生產」上移除或停用此記錄器，以減少記錄雜訊。

在AEM上設定OKTA整合時，檢閱AEM的SAML驗證處理常式的DEBUG記錄會很有幫助。 若要將記錄層級設定為DEBUG，請透過AEM OSGi Web Console建立新的Sling記錄器設定。
**請記得移除或停用[中繼與生產]上的這個記錄器，以減少記錄雜訊。**
* 瀏覽至[configMgr](http://localhost:4502/system/console/configMgr)

* 搜尋並開啟「Apache Sling記錄記錄器設定」
* 使用下列設定來建立記錄器：
   * **記錄層級**：偵錯
   * **記錄檔**： logs/saml.log
   * **記錄器**： com.adobe.granite.auth.saml
* 按一下儲存以儲存您的設定

#### 測試您的OKTA設定

登出AEM執行個體。 請嘗試存取連結。 您應該會看到OKTA SSO的實際運作。
