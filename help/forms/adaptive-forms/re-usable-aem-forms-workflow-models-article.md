---
title: 可重複使用的AEM Forms工作流程模型
description: 瞭解如何建立獨立於Adaptive Forms的工作流程模型。
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: 3354a58b-d58e-4ddb-8f90-648554a64db8
last-substantial-update: 2020-06-09T00:00:00Z
duration: 83
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 0%

---

# 建立可重複使用的AEM Forms工作流程模型{#create-re-usable-aem-forms-workflow-models}

從AEM Forms 6.5版開始，我們現在可以建立未與特定最適化表單繫結的工作流程模型。 透過這項功能，您現在可以建立一個工作流程模型，以便針對不同的最適化表單提交叫用。 有了這項功能，您就可以有通用工作流程來處理所有最適化表單提交以供檢閱和核准。

若要設計這類工作流程，請執行以下步驟

1. 登入AEM
1. 將瀏覽器指向 [工作流程模型](http://localhost:4502/libs/cq/workflow/admin/console/content/models.html)
1. 按一下 __「建立」>「建立模型」__ 新增工作流程模型的方式
1. 為工作流程模型提供適當的名稱和標題，然後按一下「完成」
1. 在編輯模式中開啟新建立的模型
1. 將指派任務元件拖放至您的工作流程模型上
1. 開啟指派工作元件的設定屬性
1. 按Tab鍵前往Forms和檔案標籤
1. 選擇型別 — 最適化表單或唯讀最適化表單。

指定表單路徑的方式有三種

1. 絕對路徑可用 — 這表示工作流程與調適型表單緊密結合。 這不是我們想要的
1. **已提交至工作流程**  — 這表示在提交最適化表單時，工作流程引擎會從提交的資料中擷取表單名稱。 這是需要選取的選項
1. 可於變數中的路徑取得 — 這表示會從工作流程變數中擷取調適型表單。下列熒幕擷圖顯示您需要為從調適型表單取消耦合工作流程選擇的正確選項

![可重複使用的AEM Forms工作流程模型](assets/workflomodel.PNG)
