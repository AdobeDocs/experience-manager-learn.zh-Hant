---
title: 使用AEM設定OKTA
description: 了解使用okta進行單一登入的各種組態設定
feature: Adaptive Forms
version: 6.5
topic: Administration
role: Admin
level: Experienced
source-git-commit: 3109d406ed4788ab492a148d4eac94f7e5ad9f2d
workflow-type: tm+mt
source-wordcount: '759'
ht-degree: 0%

---


# 使用OKTA驗證AEM作者

第一步是在OKTA入口網站上設定您的應用程式。 一旦OKTA管理員核准您的應用程式，您就能存取IdP憑證和單一登入URL。 以下是註冊新應用程式時通常使用的設定。

* **應用程式名稱：** 這是您的應用程式名稱。請務必為您的應用程式指定唯一的名稱。
* **SAML收件者：** 從OKTA驗證之後，這會是SAML回應在您的AEM例項上點擊的URL。SAML驗證處理常式通常會以/saml_login截取所有URL，但最好在應用程式根後附加。
* **SAML對象**:這是您應用程式的網域URL。請勿在網域URL中使用通訊協定（http或https）。
* **SAML名稱ID:** 從下拉式清單中選取「電子郵件」。
* **環境**:選擇適當的環境。
* **屬性**:這些是您在SAML回應中取得關於使用者的屬性。根據您的需求指定。


![okta應用程式](assets/okta-app-settings-blurred.PNG)


## 將OKTA(IdP)憑證新增至AEM信任存放區

由於SAML斷言已加密，因此我們需要將IdP(OKTA)憑證新增至AEM信任存放區，以便OKTA與AEM之間能進行安全通訊。
[初始化信任儲存](http://localhost:4502/libs/granite/security/content/truststore.html)（如果尚未初始化）。記住信任儲存密碼。 在此過程中，我們稍後需要使用此密碼。

* 導覽至[全域信任存放區](http://localhost:4502/libs/granite/security/content/truststore.html)。
* 按一下「從CER檔案添加證書」。 新增OKTA提供的IdP憑證，然後按一下提交。

   >[!NOTE]
   >
   >請勿將憑證對應至任何使用者

將憑證新增至信任存放區時，您應該會獲得憑證別名，如下方螢幕擷取畫面所示。 在您的案例中，別名可能不同。

![憑證別名](assets/cert-alias.PNG)

**記下憑證別名。您將在後續步驟中使用此功能。**

### 配置SAML驗證處理程式

導覽至[configMgr](http://localhost:4502/system/console/configMgr)。
搜尋並開啟「AdobeGranite SAML 2.0驗證處理常式」。
提供下列屬性，如下所指定
以下是需要指定的關鍵屬性：

* **path**  — 此路徑會觸發驗證處理常式
* **IdP Url**：這是您的IdP url，由OKTA提供
* **IDP憑證別名**：這是您將IdP憑證新增至AEM信託存放區時取得的別名
* **服務提供者實體Id**：這是您的AEM伺服器的名稱
* **金鑰存放區密碼**：這是您使用的信任存放區密碼
* **預設重新導向**：這是要在成功驗證時重新導向至的URL
* **UserID屬性**:uid
* **使用加密**:false
* **自動建立CRX使用者**:true
* **新增至群組**:true
* **預設群組**:oktausers(這是將新增使用者的群組。您可以在AEM中提供任何現有群組)
* **命名IDPolicy**:指定用於表示所請求主題的名稱標識符的約束。複製並貼上下列醒目提示的字串&#x200B;**urn:oasis:names:tc:SAML:2.0:nameidformat:emailAddress**
* **同步屬性**  — 這些是從AEM設定檔中的SAML斷言儲存的屬性

![saml-authentication-handler](assets/saml-authentication-settings-blurred.PNG)

### 設定Apache Sling反向連結篩選器

導覽至[configMgr](http://localhost:4502/system/console/configMgr)。
搜尋並開啟「Apache Sling Referrer Filter」。請依照下列指定設定屬性：

* **允許空白**:false
* **允許主機**:IdP的主機名稱（在您的案例中會不同）
* **允許Regexp主機**:IdP的主機名稱（在您的情況中會不同）Sling反向連結篩選反向連結屬性螢幕擷取

![反向連結篩選](assets/okta-referrer.png)

#### 為OKTA整合設定DEBUG記錄

在AEM上設定OKTA整合時，最好檢閱AEM SAML驗證處理常式的DEBUG記錄。 若要將記錄層級設為DEBUG，請透過AEM OSGi Web Console建立新的Sling記錄器設定。

請記得在「預備」和「生產」上移除或停用此記錄器，以減少記錄雜訊。

在AEM上設定OKTA整合時，最好檢閱AEM SAML驗證處理常式的DEBUG記錄。 若要將記錄層級設為DEBUG，請透過AEM OSGi Web Console建立新的Sling記錄器設定。
**請記得在「預備」和「生產」上移除或停用此記錄器，以減少記錄雜訊。**
* 導覽至[configMgr](http://localhost:4502/system/console/configMgr)

* 搜尋並開啟「Apache Sling Logging Logger Configuration」
* 使用以下配置建立記錄器：
   * **記錄層級**:除錯
   * **記錄檔**:logs/saml.log
   * **記錄器**:com.adobe.granite.auth.saml
* 按一下儲存以儲存您的設定



#### 測試您的OKTA設定

登出您的AEM例項。 嘗試存取連結。 您應該會看到OKTA SSO正在運作。
