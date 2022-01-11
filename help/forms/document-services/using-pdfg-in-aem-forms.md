---
title: 在AEM Forms中使用PDFG
description: 展示使用AEM Forms建立PDF的拖放功能
feature: PDF Generator
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: bc79fcbf-b8b3-4d3a-9cd6-0bcd9321c7d5
source-git-commit: 8d83d01fca3bfc9e6f674f7d73298b42f98a5d46
workflow-type: tm+mt
source-wordcount: '271'
ht-degree: 2%

---

# 在AEM Forms中使用PDFG{#using-pdfg-in-aem-forms}

展示使用AEM Forms建立PDF的拖放功能

PDFG代表PDF產生。 這表示您可以將各種檔案格式轉換為PDF。 最常見的是Microsoft Office文檔。 PDFG自6.1起即為AEM Forms的一部分。
[此處列出了PDFG API的javadoc](https://www.adobe.io/experience-manager/reference-materials/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html)

與本文關聯的資產可讓您將MS office文檔或JPG檔案拖放到HTML頁的拖放區。 刪除文檔後，將調用PDFG服務，並將該文檔轉換為PDF，並將其保存到AEM Server的檔案系統中。

若要安裝示範資產，請執行下列步驟

1. 依照本檔案所述設定PDFG [此處](https://helpx.adobe.com/tw/experience-manager/6-4/forms/using/install-configure-pdf-generator.html).
1. 請遵循與您的AEM Forms版本相關的適當檔案。
1. [使用套件管理器匯入和安裝與本文相關的資產。](assets/createpdfgdemov2.zip)
1. [導覽至post.jsp](http://localhost:4502/apps/AemFormsSamples/components/createPDF/POST.jsp) 在CRX中
1. 根據您的首選項更改保存位置（第9行）
1. 儲存您的變更。
1. 開啟 [  html頁面](http://localhost:4502/content/DocumentServices/CreatePDFG.html) 來拖放要轉換的檔案。
1. 將字檔或jpg放置到放置區。
1. 輸入文檔將轉換為PDF，並保存在與第4點中指定的相同位置。

下列程式碼片段顯示PDFG服務將檔案轉換為PDF的使用方式

```java
com.adobe.pdfg.service.api.GeneratePDFService pdfService = sling.getService(com.adobe.pdfg.service.api.GeneratePDFService.class);
System.out.println("Got PDF Service");
java.util.Map map = pdfService.createPDF(uploadedDocument,fileName,"","Standard","No Security", null, null);
```
