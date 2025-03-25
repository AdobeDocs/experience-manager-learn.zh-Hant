---
title: 在AEM Forms中使用PDFG
description: 示範使用AEM Forms建立PDF的拖放功能
feature: PDF Generator
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: bc79fcbf-b8b3-4d3a-9cd6-0bcd9321c7d5
last-substantial-update: 2020-07-07T00:00:00Z
duration: 52
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 0%

---

# 在AEM Forms中使用PDFG{#using-pdfg-in-aem-forms}

示範使用AEM Forms建立PDF的拖放功能

PDFG代表PDF Generation。 這表示您可以將多種檔案格式轉換成PDF。 最常見的是Microsoft Office檔案。 PDFG自6.1起即是AEM Forms的一部分。
[此處列出PDFG API的Javadoc](https://www.adobe.io/experience-manager/reference-materials/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html)

與本文相關的資產可讓您將MS Office檔案或JPG檔案拖放至HTML頁面的拖放區域。 檔案一經卸下，就會叫用PDFG服務，並將檔案轉換成PDF，儲存到AEM伺服器的檔案系統中。

若要安裝示範資產，請執行以下步驟

1. 依照此檔案[此處](https://helpx.adobe.com/experience-manager/6-4/forms/using/install-configure-pdf-generator.html)所述設定PDFG。
1. 請依照與您AEM Forms版本相關的適當檔案操作。
1. [使用封裝管理員匯入和安裝與本文相關的資產。](assets/createpdfgdemov2.zip)
1. [在您的CRX中導覽至post.jsp](http://localhost:4502/apps/AemFormsSamples/components/createPDF/POST.jsp)
1. 根據您的偏好變更儲存位置（第9行）
1. 儲存您的變更。
1. 開啟[html頁面](http://localhost:4502/content/DocumentServices/CreatePDFG.html)以拖放檔案進行轉換。
1. 將Word檔案或jpg拖放至拖放區域。
1. 輸入檔案會轉換為PDF，並儲存在點4所指定的相同位置。

下列程式碼片段顯示如何使用PDFG服務將檔案轉換成PDF

```java
com.adobe.pdfg.service.api.GeneratePDFService pdfService = sling.getService(com.adobe.pdfg.service.api.GeneratePDFService.class);
System.out.println("Got PDF Service");
java.util.Map map = pdfService.createPDF(uploadedDocument,fileName,"","Standard","No Security", null, null);
```
