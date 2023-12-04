---
title: 建立Forms以供簽署
description: 建立需要包含在簽署封裝中的表單。
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-6893
thumbnail: 6893.jpg
topic: Development
role: User
level: Beginner
exl-id: 565d81a4-2918-44ea-a3e9-ed79f246f08a
duration: 98
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 0%

---

# 建立表單以供簽署

下一步是建立您要包含在封裝中的調適型表單。 建立表單以供簽署時，請記得遵循下列要點：

* 請確定表單是根據 **SignMultipleForms** 範本。 這可確保表單已預先填入從資料庫擷取的資料。

* 表單必須設定為使用Acrobat Sign，且簽署者1欄位必須與客戶電子郵件欄位相關聯
* 表單也需與以下專案建立關聯： **getnextform**
* 表單需要使用簽章步驟元件。
* 表單還必須使用自訂 **簽署多個表單** 元件。 此元件可讓您導覽至下一個表單，以登入封裝。
* 表單提交必須設定為觸發AEM工作流程 **更新簽章狀態**
* 確定資料檔案路徑已設為 **Data.xml**. 這非常重要，因為範常式式碼會在處理表單提交的程式中，於裝載中尋找名為Data.xml的檔案。

一旦您完成表單的撰寫後，請將 **共用欄位** 表單中的自適應表單片段。 片段會標示為隱藏。 此片段包含下列欄位。

* **已簽署**  — 儲存簽名狀態的欄位
* **guid**  — 用於識別封裝中表單的唯一識別碼
* **客戶電子郵件**  — 此欄位會保留客戶的電子郵件



>[!NOTE]
>如果您想要將資料從一個表單傳送到封裝中的另一個表單，請確定表單欄位的名稱在所有表單中都相同。

## 全部完成的表單

填妥及簽署封裝中的所有表單後，我們需要顯示適當的訊息。 此訊息會在Alldone表單的協助下顯示。 Alldone表單包含在範例表單中。

## Assets

範例表單包含本教學課程中使用的可以 [已從此處下載](assets/forms-for-signing.zip)

## 後續步驟

[在本機系統上測試解決方案](./testing-and-trouble-shooting.md)
