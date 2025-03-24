---
title: 在Windows上安裝AEM Forms的簡化步驟
description: 在Windows上快速輕鬆地安裝AEM Forms的步驟
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.5
topic: Development
role: Admin
level: Beginner
exl-id: 80288765-0b51-44a9-95d3-3bdb2da38615
last-substantial-update: 2020-06-09T00:00:00Z
duration: 113
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '545'
ht-degree: 1%

---

# 在Windows上安裝AEM Forms的簡化步驟

>[!NOTE]
>
>如果您打算使用AEM Forms，請勿連按兩下AEM快速入門jar 。
>
>此外，請確定AEM Forms安裝資料夾路徑中沒有空格。
>
>例如，請勿在c：\jack和jill\AEM Forms資料夾中安裝AEM Forms

>[!NOTE]
>
>如果您正在安裝AEM Forms 6.5，請確定您已安裝下列32位元Microsoft Visual C++可轉散發套件。
>
>* Microsoft Visual C++ 2008可轉散發套件
>* Microsoft Visual C++ 2010可轉散發套件
>* Microsoft Visual C++ 2012可轉散發套件
>* Microsoft Visual C++ 2013可轉散發套件（自6.5起）

雖然我們建議遵循安裝AEM Forms的[官方檔案](https://helpx.adobe.com/tw/experience-manager/6-3/forms/using/installing-configuring-aem-forms-osgi.html)。 您可以依照下列步驟，在Windows環境中安裝和設定AEM Forms：

* 請確定您已安裝適當的JDK
   * 您需要AEM 6.2： Oracle SE 8 JDK 1.8.x （64位元）
   * 您需要AEM 6.3和AEM 6.4： Oracle SE 8 JDK 1.8.x （64位元）
   * AEM 6.5需要JDK 8或JDK 11
   * [此處列出官方JDK需求](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/introduction/technical-requirements.html?lang=zh-Hant)
* 請確定JAVA_HOME設定為指向您已安裝的JDK。
   * 若要在視窗中建立JAVA_HOME變數，請遵循下列步驟：
      * 用滑鼠右鍵按一下「我的電腦」並選取「內容」
      * 在「進階」標籤上，選取「環境變數」並建立名為JAVA_HOME的新系統變數。
      * 設定變數值以指向系統上安裝的JDK。 例如c：\program files\java\jdk1.8.0_25

* 在您的C磁碟機上建立名為AEMForms的資料夾
* 找到AEMQuickStart.Jar並將其移到AEMForms資料夾
* 將license.properties檔案複製到此AEMForms資料夾
* 建立名為「StartAemForms.bat」的批次檔案，其內容如下：
   * `java -d64 -Xmx2048M -jar AEM_6.5_Quickstart.jar -gui`
      * 此處，AEM_6.5_Quickstart.jar是我的AEM quickstart jar名稱。
   * 您可以將您的jar重新命名為任何名稱，但請確定該名稱會反映在批次檔案中。 將批次檔案儲存在AEMForms資料夾中。

* 開啟新的命令提示字元，並瀏覽至&#x200B;_c：\aemforms_。

* 從命令提示字元執行StartAemForms.bat檔案。

* 您應該會看到一個小型對話方塊，指出啟動進度。

* 啟動完成後，請開啟sling.properties檔案。 此檔案位於c：\AEMForms\crx-quickstart\conf資料夾。

* 將下列2行複製到檔案底部
   * **sling.bootdelegation.class.com.rsa.jsafe.provider.JsafeJCE=com.rsa。&#42;** **sling.bootdelegation.class.org.bouncycastle.jce.provider.BouncyCastleProvider=org.bouncycastle.&#42;**
* 檔案服務需要這兩個屬性才能運作
* 儲存sling.properties檔案
* [下載適當的表單附加元件套件](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases.html?lang=en)
* 使用[封裝管理員](http://localhost:4502/crx/packmgr/index.jsp)安裝表單附加套件。
* 安裝附加套件後，必須遵循下列步驟

   * **請確定所有組合都處於作用中狀態。 （AEMFD簽章組合除外）。**
   * **所有組合通常需要5分鐘以上的時間才會進入作用中狀態。**

   * **一旦所有套件組合都作用中（AEMFD簽章套件除外），請重新啟動您的系統以完成AEM Forms安裝**

## sun.util.calendar套件放入允許清單

1. 在[瀏覽器視窗](http://localhost:4502/system/console/configMgr)中開啟Felix網頁主控台
1. 搜尋並開啟還原序列化防火牆設定： `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl`
1. 將`sun.util.calendar`新增為`com.adobe.cq.deserfw.impl.DeserializationFirewallImpl.firewall.deserialization.whitelist.name`下的新專案
1. 儲存變更。

恭喜!!! 您現在已在系統上安裝和設定AEM Forms。
您可以視需要在伺服器上設定[Reader擴充功能](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/configuring-reader-extension-osgi.html)或[PDFG](https://experienceleague.adobe.com/docs/experience-manager-65/forms/install-aem-forms/osgi-installation/install-configure-document-services.html)
