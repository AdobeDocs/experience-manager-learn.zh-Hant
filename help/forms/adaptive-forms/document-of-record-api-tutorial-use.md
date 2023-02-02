---
title: 使用API產生含有AEM Forms的記錄檔案
description: 以寫程式方式生成記錄文檔(DOR)
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 9a3b2128-a383-46ea-bcdc-6015105c70cc
last-substantial-update: 2023-01-26T00:00:00Z
source-git-commit: ddef90067d3ae4a3c6a705b5e109e474bab34f6d
workflow-type: tm+mt
source-wordcount: '261'
ht-degree: 0%

---

# 使用API在AEM Forms中產生記錄檔案 {#using-api-to-generate-document-of-record-with-aem-forms}

以寫程式方式生成記錄文檔(DOR)

本文說明的使用 `com.adobe.aemds.guide.addon.dor.DoRService API` 產生 **記錄檔案** 寫程式。 [記錄檔案](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-advanced-authoring/generate-document-of-record-for-non-xfa-based-adaptive-forms.html) 是在適用性表單中擷取的資料的PDF版本。

1. 以下是程式碼片段。 第一行取得DOR服務。
1. 設定DoROptions。
1. 調用DoRService的呈現方法，並將DoROptions對象傳遞到呈現方法

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

要在本地系統上嘗試，請執行以下步驟

1. [使用封裝管理程式下載及安裝文章資產](assets/dor-with-api.zip)
1. 請確定您已安裝並啟動作為 [建立服務用戶文章](service-user-tutorial-develop.md)
1. [登入configMgr](http://localhost:4502/system/console/configMgr)
1. 搜尋Apache Sling Service使用者對應程式服務
1. 請務必輸入下列項目 _DevelopingWithServiceUser.core:getformsresourceresolver=fd-service_ （在「服務映射」部分）
1. [開啟表單](http://localhost:4502/content/dam/formsanddocuments/sandbox/1201-borrower-payments/jcr:content?wcmmode=disabled)
1. 填寫表單，然後按一下「 ViewPDF」
1. 您應會在瀏覽器的新索引標籤中看到DOR


**疑難排解提示**

PDF未顯示在新的瀏覽器標籤中：

1. 請確定您未封鎖瀏覽器中的快顯視窗
1. 請確定您是以管理員身分啟動AEM伺服器（至少在windows上）
1. 確認「DevelopingWithServiceUser」套件組合位於 *活動狀態*
1. [確保系統用戶](http://localhost:4502/useradmin) 「fd-service」具有以下節點的讀取、修改和建立權限 `/content/usergenerated/content/aemformsenablement`
