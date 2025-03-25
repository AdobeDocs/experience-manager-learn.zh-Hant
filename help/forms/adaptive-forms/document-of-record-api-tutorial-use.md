---
title: 透過AEM Forms使用API產生記錄檔案
description: 以程式設計方式產生記錄檔案(DOR)
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 9a3b2128-a383-46ea-bcdc-6015105c70cc
last-substantial-update: 2023-01-26T00:00:00Z
duration: 67
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '239'
ht-degree: 0%

---

# 在AEM Forms中使用API產生記錄檔案 {#using-api-to-generate-document-of-record-with-aem-forms}

以程式設計方式產生記錄檔案(DOR)

本文說明如何使用`com.adobe.aemds.guide.addon.dor.DoRService API`以程式設計方式產生&#x200B;**記錄檔案**。 [記錄檔案](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-advanced-authoring/generate-document-of-record-for-non-xfa-based-adaptive-forms.html)是在最適化表單中擷取的資料的PDF版本。

1. 以下是程式碼片段。 第一行取得DOR服務。
1. 設定DoROptions。
1. 叫用DoRService的轉譯方法，並將DoROptions物件傳遞給轉譯方法

```java
String dataXml = request.getParameter("data");
System.out.println("Got " + dataXml);
Session session;
com.adobe.aemds.guide.addon.dor.DoRService dorService = sling.getService(com.adobe.aemds.guide.addon.dor.DoRService.class);
System.out.println("Got ... DOR Service");
com.mergeandfuse.getserviceuserresolver.GetResolver aemDemoListings = sling.getService(com.mergeandfuse.getserviceuserresolver.GetResolver.class);
System.out.println("Got aem DemoListings");
resourceResolver = aemDemoListings.getFormsServiceResolver();
session = resourceResolver.adaptTo(Session.class);
resource = resourceResolver.getResource("/content/forms/af/sandbox/1201-borrower-payments");
com.adobe.aemds.guide.addon.dor.DoROptions dorOptions = new com.adobe.aemds.guide.addon.dor.DoROptions();
dorOptions.setData(dataXml);
dorOptions.setFormResource(resource);
java.util.Locale locale = new java.util.Locale("en");
dorOptions.setLocale(locale);
com.adobe.aemds.guide.addon.dor.DoRResult dorResult = dorService.render(dorOptions);
byte[] fileBytes = dorResult.getContent();
com.adobe.aemfd.docmanager.Document dorDocument = new com.adobe.aemfd.docmanager.Document(fileBytes);
resource = resourceResolver.getResource("/content/usergenerated/content/aemformsenablement");
Node paydotgov = resource.adaptTo(Node.class);
java.util.Random r = new java.util.Random();
String nodeName = Long.toString(Math.abs(r.nextLong()), 36);
Node fileNode = paydotgov.addNode(nodeName + ".pdf", "nt:file");

System.out.println("Created file Node...." + fileNode.getPath());
Node contentNode = fileNode.addNode("jcr:content", "nt:resource");
Binary binary = session.getValueFactory().createBinary(dorDocument.getInputStream());
contentNode.setProperty("jcr:data", binary);
JSONWriter writer = new JSONWriter(response.getWriter());
writer.object();
writer.key("filePath");
writer.value(fileNode.getPath());
writer.endObject();
session.save();
```

若要在本機系統上嘗試此方法，請遵循下列步驟

1. [使用封裝管理程式下載及安裝文章資產](assets/dor-with-api.zip)
1. 請確定您已安裝並啟動作為[建立服務使用者文章](service-user-tutorial-develop.md)的一部分提供的DevelopingWithServiceUser套件
1. [登入configMgr](http://localhost:4502/system/console/configMgr)
1. 搜尋Apache Sling服務使用者對應程式服務
1. 在[服務對應]區段中確定下列專案&#x200B;_DevelopingWithServiceUser.core：getformsresourceresolver=fd-service_
1. [開啟表單](http://localhost:4502/content/dam/formsanddocuments/sandbox/1201-borrower-payments/jcr:content?wcmmode=disabled)
1. 填寫表單並按一下「檢視PDF」
1. 您應該會在瀏覽器的新標籤中看到DOR


**疑難排解提示**

PDF不會顯示在新的瀏覽器標籤中：

1. 請確定您未封鎖瀏覽器中的快顯視窗
1. 請確定您是以管理員身分啟動AEM伺服器（至少在Windows上）
1. 確定&#39;DevelopingWithServiceUser&#39;組合處於&#x200B;*作用中狀態*
1. [請確定系統使用者](http://localhost:4502/useradmin) &#39; fd-service&#39;在下列節點`/content/usergenerated/content/aemformsenablement`上具有[讀取]、[修改]和[建立]許可權
