---
title: 在AEM Forms中使用PDFG
description: 展示使用AEM Forms建立PDF的拖放功能
feature: PDF 產生器
version: 6.4,6.5
topic: 開發
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '268'
ht-degree: 3%

---


# 在AEM Forms中使用PDFG{#using-pdfg-in-aem-forms}

展示使用AEM Forms建立PDF的拖放功能

PDFG代表PDF產生。 這表示您可以將各種檔案格式轉換為PDF。 最常見的是Microsoft Office文檔。 PDFG自6.1起即為AEM Forms的一部分。
[此處列出了PDFG API的javadoc](https://helpx.adobe.com/experience-manager/6-3/forms/using/aem-document-services-programmatically.html#PDFGeneratorService)

與本文相關聯的資產可讓您將MS office檔案或JPG檔案拖放至HTML頁面的放置區。 刪除文檔後，將調用PDFG服務，並將文檔轉換為PDF，並將其保存在AEM Server的檔案系統中。

若要安裝示範資產，請執行下列步驟

1. 如本檔案[此處](https://helpx.adobe.com/tw/experience-manager/6-4/forms/using/install-configure-pdf-generator.html)所述配置PDFG。
1. 請遵循與您的AEM Forms版本相關的適當檔案。
1. [使用套件管理器匯入和安裝與本文相關的資產。](assets/createpdfgdemov2.zip)
1. [導覽至post.](http://localhost:4502/apps/AemFormsSamples/components/createPDF/POST.jsp) jspin您的CRX
1. 根據您的首選項更改保存位置（第9行）
1. 儲存您的變更。
1. 開啟[ html頁面](http://localhost:4502/content/DocumentServices/CreatePDFG.html)以拖放要轉換的檔案。
1. 將字檔或jpg放置到放置區。
1. 輸入文檔將轉換為PDF並保存在第4點中指定的同一位置。

下列程式碼片段說明如何使用PDFG服務將檔案轉換為PDF

```java
com.adobe.pdfg.service.api.GeneratePDFService pdfService = sling.getService(com.adobe.pdfg.service.api.GeneratePDFService.class);
System.out.println("Got PDF Service");
java.util.Map map = pdfService.createPDF(uploadedDocument,fileName,"","Standard","No Security", null, null);
```

