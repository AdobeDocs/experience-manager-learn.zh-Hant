---
title: AEM Forms
seo-title: Merge Adaptive Form data with Acroform
description: Acroforms與AEM Forms整合的第二部分。 從Acroform建立架構。
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '177'
ht-degree: 0%

---


# 從頂框建立架構

下一步是從前面步驟中建立的頂層窗體建立架構。 提供了一個示例應用程式，用於作為本教程的一部分建立模式。 要建立架構，請按照以下說明操作：

1. 登錄到 [CRXDE Lite](http://localhost:4502/crx/de)
2. 開啟到檔案 `/apps/AemFormsSamples/components/createxsd/POST.jsp`
3. 更改 `saveLocation` 到硬碟上相應的資料夾。 確保已建立要保存到的資料夾。
4. 將瀏覽器指向 [建立XSD](http://localhost:4502/content/DocumentServices/CreateXsd.html) 上承載的頁AEM面。
5. 拖放頂圖。
6. 檢查步驟3中指定的資料夾。 架構檔案將保存到此位置。

## 上載頂層

要使本演示在您的系統上工作，您需要建立一個名為 `acroforms` 在AEM Assets。 將Acroform上載到此 `acroforms` 的子菜單。

>[!NOTE]
>
>示例代碼將查找此資料夾中的頂框。 要合併自適應表單的已提交資料，需要頂蓋。
