---
title: 在HTML5表單提交時觸發AEM工作流程 — 檢閱並核准PDF
description: 稽核已提交PDF的工作流程
feature: Mobile Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-16133
exl-id: a767d8f8-d75e-4472-9139-c08d804ee076
duration: 32
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '169'
ht-degree: 1%

---

# 用於檢閱和核准已提交PDF的工作流程

最後一個也是最後一個步驟是建立AEM工作流程，這會產生靜態或非互動式PDF以供檢閱和核准。 工作流程是透過節點`/content/formsubmissions`上設定的AEM Launcher觸發。

以下熒幕擷圖顯示工作流程中的步驟。

![工作流程](assets/workflow.PNG)

## 產生非互動式PDF工作流程步驟

此處會指定XDP範本及要與範本合併的資料。 要合併的資料是從PDF提交的資料。 此提交的資料儲存在節點```/content/formsubmissions```下

![工作流程](assets/generate-pdf1.PNG)

產生的PDF已指派給名為`submittedPDF`的工作流程變數。

![工作流程](assets/generate-pdf2.PNG)

### 指派產生的PDF以供檢閱和核准

指派任務工作流程元件是用來指派產生的PDF以供檢閱和核准。 變數`submittedPDF`用於指派工作流程元件的Forms和檔案索引標籤。

![工作流程](assets/assign-task.PNG)


## 後續步驟

[在您的環境中部署資產](./deploy-assets.md)