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

# 建立初始窗體

初始表單（再融資表單）用於通過觸發 **簽署多個Forms** 工作AEM流。 您可以輸入所選的值，但確保將以下欄位添加到表單中。

| 欄位類型 | 名稱 | 用途 | 隱藏 | 預設值 |
| ------------------------|---------------------------------------|--------------------|--------|----------------- |
| 文本欄位 | 簽名 | 指示簽名狀態 | Y | N |
| 文本欄位 | GUID | 唯一標識窗體 | Y | 3889 |
| 文本欄位 | customerName | 捕獲客戶名稱 | N |
| 文本欄位 | 客戶電子郵件 | 要發送通知的客戶電子郵件 | N |
| 複選框 | 表單到符號 | 這些項標識包中的表單 | N |

需要配置初始表單以觸發名為AEM的工作流 **符號多樣式**
確保資料檔案路徑設定為 **資料.xml**。 這非常重要，因為示例代碼在表單提交過程中查找名為Data.xml的檔案。

## Assets

初始表單（再融資表單）可以 [從此處下載](assets/refinance-form.zip)

## 後續步驟

[建立用於簽名的表單](./create-forms-for-signing.md)
