---
title: 配置OKTA AEM
description: 瞭解使用OKTA使用單點登錄的各種配置設定。
version: 6.5
topic: Integrations, Security, Administration
feature: Integrations
role: Admin
level: Experienced
jira: KT-12305
last-substantial-update: 2023-03-01T00:00:00Z
exl-id: 460e9bfa-1b15-41b9-b8b7-58b2b1252576
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '782'
ht-degree: 0%

---

# 使用OKTA向AEM作者驗證

> 請參閱 [SAML 2.0身份驗證](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/authentication/saml-2-0.html) 有關如何設定OKTA的說明，請AEMas a Cloud Service。

第一步是在OKTA門戶上配置您的應用。 一旦您的應用獲得OKTA管理員的批准，您將有權訪問IdP證書並單一登錄URL。 以下是註冊新應用程式時通常使用的設定。

* **應用程式名稱：** 這是您的應用程式名稱。 確保為應用程式指定唯一的名稱。
* **SAML收件人：** 從OKTA進行身份驗證後，這是SAML響應在實例AEM上命中的URL。 SAML身份驗證處理程式通常使用/ saml_login攔截所有URL，但最好在應用程式根目錄後追加它。
* **SAML受眾**:這是應用程式的域URL。 不要在域URL中使用協定（http或https）。
* **SAML名稱ID:** 從下拉清單中選擇「電子郵件」。
* **環境**:選擇適當的環境。
* **屬性**:這些是您在SAML響應中獲得的有關用戶的屬性。 根據需要指定它們。


![奧克塔應用](assets/okta-app-settings-blurred.PNG)


## 將OKTA(IdP)證書添加到信AEM任儲存

由於SAML斷言是加密的，因此需要將IdP(OKTA)證書添加到信AEM任儲存中，以允許OKTA和之間進行安全通AEM信。
[初始化信任儲存](http://localhost:4502/libs/granite/security/content/truststore.html)，如果尚未初始化。
記住信任儲存密碼。 我們以後需要在此過程中使用此密碼。

* 導航到 [全局信任儲存](http://localhost:4502/libs/granite/security/content/truststore.html)。
* 按一下「從CER檔案添加證書」。 添加OKTA提供的IdP證書，然後按一下提交。

   >[!NOTE]
   >
   >請不要將證書映射到任何用戶

在將證書添加到信任儲存時，您應獲得以下螢幕快照中所示的證書別名。 別名在您的情況下可能不同。

![證書別名](assets/cert-alias.PNG)

**記下證書別名。 在後續步驟中，您需要這個。**

### 配置SAML身份驗證處理程式

導航到 [configMgr](http://localhost:4502/system/console/configMgr)。
搜索並開啟「Adobe花崗岩SAML 2.0身份驗證處理程式」。
提供以下指定的屬性以下是需要指定的關鍵屬性：

* **路徑**  — 這是觸發驗證處理程式的路徑
* **IDP URL**：這是OKTA提供的IdP URL
* **IDP證書別名**：這是在將IdP證書添加到信任儲存中時AEM獲得的別名
* **服務提供商實體ID**：這是伺服器的名AEM稱
* **密鑰儲存的密碼**：這是您使用的信任儲存密碼
* **預設重定向**：這是成功驗證時要重定向到的URL
* **用戶ID屬性**:uid
* **使用加密**:false
* **自動建立CRX用戶**:true
* **添加到組**:true
* **預設組**:oktausers(這是用戶添加到的組。 您可以在中提供任何現有組AEM)
* **命名IDPolicy**:指定用於表示所請求主題的名稱標識符的約束。 複製並貼上以下突出顯示的字串 **甕:oasis:名稱:tc:SAML:2.0:nameidformat:電子郵件地址**
* **同步屬性**  — 這些屬性是從配置檔案中的SAML斷言儲存AEM的

![saml-authentication-handler](assets/saml-authentication-settings-blurred.PNG)

### 配置Apache Sling引用篩選器

導航到 [configMgr](http://localhost:4502/system/console/configMgr)。
搜索並開啟「Apache Sling引用過濾器」。按如下指定設定以下屬性：

* **允許空**:假
* **允許主機**:IdP的主機名（這與您的情況不同）
* **允許Regexp主機**:IdP的主機名（這在您的情況下不同）Sling引用者篩選器引用者屬性螢幕快照

![引用過濾器](assets/okta-referrer.png)

#### 為OKTA整合配置DEBUG日誌

在上設定OKTA整合時AEM，檢查SAML驗證處理程式的DEBUG日AEM志會很有幫助。 要將日誌級別設定為DEBUG，請通過AEMOSGi Web控制台建立新的Sling Logger配置。

切記在舞台和生產上刪除或禁用此記錄器以減少日誌噪音。

在上設定OKTA整合時AEM，檢查SAML驗證處理程式的DEBUG日AEM志會很有幫助。 要將日誌級別設定為DEBUG，請通過AEMOSGi Web控制台建立新的Sling Logger配置。
**切記在舞台和生產上刪除或禁用此記錄器以減少日誌噪音。**
* 導航到 [configMgr](http://localhost:4502/system/console/configMgr)

* 搜索並開啟「Apache Sling日誌記錄記錄器配置」
* 使用下列設定來建立記錄器：
   * **日誌級別**:調試
   * **日誌檔案**:logs/saml.log
   * **記錄器**:com.adobe.granite.auth.saml
* 按一下保存以保存設定

#### TestOKTA配置

註銷實AEM例。 嘗試訪問連結。 您應看到OKTA SSO正在執行。
