---
title: PDFG在AEM Forms的應用
description: 演示拖放功能以使用AEM Forms建立PDF
feature: PDF Generator
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: bc79fcbf-b8b3-4d3a-9cd6-0bcd9321c7d5
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 2%

---

# PDFG在AEM Forms的應用{#using-pdfg-in-aem-forms}

演示拖放功能以使用AEM Forms建立PDF

PDFG代表PDF生成。 這意味著您可以將各種檔案格式轉換為PDF。 最常見的是Microsoft辦公室的檔案。 PDFG公司自6.1歲起一直是AEM Forms的一部分。
[此處列出了PDFG API的Javadoc](https://www.adobe.io/experience-manager/reference-materials/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html)

與此文章關聯的資產將允許您將MS office文檔或JPG檔案拖放到HTML頁的放置區域。 文檔一旦被刪除，就調用PDFG服務，將文檔轉換為PDF，並保存在伺服器的檔案系AEM統中。

要安裝演示資產，請執行以下步驟

1. 配置本文檔中提到的PDFG [這裡](https://helpx.adobe.com/tw/experience-manager/6-4/forms/using/install-configure-pdf-generator.html)。
1. 請按照與您的AEM Forms版本相關的相應文檔進行操作。
1. [使用包管理器導入和安裝與本文相關的資產。](assets/createpdfgdemov2.zip)
1. [導航到post.jsp](http://localhost:4502/apps/AemFormsSamples/components/createPDF/POST.jsp) 在CRX中
1. 根據您的首選項更改保存位置（第9行）
1. 保存更改。
1. 開啟 [html頁面](http://localhost:4502/content/DocumentServices/CreatePDFG.html) 用於拖放要轉換的檔案。
1. 將單詞檔案或jpg放入放置區域。
1. 將輸入文檔轉換為PDF，並保存在點4中指定的同一位置。

以下代碼段顯示PDFG服務將檔案轉換為PDF的用法

```java
com.adobe.pdfg.service.api.GeneratePDFService pdfService = sling.getService(com.adobe.pdfg.service.api.GeneratePDFService.class);
System.out.println("Got PDF Service");
java.util.Map map = pdfService.createPDF(uploadedDocument,fileName,"","Standard","No Security", null, null);
```
