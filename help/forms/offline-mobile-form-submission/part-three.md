---
title: 在HTM5表單提交上觸發AEM工作流程
seo-title: 在HTML5表單提交上觸發AEM工作流程
description: 繼續以離線模式填寫行動表單，並提交行動表單以觸發AEM工作流程
seo-description: 繼續以離線模式填寫行動表單，並提交行動表單以觸發AEM工作流程
feature: 行動表單
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: 開發
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '192'
ht-degree: 3%

---


# 審核和批准提交的PDF的工作流

最後也是最後一步，是建立AEM工作流程，以產生靜態或非互動式PDF以供審核和核准。 工作流程將透過在節點`/content/pdfsubmissions`上設定的AEM啟動器觸發。

以下螢幕擷圖顯示工作流程中涉及的步驟。

![工作流程](assets/workflow.PNG)

## 產生非互動式PDF工作流程步驟

此處指定XDP範本以及要與範本合併的資料。 要合併的資料是PDF中提交的資料。 此提交的資料儲存在節點`/content/pdfsubmissions`下。

![工作流程](assets/generate-pdf1.PNG)

產生的PDF會指派給名為`submittedPDF`的工作流程變數。

![工作流程](assets/generate-pdf2.PNG)

### 指派產生的PDF以進行審核和核准

在此處分配任務工作流元件用於分配生成的PDF以進行審核和批准。 變數`submittedPDF`用於「分配任務」工作流元件的「Forms和文檔」頁簽中。

![工作流程](assets/assign-task.PNG)
