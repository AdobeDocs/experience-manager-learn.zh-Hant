---
title: 在HTM5表單提交上觸發AEM工作流程
seo-title: 在HTML5表單提交上觸發AEM工作流程
description: 在離線模式下繼續填寫行動表單，並提交行動表單以觸發AEM工作流程
seo-description: 在離線模式下繼續填寫行動表單，並提交行動表單以觸發AEM工作流程
feature: mobile-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: c56942831614b981684861ea78f1bd15f3bb1ab9
workflow-type: tm+mt
source-wordcount: '189'
ht-degree: 2%

---


# 審核和核准提交PDF的工作流程

最後一個也是最後一個步驟是建立AEM工作流程，以產生靜態或非互動的PDF供審核和核准。 工作流程將透過在節點`/content/pdfsubmissions`上設定的AEM啟動器觸發。

以下螢幕擷取顯示工作流程中涉及的步驟。

![工作流程](assets/workflow.PNG)

## 產生非互動式PDF工作流程步驟

此處指定了XDP模板和要與模板合併的資料。 要合併的資料是PDF中提交的資料。 此提交的資料儲存在節點`/content/pdfsubmissions`下。

![工作流程](assets/generate-pdf1.PNG)

產生的PDF會指派給稱為`submittedPDF`的工作流程變數。

![工作流程](assets/generate-pdf2.PNG)

### 指派產生的PDF供審閱和核准

指派工作流程元件會用在這裡，以指派產生的PDF供審核和核准。 變數`submittedPDF`用於「分配任務」工作流元件的「表單和文檔」頁籤。

![工作流程](assets/assign-task.PNG)
