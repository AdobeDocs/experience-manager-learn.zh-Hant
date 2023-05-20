---
title: 建立用於簽名的Forms
description: 建立需要包含在簽名包中的表單。
feature: Adaptive Forms
version: 6.4,6.5
kt: 6893
thumbnail: 6893.jpg
topic: Development
role: User
level: Beginner
exl-id: 565d81a4-2918-44ea-a3e9-ed79f246f08a
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 0%

---

# 建立用於簽名的表單

下一步是建立要包含在包中的自適應表單。 在建立簽名表單時，請記住以下幾點：

* 確保表單基於 **SignMultipleForms** 的下界。 這確保表單預先填充了從資料庫獲取的資料。

* 需要將表單配置為使用Acrobat Sign，並且簽名者1欄位需要與「客戶電子郵件」欄位關聯
* 表單還需要與調用的clientLib關聯 **getnexform**
* 表單需要使用「簽名步驟」元件。
* 窗體還必須使用自定義 **對多個表單簽名** 元件。 此元件允許您導航到要登錄包的下一個表單。
* 必須將表單的提交配置為觸發工作AEM流 **更新簽名狀態**
* 確保資料檔案路徑設定為 **資料.xml**。 這非常重要，因為示例代碼在表單提交過程中查找名為Data.xml的檔案。

建立表單後，包括 **公用欄位** 自適應格式片段。 片段被標籤為隱藏。 此片段包含以下欄位。

* **簽名**  — 用於保存簽名狀態的欄位
* **GUID**  — 標識包中表單的唯一標識符
* **客戶電子郵件**  — 此欄位包含客戶的電子郵件



>[!NOTE]
>如果要將資料從一個表單傳輸到包中的另一個表單，請確保在所有表單中對表單域的命名相同。

## 全部完成表單

一旦包中的所有表單都被填寫和簽名，我們需要顯示相應的消息。 此消息在Alldone窗體的幫助下顯示。 Alldone窗體包含在示例窗體中。

## Assets

包括本教程中使用的示例表單可以 [從此處下載](assets/forms-for-signing.zip)

## 後續步驟

[Test本地系統上的解決方案](./testing-and-trouble-shooting.md)
