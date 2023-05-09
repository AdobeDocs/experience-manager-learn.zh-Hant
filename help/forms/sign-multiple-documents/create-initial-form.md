---
title: 建立初始表單以觸發流程
description: 建立初始表單以觸發電子郵件通知以啟動簽名過程。
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: User
level: Intermediate
kt: 6892
thumbnail: 6892.jpg
exl-id: d7c55dc8-d886-4629-bb50-d927308d12e3
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '177'
ht-degree: 5%

---

# 建立初始表單

初始表單（再融資表單）用於通過觸發 **簽署多個Forms** AEM工作流程。 您可以輸入您選擇的值，但請確定已將下列欄位新增至表單中。

| 欄位類型 | 名稱 | 用途 | 隱藏 | 預設值 |
| ------------------------|---------------------------------------|--------------------|--------|----------------- |
| TextField | 簽名 | 指示簽名狀態 | Y | N |
| TextField | guid | 唯一識別表單 | Y | 3889 |
| TextField | customerName | 擷取客戶名稱 | N |
| TextField | customerEmail | 要發送通知的客戶電子郵件 | N |
| CheckBox | formsToSign | 項目會識別套件中的表單 | N |

需要設定初始表單，以觸發名為的AEM工作流程 **多重表單**
確認資料檔案路徑已設為 **Data.xml**. 當范常式式碼在表單提交程式的裝載中尋找名為Data.xml的檔案時，這是很重要的。

## Assets

初始表單（再融資表單）可以是 [從此處下載](assets/refinance-form.zip)

## 後續步驟

[建立要用於簽名的表單](./create-forms-for-signing.md)
