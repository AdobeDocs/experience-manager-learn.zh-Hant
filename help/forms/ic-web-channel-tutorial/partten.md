---
title: 配置報廢展望面板
seo-title: Configuring Retirement Outlook Panel
description: 這是建立第一個互動式通訊檔案的多步驟教學課程的10部分。 在本部分中，我們將通過添加文本和圖表元件來配置Retimution Outlook面板。
seo-description: This is part 10 of a multi-step tutorial for creating your first interactive communications document. In this part, we will configure Retirement Outlook Panel by adding text and chart components.
uuid: 1d5119b5-e797-4bf0-9b10-995b3f051f92
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 2ee2cea2-aefa-4d21-a258-248648f73a68
topic: Development
role: Developer
level: Beginner
exl-id: 0dd8a430-9a4e-4dc7-ad75-6ad2490430f2
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '349'
ht-degree: 1%

---

# 配置報廢展望面板{#configuring-retirement-outlook-panel}

* 這是建立第一個互動式通訊檔案的多步驟教學課程的10部分。 在本部分中，我們將通過添加文本和圖表元件來配置Retimution Outlook面板。

* 登入AEM Forms並導覽至「Adobe Experience Manager > Forms > Forms與檔案」。

* 開啟401KStatement資料夾。

* 在編輯模式下開啟401KStatement文檔。

**配置左面板目標區域**

* 點選右側的「左面板」目標區域，然後按一下「+」圖示以開啟「插入元件」對話方塊。

* 插入文本元件。

* 輕輕點選新增的文字元件，以開啟元件工具列

* 選取「鉛筆」圖示以編輯預設文字。

* 將預設文字取代為「**你的退休收入前景」**

**配置RightPanel目標區域**

* 點選右側的「RightPanel」目標區域，然後按一下「+」圖示以開啟「插入元件」對話方塊。

* 插入文本元件。

* 輕輕點選新增的文字元件，以開啟元件工具列。

* 選取「鉛筆」圖示以編輯預設文字。

* 將預設文字取代為「**月退休收入估計」**

## 添加退休收入展望文檔片段 {#add-retirement-income-outlook-document-fragment}

* 按一下「資產」圖示，然後套用篩選器以顯示「檔案片段」類型的資產。 將RetirementIncomeOutlook文檔片段拖放到「左面板」目標區域。

* 您可以參考 [到本頁](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/partseven.html) 新增檔案片段至內容區域時。

## 添加估計月收入表 {#adding-estimated-monthly-income-chart}

* 按一下右側的「RightPanel」目標區域。 按一下「+」圖示以插入圖表元件。 我們將用一個柱圖來顯示月收入估計值。 輕輕點選新插入的圖表元件。 選擇「扳手」表徵圖以開啟「配置屬性」工作表。使用以下螢幕截圖所示的屬性配置圖表。

**AEM Forms 6.4 — 設定預計月收入欄圖**

![form64](assets/estimatedmonthlyincomechart.png)

**AEM Forms 6.5 — 設定預計月收入欄圖**

![forms65](assets/estimatedmonthlyincomechart65.PNG)

## 後續步驟

[配置餅圖](./parteleven.md)
