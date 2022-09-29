---
title: 可重複使用的AEM Forms工作流程模型
description: 了解如何建立不受適用性Forms影響的工作流程模型。
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: 3354a58b-d58e-4ddb-8f90-648554a64db8
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '285'
ht-degree: 0%

---

# 建立可重複使用的AEM Forms工作流程模型{#create-re-usable-aem-forms-workflow-models}

從AEM Forms 6.5版開始，我們現在可以建立不系結至特定適用性表單的工作流程模型。 透過此功能，您現在可以建立一個工作流程模型，以便在不同的最適化表單提交時叫用。 透過此功能，您可以擁有一般工作流程，處理所有最適化表單提交以供審核和核准。

要設計此類工作流，請執行以下步驟

1. 登入AEM
1. 將瀏覽器指向 [工作流模型](http://localhost:4502/libs/cq/workflow/admin/console/content/models.html)
1. 按一下 __建立>建立模型__ 添加工作流模型
1. 為工作流模型提供適當的名稱和標題，然後按一下完成
1. 在編輯模式下開啟新建立的模型
1. 將「分配任務」元件拖放到工作流模型中
1. 開啟分配任務元件的配置屬性
1. 索引標籤至Forms和檔案索引標籤
1. 選取「類型 — 適用性表單」或「唯讀適用性表單」。

有三種方式可指定表單路徑

1. 以絕對路徑提供 — 這表示工作流程與最適化表單緊密結合。 這不是我們想要的
1. **提交至工作流程**  — 這表示提交最適化表單時，工作流程引擎會從提交的資料中擷取表單名稱。 這是需要選取的選項
1. 可在變數中的路徑使用 — 這表示從工作流變數中提取最適化表單以下螢幕截圖顯示您需要選擇的正確選項，以從最適化表單中解除耦合工作流

![可重複使用的AEM Forms工作流程模型](assets/workflomodel.PNG)
