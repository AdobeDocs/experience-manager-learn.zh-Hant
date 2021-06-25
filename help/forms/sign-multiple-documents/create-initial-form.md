---
title: 建立初始表單以觸發流程
description: 建立初始表單以觸發電子郵件通知以啟動簽名過程。
feature: 適用性表單
version: 6.4,6.5
topic: 開發
role: Business Practitioner
level: Intermediate
kt: 6892
thumbnail: 6892.jpg
source-git-commit: 3569d8b2a38d1cce02f6f4ff8b0c583f4dc118b6
workflow-type: tm+mt
source-wordcount: '171'
ht-degree: 5%

---


# 建立初始表單

初始表單（再融資表單）用於通過觸發&#x200B;**Sign Multiple Forms** AEM工作流來簽署多個表單。 您可以輸入您選擇的值，但請確定已將下列欄位新增至表單中。

| 欄位類型 | 名稱 | 用途 | 隱藏 | 預設值 |
| ------------------------|---------------------------------------|--------------------|--------|----------------- |
| TextField | 簽名 | 指示簽名狀態 | Y | N |
| TextField | guid | 唯一識別表單 | Y | 3889 |
| TextField | customerName | 擷取客戶名稱 | N |
| TextField | customerEmail | 要發送通知的客戶電子郵件 | N |
| CheckBox | formsToSign | 項目會識別套件中的表單 | N |

需要設定初始表單，以觸發名為&#x200B;**signmultipleforms**的AEM工作流程
請確定「資料檔案路徑」已設為**Data.xml**。 當范常式式碼在表單提交程式的裝載中尋找名為Data.xml的檔案時，這是很重要的。

## 資產

初始表單（再融資表單）可從此處下載[](assets/refinance-form.zip)





