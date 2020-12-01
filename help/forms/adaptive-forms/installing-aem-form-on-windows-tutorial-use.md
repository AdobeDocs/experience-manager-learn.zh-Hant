---
title: 在Windows上安裝AEM表單的簡化步驟
seo-title: 在Windows上安裝AEM表單的簡化步驟
description: 在Windows上安裝AEM Forms的快速簡單步驟
seo-description: 在Windows上安裝AEM Forms的快速簡單步驟
uuid: a148b8f0-83db-47f6-89d3-c8a9961be289
feature: adaptive-forms
topics: administration
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
discoiquuid: 1182ef4d-5838-433b-991d-e24ab805ae0e
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 1%

---


# 在Windows上安裝AEM表單的簡化步驟

>[!NOTE]
>
>如果您想要使用AEM Forms，請勿按兩下「AEM快速入門」jar。
>
>此外，請確定「AEM Forms安裝」檔案夾路徑中沒有空格。
>
>例如，請勿在c:\jack and jill\AEM Forms folder中安裝AEM Forms

>[!NOTE]
>
>如果您要安裝AEM Forms 6.5，請確定您已安裝下列32位元Microsoft Visual C++可轉散發套件。
>
>* Microsoft Visual C++ 2008可重新分發
>* Microsoft Visual C++ 2010可重新分發
>* Microsoft Visual C++ 2012可重新分發
>* Microsoft Visual C++ 2013可重新分發（截止至6.5日）


雖然我們建議依照[官方檔案](https://helpx.adobe.com/experience-manager/6-3/forms/using/installing-configuring-aem-forms-osgi.html)安裝AEM Forms。 請依照下列步驟，在Windows環境中安裝及設定AEM Forms:

* 請確定您已安裝適當的JDK
   * AEM 6.2，您需要：Oracle SE 8 JDK 1.8.x（64位）
* 
   * AEM 6.3和AEM 6.4，您需要：Oracle SE 8 JDK 1.8.x（64位）
* AEM 6.5需要JDK 8或JDK 11
* [此處列](https://helpx.adobe.com/experience-manager/6-3/sites/deploying/using/technical-requirements.html) 出正式JDK要求
* 請確定JAVA_HOME已設定為指向已安裝的JDK。
   * 要在Windows中建立JAVA_HOME變數，請執行以下步驟：
      * 按一下右鍵「My Computer（我的電腦）」 ，然後選擇「Properties（屬性）」
      * 在「高級」頁籤上，選擇「環境變數」並建立名為JAVA_HOME的新系統變數。
      * 將變數值設為指向安裝在系統上的JDK。 例如c:\program files\java\jdk1.8.0_25

* 在C驅動器上建立名為AEMForms的資料夾
* 找到AEMQuickStart.Jar並將其移入AEMForms資料夾
* 將license.properties檔案複製到此AEMForms資料夾
* 使用下列內容建立名為「StartAemForms.bat」的批次檔案：
   * java -d64 -Xmx2048M -jar AEM_6.3_Quickstart.jar -gui。此處AEM_6.3_Quickstart.jar是我的AEM快速入門jar的名稱。
   * 您可以將jar更名為任何名稱，但請確保該名稱反映在批處理檔案中。將批處理檔案保存在AEMForms資料夾中。

* 開啟新的命令提示，並導覽至c:\aemforms。

* 從命令提示符執行StartAemForms.bat檔案。

* 應該會出現一個小對話框，指示啟動的進度。

* 啟動完成後，請開啟sling.properties檔案。 這位於c:\AEMForms\crx-quickstart\conf folder。

* 將下列2行複製到檔案底部
   * **sling.bootdelegation.class.com.rsa.jsafe.provider.JsafeJCE=com.rsa。*** **sling.bootdelegation.class.org.bouncycastle.jce.provider.BuncyCastleProvider=org.bouncycastle.***
* 檔案服務必須具備這兩個屬性才能運作
* 儲存sling.properties檔案

* [登入封裝共用 ](http://localhost:4502/crx/packageshare/login.html)

   * 您需要AdobeId才能登入以套件共用
   * 搜尋適合您AEM Forms和作業系統版本的AEM Forms Add on套件
   * 或者，您也可以下載適當的表單addon package[](https://helpx.adobe.com/aem-forms/kb/aem-forms-releases.html)
   * 安裝Add on軟體包後，需要遵循以下步驟

      * **請確定所有束都處於活動狀態。（除AEMFD簽名套裝外）。**
      * **所有捆綁包通常需要5分鐘或更長時間才能進入活動狀態。**
   * **當所有整合都生效（AEMFD簽章整合除外）後，請重新啟動您的系統以完成AEM Forms安裝**


* 將`sun.util.calendar`軟體包添加到允許的清單：

   1. 在您的[瀏覽器視窗](http://localhost:4502/system/console/configMgr)中開啟Felix網頁主控台
   2. 搜索並開啟還原序列化防火牆配置：`com.adobe.cq.deserfw.impl.DeserializationFirewallImpl`
   3. 在`com.adobe.cq.deserfw.impl.DeserializationFirewallImpl.firewall.deserialization.whitelist.name`下將`sun.util.calendar`新增為新項目
   4. 儲存變更。

恭喜！!! 您現在已在您的系統上安裝並設定AEM Forms。
視您的需求而定，您可在伺服器上設定[Reader Extensions](https://helpx.adobe.com/experience-manager/6-3/forms/using/configuring-document-services.html)或[ PDFG](https://helpx.adobe.com/experience-manager/6-3/forms/using/install-configure-pdf-generator.html)
