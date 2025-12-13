---
title: 建立初始表單以觸發程式
description: 建立初始表單以觸發電子郵件通知以開始簽名流程。
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: User
level: Intermediate
jira: KT-6892
thumbnail: 6892.jpg
exl-id: d7c55dc8-d886-4629-bb50-d927308d12e3
duration: 35
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '177'
ht-degree: 9%

---

# 建立初始表單

初始表單（再融資表單）是用來簽署多個表單，方法是觸發&#x200B;**簽署多個Forms** AEM工作流程。 您可以輸入您選擇的值，但請確定表單中已新增下列欄位。

| 欄位類型 | 名稱 | 用途 | 隱藏 | 預設值 |
|--- |--- |---|--- |--- |
| 文字欄位 | 已簽署 | 表示簽署狀態 | Y | N |
| 文字欄位 | guid | 以唯一識別表單 | Y | 3889 |
| 文字欄位 | customername | 若要擷取客戶名稱 | N | |
| 文字欄位 | 客戶電子郵件 | 要傳送通知的客戶電子郵件 | N | |
| 核取方塊 | formsToSign | 專案會識別封裝中的表單 | N | |

初始表單必須設定為觸發名為&#x200B;**signmultipleforms**的AEM工作流程
確定資料檔案路徑已設定為**Data.xml**。 這非常重要，因為範常式式碼會在處理表單提交的程式中，於裝載中尋找名為Data.xml的檔案。

## Assets

初始表單（再融資表單）可從這裡[下載](assets/refinance-form.zip)

## 後續步驟

[建立用於簽署的表單](./create-forms-for-signing.md)
