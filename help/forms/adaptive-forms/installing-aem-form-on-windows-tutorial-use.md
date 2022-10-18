---
title: 在Windows上安裝AEM Forms的簡化步驟
description: 在windows上快速輕鬆安裝AEM Forms
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
topic: Development
role: Admin
level: Beginner
exl-id: 80288765-0b51-44a9-95d3-3bdb2da38615
source-git-commit: 09f6c4b0bec10edd306270a7416fcaff8a584e76
workflow-type: tm+mt
source-wordcount: '578'
ht-degree: 5%

---

# 在Windows上安裝AEM Forms的簡化步驟

>[!NOTE]
>
>如果您要使用AEM Forms，請勿連按兩下AEM快速入門Jar。
>
>此外，請確定AEM Forms安裝資料夾路徑中沒有空格。
>
>例如，請勿在c:\jack and jill\AEM Forms folder中安裝AEM Forms

>[!NOTE]
>
>如果您要安裝AEM Forms 6.5，請確定您已安裝下列32位元Microsoft Visual C++可轉散發套件。
>
>* Microsoft Visual C++ 2008可再發行
>* Microsoft Visual C++ 2010可再發行
>* Microsoft Visual C++ 2012可再發行
>* Microsoft Visual C++ 2013可再發行（截至6.5日）


雖然我們建議遵循 [官方檔案](https://helpx.adobe.com/tw/experience-manager/6-3/forms/using/installing-configuring-aem-forms-osgi.html) 安裝AEM Forms。 可依照下列步驟，在Windows環境上安裝和設定AEM Forms:

* 請確定您已安裝適當的JDK
   * AEM 6.2您需要：OracleSE 8 JDK 1.8.x（64位）
* 
   * AEM 6.3和AEM 6.4，您需要：OracleSE 8 JDK 1.8.x（64位）
* AEM 6.5您需要JDK 8或JDK 11
* [官方JDK需求](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/introduction/technical-requirements.html?lang=zh-Hant) 此處列出
* 請確定JAVA_HOME已設定為指向已安裝的JDK。
   * 要在windows中建立JAVA_HOME變數，請執行以下步驟：
      * 按一下右鍵「My Computer（我的電腦）」並選擇「Properties（屬性）」
      * 在「高級」頁簽上，選擇「環境變數」並建立名為JAVA_HOME的新系統變數。
      * 設定變數值，以指向系統上安裝的JDK。 例如c:\program files\java\jdk1.8.0_25

* 在C驅動器上建立名為AEMForms的資料夾
* 找到AEMQuickStart.Jar並將其移入AEMForms資料夾
* 將license.properties檔案複製到此AEMForms資料夾
* 建立名為「StartAemForms.bat」的批次檔案，其中包含下列內容：
   * `java -d64 -Xmx2048M -jar AEM_6.5_Quickstart.jar -gui`
      * 其中AEM_6.5_Quickstart.jar是我的AEM Quickstart Jar的名稱。
   * 您可以將jar重新命名為任何名稱，但請確定該名稱反映在批次檔案中。 將批處理檔案保存在AEMForms資料夾中。

* 開啟新的命令提示符，然後導覽至 _c:\aemforms_.

* 從命令提示符執行StartAemForms.bat檔案。

* 您應該會看到一個小對話框，指明啟動的進度。

* 啟動完成後，開啟sling.properties檔案。 這位於c:\AEMForms\crx-quickstart\conf folder。

* 將下列2行複製到檔案底部
   * **sling.bootdelegation.class.com.rsa.jsafe.provider.JsafeJCE=com.rsa。&#42;** **sling.bootdelegation.class.org.bouncycastle.jce.provider.BouncyCastleProvider=org.buncycastle。&#42;**
* 檔案服務必須有這兩個屬性才能運作
* 儲存sling.properties檔案
* [下載適當的表單addon套件](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases.html?lang=en)
* 使用安裝Forms附加套件 [套件管理器。](http://localhost:4502/crx/packmgr/index.jsp)
* 安裝到軟體包後，需要執行下列步驟

       * **確認所有套件組合都處於作用中狀態。 （AEMFD簽名包除外）。**
       * **所有套件通常需要5分鐘或更長時間才能進入作用中狀態。**
   
   * **所有套件組合都生效後（AEMFD簽名套件組合除外），請重新啟動系統以完成AEM Forms安裝**

## sun.util.calendar包到允許的清單

1. 在您的 [瀏覽器視窗](http://localhost:4502/system/console/configMgr)
2. 搜索並開啟反序列化防火牆配置： `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl`
3. 新增 `sun.util.calendar` 作為新條目 `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl.firewall.deserialization.whitelist.name`
4. 儲存變更。

恭喜!!! 您現在已在系統上安裝並設定AEM Forms。
視您的需求而定，您可以設定  [Reader擴充功能](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/configuring-reader-extension-osgi.html?lang=en) 或 [ PDFG](https://experienceleague.adobe.com/docs/experience-manager-64/forms/install-aem-forms/osgi-installation/install-configure-document-services.html?lang=zh-Hant) 伺服器
