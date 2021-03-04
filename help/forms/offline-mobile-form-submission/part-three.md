---
title: HTM5表AEM單提交的觸發工作流程
seo-title: HTML5表AEM單提交的觸發工作流程
description: 在離線模式下繼續填寫行動表單，並提交行動表單以觸發工AEM作流程
seo-description: 在離線模式下繼續填寫行動表單，並提交行動表單以觸發工AEM作流程
feature: 行動表單
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: 開發
role: 開發人員
level: 經驗豐富
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '194'
ht-degree: 4%

---


# 審核和核准提交PDF的工作流程

最後一個也是最後一個步驟，AEM是建立工作流程，以產生靜態或非互動的PDF供審核和核准。 工作流將通過在節點AEM`/content/pdfsubmissions`上配置的啟動程式觸發。

以下螢幕擷取顯示工作流程中涉及的步驟。

![工作流程](assets/workflow.PNG)

## 產生非互動式PDF工作流程步驟

此處指定了XDP模板和要與模板合併的資料。 要合併的資料是PDF中提交的資料。 此提交的資料儲存在節點`/content/pdfsubmissions`下。

![工作流程](assets/generate-pdf1.PNG)

產生的PDF會指派給稱為`submittedPDF`的工作流程變數。

![工作流程](assets/generate-pdf2.PNG)

### 指派產生的PDF供審閱和核准

指派工作流程元件會用在這裡，以指派產生的PDF供審核和核准。 變數`submittedPDF`用於「指派任務」工作流元件的「Forms和文檔」頁籤。

![工作流程](assets/assign-task.PNG)
