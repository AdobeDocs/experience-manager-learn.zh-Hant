---
title: 建立可重複使用的AEM Forms工作流程模型。
seo-title: 建立可重複使用的AEM Forms工作流程模型。
description: 獨立於自適應Forms的工作流模型。
seo-description: 獨立於自適應Forms的工作流模型。
feature: 工作流程
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.5
uuid: 3a082743-3e56-42f4-a44b-24fa34165926
discoiquuid: 9f18c314-39d1-4c82-b1bc-d905ea472451
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '291'
ht-degree: 0%

---


# 建立可重複使用的AEM Forms工作流模型{#create-re-usable-aem-forms-workflow-models}

從AEM Forms6.5版開始，我們現在可以建立未系結至特定最適化表單的工作流程模型。 有了這項功能，您現在可以建立一個工作流程模型，以便在不同的調適性表單提交時呼叫。 有了這項功能，您就可以擁有一般工作流程來處理所有調適性表單提交，以供審核和核准。

若要設計此類工作流程，請執行下列步驟

1. 登入AEM至
1. 將瀏覽器指向[工作流程模型](http://localhost:4502/libs/cq/workflow/admin/console/content/models.html)
1. 按一下「建立」 |建立模型以新增工作流程模型
1. 為工作流模型提供適當的名稱和標題，然後按一下「完成」(Done)
1. 在編輯模式下開啟新建立的模型
1. 將Assign Task元件拖放到您的工作流模型上
1. 開啟Assign Task元件的配置屬性
1. 「Forms」和「文檔」頁籤
1. 選擇「類型」(Type)-「最適化表單」(Adaptive Form)或「只讀」(Read-Only Adaptive Form)。

有3種方式可指定表單路徑

1. 以絕對路徑提供——這表示工作流程將與最適化表單緊密結合。 這不是我們想要的
1. **提交至工作流** -這表示提交最適化表單時，工作流引擎將從提交的資料擷取表單名稱。這是需要選取的選項
1. 可在變數中的路徑使用——這表示將從工作流程變數中挑選最適化表單
下列螢幕擷取畫面顯示您需要選擇正確的選項，以從最適化表單中解除耦合工作流程

![工作流模型](assets/workflomodel.PNG)