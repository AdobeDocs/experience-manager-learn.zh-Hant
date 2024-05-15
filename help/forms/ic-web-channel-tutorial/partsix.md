---
title: 建立Web Channel的互動式通訊
description: 這是建立第一個互動式通訊檔案的多步驟教學課程的第6部分。 在本節中，我們將建立Web Channel的互動式通訊。
discoiquuid: b44ff855-9ead-471e-8f0f-b562b88a5337
feature: Interactive Communication
doc-type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: a0a0c8dc-5302-446c-9fec-e23fe1320e34
duration: 33
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '179'
ht-degree: 1%

---

# 建立Web Channel的互動式通訊

在本節中，我們將建立Web Channel的互動式通訊。

1. 登入AEM編寫執行個體，並導覽至「Adobe Experience Manager > Forms > Forms &amp; Documents」。
1. 開啟401KStatment資料夾。
1. 點選「建立」並選取「互動式通訊」。 會顯示「建立互動式通訊」頁面。
1. 輸入下列資訊

   1. 標題：401KStatement
   1. 說明：適用於個別參與者的401KStatement
   1. 表單資料模型：RetirationAccountStatement
   1. 預填服務：表單資料模型預填服務

1. 點選「下一步」
1. 指定下列專案

   1. 取消勾選「列印管道」核取方塊。 我們不會針對列印管道建立檔案。
   1. Web：選取此選項可產生Web管道的檔案
   1. 互動式通訊：範本： **global>RetirationAccountStatemen** t（這是先前步驟建立的範本）
   1. 佈景主題：**參考佈景主題 — >畫布2.0**

1. 點選「建立」
1. 您可以按一下「完成」或「編輯」來關閉對話方塊。

## 後續步驟

[新增文字和影像至檔案](./partseven.md)
