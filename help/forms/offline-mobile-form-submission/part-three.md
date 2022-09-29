---
title: 在HTM5表單提交上觸發AEM工作流程 — 檢閱和核准PDF
seo-title: Trigger AEM Workflow on HTML5 Form Submission
description: 繼續以離線模式填寫行動表單，並提交行動表單以觸發AEM工作流程
seo-description: Continue filling mobile form in offline mode and submit mobile form to trigger AEM workflow
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: a767d8f8-d75e-4472-9139-c08d804ee076
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 2%

---

# 審核及核准提交PDF的工作流程

最後也是最終的步驟，是建立AEM工作流程，產生靜態或非互動式PDF以供審核和核准。 工作流程是透過在節點上設定的AEM啟動器觸發 `/content/pdfsubmissions`.

以下螢幕擷圖顯示工作流程中涉及的步驟。

![工作流程](assets/workflow.PNG)

## 產生非互動式PDF工作流程步驟

此處指定XDP範本以及要與範本合併的資料。 要合併的資料是來自PDF的提交資料。 此提交的資料儲存在節點下 `/content/pdfsubmissions`.

![工作流程](assets/generate-pdf1.PNG)

產生的PDF會指派給 `submittedPDF`.

![工作流程](assets/generate-pdf2.PNG)

### 指派產生的PDF以進行審核和核准

在此處分配任務工作流元件用於分配生成的PDF以進行複查和審批。 變數 `submittedPDF` 用於「分配任務」工作流元件的「Forms和文檔」頁簽中。

![工作流程](assets/assign-task.PNG)
