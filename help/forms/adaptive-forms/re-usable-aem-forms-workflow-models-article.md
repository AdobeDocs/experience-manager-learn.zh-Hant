---
title: 可重用的AEM Forms工作流模型
description: 瞭解如何建立獨立於自適應Forms的工作流模型。
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: 3354a58b-d58e-4ddb-8f90-648554a64db8
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '285'
ht-degree: 0%

---

# 建立可重用的AEM Forms工作流模型{#create-re-usable-aem-forms-workflow-models}

從AEM Forms6.5版開始，我們現在可以建立不與特定自適應表單綁定的工作流模型。 利用此功能，現在可以建立一個可以在不同自適應表單提交中調用的工作流模型。 使用此功能，您可以擁有一個通用工作流來處理所有自適應表單提交以供審閱和批准。

要設計此工作流，請執行以下步驟

1. 登錄到AEM
1. 將瀏覽器指向 [工作流模型](http://localhost:4502/libs/cq/workflow/admin/console/content/models.html)
1. 按一下 __建立>建立模型__ 添加工作流模型
1. 為工作流模型提供相應的名稱和標題，然後按一下「完成」(Done)
1. 在編輯模式下開啟新建立的模型
1. 將「分配任務」元件拖放到工作流模型
1. 開啟分配任務元件的配置屬性
1. 「Forms和文檔」頁籤
1. 選擇「類型」 — 「自適應表單」或「只讀自適應表單」。

有三種方式可指定表單路徑

1. 絕對路徑可用 — 這意味著工作流與自適應表單緊密耦合。 這不是我們想要的
1. **已提交到工作流**  — 這意味著當自適應表單被提交時，工作流引擎會從提交的資料中提取表單的名稱。 這是需要選擇的選項
1. 在變數中的路徑上可用 — 這意味著從工作流變數中選取了自適應表單以下螢幕快照顯示了從自適應表單中取消耦合工作流所需的正確選項

![可重用的AEM Forms工作流模型](assets/workflomodel.PNG)
