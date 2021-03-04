---
title: 在AEM Forms使用PDFG
seo-title: 在AEM Forms使用PDFG
description: 展示使用AEM Forms建立PDF的拖放功能
seo-description: 展示使用AEM Forms建立PDF的拖放功能
uuid: ee54edb4-a7b1-42ed-81ea-cb6bef6cf97f
feature: pdf-generator
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 7f570f12-ce43-4da7-a249-ef6bd0fe48e5
topic: 開發
role: 開發人員
level: 中級
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '285'
ht-degree: 2%

---


# 在AEM Forms使用PDFG{#using-pdfg-in-aem-forms}

展示使用AEM Forms建立PDF的拖放功能

PDFG代表PDF產生。 這表示您可以將多種檔案格式轉換為PDF。 最常見的是Microsoft Office檔案。 PDFG自6.1起就成為AEM Forms的一部分。
[此處列出了PDFG API的javadoc](https://helpx.adobe.com/experience-manager/6-3/forms/using/aem-document-services-programmatically.html#PDFGeneratorService)

與本文相關的資產可讓您將MS office檔案或JPG檔案拖放至HTML頁面的拖放區。 檔案一旦被丟棄，就會調用PDF服務，將檔案轉換為PDF，並保存到伺服器的檔案AEM系統中。

若要安裝示範資產，請執行下列步驟

1. 如本檔案[此處](https://helpx.adobe.com/tw/experience-manager/6-4/forms/using/install-configure-pdf-generator.html)所述配置PDF。
1. 請依照您的AEM Forms版相關檔案進行。
1. [使用套件管理員匯入及安裝與本文相關的資產。](assets/createpdfgdemov2.zip)
1. [導覽至post.](http://localhost:4502/apps/AemFormsSamples/components/createPDF/POST.jsp) jspin您的CRX
1. 根據您的偏好設定變更儲存位置（第9行）
1. 儲存您的變更。
1. 開啟[ html page](http://localhost:4502/content/DocumentServices/CreatePDFG.html)以拖放檔案進行轉換。
1. 將單字檔案或jpg拖放至拖放區。
1. 輸入檔案將轉換為PDF並儲存在與第4點中指定的相同位置。

下列程式碼片段顯示PDFG服務在將檔案轉換為PDF時的使用情形

```java
com.adobe.pdfg.service.api.GeneratePDFService pdfService = sling.getService(com.adobe.pdfg.service.api.GeneratePDFService.class);
System.out.println("Got PDF Service");
java.util.Map map = pdfService.createPDF(uploadedDocument,fileName,"","Standard","No Security", null, null);
```

