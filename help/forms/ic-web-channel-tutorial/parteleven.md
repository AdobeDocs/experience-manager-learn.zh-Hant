---
title: 配置投資組合面板
seo-title: Configuring Investment Mix Panel
description: 這是建立第一個互動式通信文檔的多步教程的第11部分。在本部分中，我們將添加餅圖以顯示當前和模型投資組合。
seo-description: This is part 11 of multistep tutorial for creating your first interactive communications document.In this part, we will add pie charts to display the current and model investment mix.
uuid: b0132912-cb6e-4dec-8309-5125d29ad291
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 77de4e76-53ee-467c-a71c-d1d3ea15283b
topic: Development
role: Developer
level: Beginner
exl-id: 774d7a6e-2b8f-4a70-98c5-e7712478ff75
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 0%

---

# 配置投資組合面板

在此部分，我們將添加餅圖以顯示當前和模型投資組合。

* 登錄AEM Forms並導航到Adobe Experience Manager>Forms>Forms和文檔。

* 開啟401KStatement資料夾。

* 在編輯模式下開啟401KStatement。

* 我們將添加2個餅圖，以表示客戶的當前和模型投資組合。

## 當前資產組合 {#current-asset-mix}

* 按一下右側的「CurrentAssetMix」面板，然後選擇「+」表徵圖並插入文本元件。 將預設文本更改為「當前資產組合」。

* 按一下「CurrentAssetMix」面板，選擇「+」表徵圖並插入圖表元件。 按一下新插入的圖表元件，然後按一下「扳手」表徵圖以開啟圖表的配置屬性表。

* 設定如下圖所示的屬性。 確保圖表類型為餅圖。

* 請注意綁定到X和Y軸的資料模型對象。 您需要選擇表單資料模型的根元素，然後細化以選擇相應的元素。

* ![當前資產組合](assets/currentassetmixchart.png)

## 模型資產組合 {#model-asset-mix}

* 按一下右側的「RecommendedAssetMix」面板，然後選擇「+」表徵圖並插入文本元件。 將預設文本更改為「模型資產組合」。

* 點擊「RecommendedAssetMix」面板，選擇「+」表徵圖並插入圖表元件。 按一下新插入的圖表元件，然後按一下「扳手」表徵圖以開啟圖表的配置屬性表。

* 設定如下圖所示的屬性。 確保圖表類型為餅圖。

* 請注意綁定到X和Y軸的資料模型對象。 您需要選擇表單資料模型的根元素，然後細化以選擇相應的元素。

* ![程式集類型](assets/modelassettypechart.png)

## 後續步驟

[準備提供Web渠道文檔](./parttwelve.md)
