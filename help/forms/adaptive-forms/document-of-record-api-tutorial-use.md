---
title: 使用API與AEM Forms生成記錄文檔
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

# 使用API生成AEM Forms的記錄文檔 {#using-api-to-generate-document-of-record-with-aem-forms}

以寫程式方式生成記錄文檔(DOR)

本文說明了 `com.adobe.aemds.guide.addon.dor.DoRService API` 生成 **記錄文檔** 以寫程式方式。 [記錄文檔](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-advanced-authoring/generate-document-of-record-for-non-xfa-based-adaptive-forms.html) 是自適應表單中捕獲的資料的PDF版本。

1. 以下是代碼段。 第一行是DOR服務。
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

要在本地系統上嘗試此操作，請執行以下步驟

1. [使用包管理器下載並安裝文章資產](assets/dor-with-api.zip)
1. 確保已安裝並啟動作為 [建立服務用戶項目](service-user-tutorial-develop.md)
1. [登錄到configMgr](http://localhost:4502/system/console/configMgr)
1. 搜索Apache Sling服務用戶映射器服務
1. 確保輸入以下條目 _DevegingWithServiceUser.core:getformsresourceresolver=fd-service_ 在「服務映射」部分中
1. [開啟窗體](http://localhost:4502/content/dam/formsanddocuments/sandbox/1201-borrower-payments/jcr:content?wcmmode=disabled)
1. 填寫表單，然後按一下「查看PDF」
1. 您應該在瀏覽器的新頁籤中看到DOR


**故障排除提示**

PDF不顯示在新瀏覽器頁籤中：

1. 確保未在瀏覽器中阻止彈出窗口
1. 確保以管理員身AEM份啟動伺服器（至少在Windows上）
1. 確保「DevelopingWithServiceUser」捆綁包位於 *活動狀態*
1. [確保系統用戶](http://localhost:4502/useradmin) 「fd-service」對以下節點具有讀取、修改和建立權限 `/content/usergenerated/content/aemformsenablement`
