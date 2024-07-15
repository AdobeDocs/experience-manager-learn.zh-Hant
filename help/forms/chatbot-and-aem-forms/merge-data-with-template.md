---
title: 將資料與XDP範本合併
description: 將資料與範本合併，以建立PDF檔案
feature: Adaptive Forms
version: 6.5
jira: KT-15344
topic: Development
role: User
level: Intermediate
exl-id: 6a865402-db3d-4e0e-81a0-a15dace6b7ab
duration: 15
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '80'
ht-degree: 0%

---

# 將資料與XDP範本合併

下一步是將XML資料與範本合併，以產生PDF。 接著，系統會使用Adobe Sign傳送此PDF以供簽名。

## 使用OutputService產生PDF

已使用OutputService的[generatePDF](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/output/api/OutputService.html#generatePDFOutput-com.adobe.aemfd.docmanager.Document-com.adobe.aemfd.docmanager.Document-com.adobe.fd.output.api.PDFOutputOptions-)方法來產生PDF。
接著會使用Adobe Sign REST API傳送產生的PDF以供簽名。
