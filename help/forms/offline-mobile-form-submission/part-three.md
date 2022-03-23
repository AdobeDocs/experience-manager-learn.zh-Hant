---
title: 觸AEM發HTM5表單提交工作流 — 審核和批准PDF
seo-title: Trigger AEM Workflow on HTML5 Form Submission
description: 以離線模式繼續填寫移動表單並提交移動表單以觸發工AEM作流
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
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# 要審閱和批准提交的PDF的工作流

最後一個也是最後一個步驟是創AEM建工作流，該工作流將生成靜態或非互動式PDF以供審核和批准。 將通過節點上配置的啟AEM動程式觸發工作流 `/content/pdfsubmissions`。

以下螢幕快照顯示了工作流中涉及的步驟。

![工作流程](assets/workflow.PNG)

## 生成非互動式PDF工作流步驟

此處指定了XDP模板和要與模板合併的資料。 要合併的資料是來自PDF的提交資料。 此提交的資料儲存在節點下 `/content/pdfsubmissions`。

![工作流程](assets/generate-pdf1.PNG)

生成的PDF被分配給稱為 `submittedPDF`。

![工作流程](assets/generate-pdf2.PNG)

### 分配生成的PDF以供審閱和審批

此處使用分配任務工作流元件來分配生成的PDF以供審核和審批。 變數 `submittedPDF` 在「分配任務」工作流元件的「Forms和文檔」頁籤中使用。

![工作流程](assets/assign-task.PNG)
