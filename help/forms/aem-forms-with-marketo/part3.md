---
title: AEM Forms與Marketo （第3部分）
description: 教學課程使用AEM Forms表單資料模型將AEM Forms與Marketo整合。
feature: Adaptive Forms, Form Data Model
version: Experience Manager 6.4, Experience Manager 6.5
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="整合" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: 7096340b-8ccf-4f5e-b264-9157232e96ba
duration: 78
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '217'
ht-degree: 3%

---

# 建立表單資料模型

設定資料來源後，下一個步驟是根據先前步驟中設定的資料來源建立表單資料模型。 若要建立表單資料模型，請遵循下列步驟：

將瀏覽器指向[資料整合頁面。](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm)此清單列出在您的AEM執行個體上建立的所有資料整合。

1. 按一下建立 | 表單資料模型
1. 提供有意義的標題，例如FormsAndMarketo ，然後按「下一步」
1. 選取在先前步驟中設定的資料來源，然後按一下建立和編輯，在編輯模式中開啟表單資料模型
1. 展開「FormsAndMarketo」節點。 展開服務節點
1. 選取第一個「取得」作業
1. 按一下新增選取的專案
1. 在「新增關聯的模型物件」對話方塊中按一下「全選」，然後按一下「新增」
1. 按一下儲存按鈕以儲存您的表單資料模型
1. 定位至服務標籤
1. 選取列出的唯一服務，然後按一下測試服務
1. 提供有效的leadId，然後按一下「測試」。 如果一切順利，您應該要回頭取得銷售機會詳細資訊，如下方熒幕擷圖所示
   ![testresults](assets/testresults.png)

## 後續步驟

[彙總以進行測試](./part4.md)
