---
title: 在Windows上安裝AEM Forms的簡化步驟
description: 在Windows上安裝AEM Forms快速而簡單的步驟
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
topic: Development
role: Admin
level: Beginner
exl-id: 80288765-0b51-44a9-95d3-3bdb2da38615
source-git-commit: 5c53919dd038c0992e1fe5dd85053f26c03c5111
workflow-type: tm+mt
source-wordcount: '586'
ht-degree: 5%

---

# 在Windows上安裝AEM Forms的簡化步驟

>[!NOTE]
>
>如果要使AEM用AEM Forms，請勿按兩下「快速啟動」(Quick Start)jar。
>
>另外，確保「AEM Forms安裝」資料夾路徑中沒有空格。
>
>例如，不要在c:\jack and jill\AEM Forms folder中安裝AEM Forms

>[!NOTE]
>
>如果要安裝AEM Forms6.5，請確保已安裝以下32位MicrosoftVisual C++可再分發版。
>
>* MicrosoftVisual C++ 2008可再發行
>* MicrosoftVisual C++ 2010可再發行
>* MicrosoftVisual C++ 2012可再發行
>* MicrosoftVisual C++ 2013可再發行版（截至6.5）


儘管我們建議 [正式檔案](https://helpx.adobe.com/tw/experience-manager/6-3/forms/using/installing-configuring-aem-forms-osgi.html) 安裝AEM Forms。 可以按照以下步驟在Windows環境中安裝和配置AEM Forms:

* 確保安裝了相應的JDK
   * AEM 6.2：您需要：OracleSE 8 JDK 1.8.x（64位）
* 
   * AEM 6.3AEM和6.4:OracleSE 8 JDK 1.8.x（64位）
* AEM 6.5需要JDK 8或JDK 11
* [正式JDK要求](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/introduction/technical-requirements.html?lang=zh-Hant) 此處列出
* 確保JAVA_HOME設定為指向已安裝的JDK。
   * 要在窗口中建立JAVA_HOME變數，請執行以下步驟：
      * 按一下右鍵「My Computer（我的電腦）」並選擇「Properties（屬性）」
      * 在「高級」頁籤上，選擇「環境變數」，然後建立一個名為JAVA_HOME的新系統變數。
      * 將變數值設定為指向系統上安裝的JDK。 例如c:\program files\java\jdk1.8.0_25

* 在C驅動器上建立名為AEMForms的資料夾
* 找到AEMQuickStart.Jar並將其移到AEMForms資料夾
* 將license.properties檔案複製到此AEMForms資料夾
* 建立名為「StartAemForms.bat」的批處理檔案，其內容如下：
   * java -d64 -Xmx2048M -jar AEM6.5_Quickstart.jar -gui。 此處AEM_6.5_Quickstart.jar是我的快速啟動AEMjar的名稱。
   * 您可以將jar更名為任何名稱，但請確保該名稱反映在批處理檔案中。 將批處理檔案保存在AEMForms資料夾中。

* 開啟新的命令提示符，然後導航到 _c:\aemforms_。

* 從命令提示符執行StartAemForms.bat檔案。

* 您應該得到一個小對話框，指示啟動的進度。

* 啟動完成後，開啟sling.properties檔案。 此地址位於c:\AEMForms\crx-quickstart\conf folder。

* 將以下2行複製到檔案底部
   * **sling.bootdelegation.class.com.rsa.jsafe.provider.JsafeJCE=com.rsa。&#42;** **sling.bootdelegation.class.org.bouncycastle.jce.provider.BouncyCastleProvider=org.boncycastle。&#42;**
* 文檔服務要工作，需要這兩個屬性
* 保存sling.properties檔案
* [下載相應的表單載入包](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases.html?lang=en)
* 使用 [包管理器。](http://localhost:4502/crx/packmgr/index.jsp)
* 安裝了添加到軟體包後，需要執行以下步驟

       * **確保所有捆綁包都處於活動狀態。 （除AEMFD簽名包外）。**
       * **所有捆綁包通常需要5分鐘或更長時間才能進入活動狀態。**
   
   * **所有捆綁包都處於活動狀態（除AEMFD簽名捆綁包外）後，重新啟動系統以完成AEM Forms安裝**

## sun.util.calendar包到允許的清單

1. 在您的中開啟Felix Web控制台 [瀏覽器窗口](http://localhost:4502/system/console/configMgr)
2. 搜索並開啟反序列化防火牆配置： `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl`
3. 添加 `sun.util.calendar` 的 `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl.firewall.deserialization.whitelist.name`
4. 儲存變更。

恭喜你！!! 您現在已在系統上安裝並配置了AEM Forms。
根據您的需要，您可以配置  [Reader擴展](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/configuring-reader-extension-osgi.html?lang=en) 或 [ PDFG](https://experienceleague.adobe.com/docs/experience-manager-64/forms/install-aem-forms/osgi-installation/install-configure-document-services.html?lang=zh-Hant) 在伺服器上
