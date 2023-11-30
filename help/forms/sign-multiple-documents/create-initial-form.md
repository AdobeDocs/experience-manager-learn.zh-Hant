---
title: 建立初始表單以觸發程式
description: 建立初始表單以觸發電子郵件通知以開始簽名流程。
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: User
level: Intermediate
jira: KT-6892
thumbnail: 6892.jpg
exl-id: d7c55dc8-d886-4629-bb50-d927308d12e3
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '177'
ht-degree: 5%

---

# 建立初始表單

初始表單（再融資表單）是用來簽署多個表單，其方式為觸發 **簽署多個Forms** AEM工作流程。 您可以輸入您選擇的值，但請確定表單中已新增下列欄位。

| 欄位型別 | 名稱 | 用途 | 隱藏 | 預設值 |
| ------------------------|---------------------------------------|--------------------|--------|----------------- |
| 文字欄位 | 已簽署 | 表示簽署狀態 | Y | N |
| 文字欄位 | guid | 以唯一識別表單 | Y | 3889 |
| 文字欄位 | customername | 若要擷取客戶名稱 | N |
| 文字欄位 | 客戶電子郵件 | 要傳送通知的客戶電子郵件 | N |
| 核取方塊 | formsToSign | 專案會識別封裝中的表單 | N |

初始表單需要設定以觸發名為的AEM工作流程 **signmultipleforms**
確定資料檔案路徑已設為 **Data.xml**. 這非常重要，因為範常式式碼會在處理表單提交的程式中，於裝載中尋找名為Data.xml的檔案。

## Assets

初始表單（再融資表單）可以是 [已從此處下載](assets/refinance-form.zip)

## 後續步驟

[建立用於簽署的表單](./create-forms-for-signing.md)
