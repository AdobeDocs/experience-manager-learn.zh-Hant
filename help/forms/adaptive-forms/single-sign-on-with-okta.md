---
title: 將OKTA配置為AEM
description: 瞭解使用okta使用單一登入的各種組態設定
feature: 管理
topics: development, authentication, security
audience: developer
doc-type: tutorial
activity: setup
version: 6.5
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '763'
ht-degree: 0%

---


# 使用OKTA驗證AEM作者

第一步是在OKTA入口網站上設定您的應用程式。 一旦您的應用程式獲得OKTA管理員的核准，您就可以存取IdP憑證並在URL上單一登入。 以下是註冊新應用程式時通常使用的設定。

* **應用程式名稱：** 這是您的應用程式名稱。請確定您為應用程式指定了唯一的名稱。
* **SAML收件者：** 從OKTA進行驗證後，此URL會以SAML回應點AEM擊您的例項。SAML驗證處理常常會以/saml_login截取所有URL，但最好在您的應用程式根目錄後附加它。
* **SAML對象**:這是您應用程式的網域URL。請勿在網域URL中使用通訊協定（http或https）。
* **SAML名稱ID：從** 下拉式清單中選取「電子郵件」。
* **環境**:選擇您的適當環境。
* **屬性**:這些是您在SAML回應中取得的使用者相關屬性。根據您的需求指定它們。


![okta應用程式](assets/okta-app-settings-blurred.PNG)


## 將OKTA(IdP)憑證新增至信任AEM商店

由於SAML斷言是加密的，因此我們需要將IdP(OKTA)憑證新增至信任存放AEM區，以允許OKTA和之間進行安全通AEM訊。
[初始化信任儲存](http://localhost:4502/libs/granite/security/content/truststore.html)，如果尚未初始化。記住信任存放區密碼。 在此過程中，我們以後需要使用此密碼。

* 導覽至[全域信任商店](http://localhost:4502/libs/granite/security/content/truststore.html)。
* 按一下「從CER檔案添加證書」。 新增OKTA提供的IdP憑證，然後按一下「送出」。

   >[!NOTE]
   >
   >請勿將憑證對應至任何使用者

將憑證新增至信任存放區時，您應取得憑證別名，如下方螢幕擷取畫面所示。 在您的情況下，別名可能不同。

![認證別名](assets/cert-alias.PNG)

**記下證書別名。在後續步驟中，您將需要此功能。**

### 設定SAML驗證處理常式

導覽至[configMgr](http://localhost:4502/system/console/configMgr)。
搜尋並開啟「Adobe花崗岩SAML 2.0驗證處理常式」。
提供下列屬性，如下所指定
以下是需要指定的關鍵屬性：

* **path**  —— 這是觸發驗證處理常式的路徑
* **IdP Url**：這是OKTA提供的IdP url
* **IDP憑證別名**：這是您將IdP憑證新增至信任存放區時AEM取得的別名
* **服務提供者實體Id**：這是您的伺服器AEM名稱
* **Key store的密碼**：這是您使用的信任存放區密碼
* **預設重新導向**：這是成功驗證時重新導向至的URL
* **UserID屬性**:uid
* **使用加密**:false
* **自動建立CRX用戶**:true
* **新增至群組**:true
* **預設群組**:oktausers(這是新增使用者的群組。您可以在中提供任何現有群組AEM)
* **NamedIDPolicy**:指定名稱標識符的約束條件，該標識符用於表示所請求的主題。複製並貼上下列反白顯示的字串&#x200B;**urn:oasis:names:tc:SAML:2.0:nameidformat:emailAddress**
* **同步屬性** -這些屬性是從描述檔中的SAML斷言儲存的AEM屬性

![saml-authentication-handler](assets/saml-authentication-settings-blurred.PNG)

### 設定Apache Sling Referrer Filter

導覽至[configMgr](http://localhost:4502/system/console/configMgr)。
搜尋並開啟「Apache Sling Referrer Filter」。請依下列指定設定下列屬性：

* **允許空**:true
* **允許主機**:IdP的主機名稱（在您的情況下會有所不同）
* **允許Regexp主機**:IdP的主機名稱（在您的情況下會不同）Sling Referrer Filter Referrer屬性螢幕擷取

![反向連結篩選](assets/sling-referrer-filter.PNG)

#### 為OKTA整合設定DEBUG記錄

在上設定OKTA整合時，AEM請參閱SAML驗證處理常式的DEBUG記AEM錄檔。 若要將記錄層級設為DEBUG，請透過AEMOSGi Web Console建立新的Sling Logger設定。

請記得在「舞台」和「生產」上移除或停用此記錄器，以減少記錄雜訊。

在上設定OKTA整合時，可AEM以檢視SAML驗證處理常式的DEBUG記AEM錄檔。 若要將記錄層級設為DEBUG，請透過AEMOSGi Web Console建立新的Sling Logger設定。
**請記得在「舞台」和「生產」上移除或停用此記錄器，以減少記錄雜訊。**
* 導覽至[configMgr](http://localhost:4502/system/console/configMgr)

* 搜尋並開啟「Apache Sling Logging Logger Configuration」
* 使用以下配置建立記錄器：
   * **記錄層級**:除錯
   * **日誌檔案**:logs/saml.log
   * **Logger**:com.adobe.granite.auth.saml
* 按一下儲存以儲存您的設定



#### 測試您的OKTA設定

註銷實例AEM。 嘗試存取連結。 您應該會看到OKTA SSO的實際運作。
