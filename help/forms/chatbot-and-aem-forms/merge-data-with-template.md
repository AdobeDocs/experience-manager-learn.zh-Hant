---
title: 將資料與XDP範本合併
description: 將資料與範本合併，以建立PDF檔案
feature: Adaptive Forms
version: 6.5
jira: KT-15344
topic: Development
role: User
level: Intermediate
source-git-commit: eb4463ae0270725c5b0bd96e9799bada25b06303
workflow-type: tm+mt
source-wordcount: '80'
ht-degree: 0%

---

# 將資料與XDP範本合併

下一步是將XML資料與範本合併，以產生PDF。 接著，系統會使用Adobe Sign傳送此PDF以供簽名。

## 使用OutputService產生PDF

此 [generatePDF](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/output/api/OutputService.html#generatePDFOutput-com.adobe.aemfd.docmanager.Document-com.adobe.aemfd.docmanager.Document-com.adobe.fd.output.api.PDFOutputOptions-) 已使用OutputService的方法產生PDF。
接著會使用Adobe Sign REST API傳送產生的PDF以供簽名。

