---
title: 將最適化表單資料儲存在Azure儲存體
description: 建立並設定最適化表單，以將資料儲存在Azure儲存體中
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
thumbnail: 335423.jpg
source-git-commit: 52c8d96a03b4d6e4f2a0a3c92f4307203e236417
workflow-type: tm+mt
source-wordcount: '286'
ht-degree: 0%

---

# 整合所有內容

我們現在已具備使用案例所需的所有必要設定/整合。 最後一步是根據Azure儲存體支援的表單資料模型建立最適化表單。

建立及最適化表單，並確定其基礎是正確的最適化表單範本。 這將確保我們與範本相關聯的自訂程式碼將在每次呈現最適化表單時執行。

## 最適化表單範例

我們在表單中新增了2個隱藏欄位

* Blob ID — 初始化欄位時，會以GUID填入此欄位。 此欄位的值會用作blobid，以唯一識別表單資料的blob儲存。此blobid用於識別表單資料。
* 傳回Blob ID — 此欄位會填入服務呼叫的結果，以將資料儲存在Azure儲存體中。 此值將與上一步驟的Blob ID相同。

表單具有下列商業規則

* 使用者輸入電子郵件地址時，儲存表單按鈕便會顯示。 按一下「儲存表單」按鈕時，表單資料會使用表單資料模型的叫用服務操作儲存在Azure儲存體中。
* 叫用服務傳回的BlobID會儲存在Blob ID欄位中。 當此值變更時，會使用SendGrid將電子郵件傳送給應徵者。 電子郵件將包含連結，以開啟Blob ID所識別的部分完成表單。
* 當資料成功儲存在Azure儲存體時，會向使用者顯示確認文字

### 後續步驟

[部署範例資產](./deploy-sample-assets.md)

