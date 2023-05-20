---
title: 影像、色板、旋轉和混合媒體集
description: Dynamic Media Classic最有用、最強大的能力之一是支援建立像Image、Swatch、Spin和Mixed Media Sets這樣的富媒體集。 瞭解每個富媒體集是什麼以及如何在Dynamic Media Classic建立每種類型。 然後瞭解有關「批集預設」的更多資訊，該預設可自動執行上載時建立富媒體集的過程。
feature: Dynamic Media Classic, Image Sets, Mixed Media Sets, Spin Sets
topic: Content Management
role: User
level: Beginner
exl-id: 45c86ff2-d991-46a7-a8d1-25c9fec142d9
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '1433'
ht-degree: 1%

---

# 影像、色板、旋轉和混合媒體集 {#media-sets}

Dynamic Media Classic的集合可以超越單幅影像，實現動態調整和縮放，從而提供更豐富的線上體驗。 本教程的本節將探討如何在Dynamic Media Classic建立以下富媒體集：

- 影像集
- 色板集
- 迴轉集
- 混合媒體集

還將說明如何使用「批集預設」通過上載自動建立集。

## 你一直想知道的關於場景的一切

除了基本的動態調整和縮放，集合可能是應用最廣泛的Dynamic Media Classic子產品。 集實質上是「虛擬」資產，它不包含任何實際影像，但是由一組與其他影像和/或視頻的關係組成。 集合最吸引人的地方是，它們是可以「現成」的微型應用。 也就是說，每個設定查看器都包含其自己的邏輯和介面，因此您只需在站點上呼叫它們即可。 此外，它們只要求您跟蹤每個集的單個資產ID，而不必親自管理所有成員資產和關係。

在建立集時，該集將作為單獨的資產進行管理，在從URL提供服務之前，必須將其標籤為發佈和發佈。 其所有成員資產也必須公佈。

### 集類型

讓我們瞭解您可以在Dynamic Media Classic建立的四種類型集：影像、色板、旋轉和混合媒體集。

## 影像集

這是最常見的集類型。 通常，您會將其用於同一項目的替代視圖。 它包含通過按一下該影像的關聯縮覽圖載入到查看器中的多個影像。

![影像](assets/media-sets/image-set-1.jpg)

_影像集示例_

上述影像集的URL可能顯示為：

![影像](assets/media-sets/image-set-url-1.png)

- 瞭解有關使用 [快速啟動映像集](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sets/quick-start-image-sets.html)。
- 瞭解如何 [建立映像集](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sets/creating-image-set.html#creating-an-image-set)。

### 色板集

此類型的集通常用於顯示同一項目的彩色視圖。 它由成對的影像和顏色色板組成。

「色板」和「影像集」之間的主要區別是，「色板集」使用不同的影像作為可點擊的色板，而「影像集」使用原始影像的可點擊的縮覽圖版本。

色板集不著色影像（常見的誤解）。 只是換用影像，與在「影像集」中完全相同。 迷你色板影像可以是使用Photoshop創作的，每種顏色可以單獨拍攝，或者Dynamic Media Classic的裁剪工具可以用來從彩色影像中製作一個色板。

![影像](assets/media-sets/image-set-2.jpg)

_樣本集示例_

上述色板集的URL可能顯示為：

![影像](assets/media-sets/image-set_url.png)

- 瞭解有關使用 [快速開始色板集](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/swatch-sets/quick-start-swatch-sets.html)。
- 瞭解如何 [建立色板集](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/swatch-sets/creating-swatch-set.html#creating-a-swatch-set)。

### 迴轉集

此集通常用於顯示項目的360度視圖。 與斯沃琪集一樣，旋轉集也不使用3D魔術 — 真正的工作是從各個方面建立一幅影像的許多照片。 觀看者只是允許您在影像之間切換，如停止動畫。

「旋轉集」(Spin Sets)可以沿單個軸沿一個方向旋轉，或者如果交替建立為2D旋轉集(Spin Sets) — 在多個軸上旋轉。 例如，汽車可以在所有車輪都在地面時進行旋轉，然後也可以在汽車後輪上「翻轉」並旋轉。 對於正確設定的2D旋轉集，每個軸的每行影像數應相同。 換句話說，如果在兩個軸上旋轉，則需要的影像數是單角度旋轉的兩倍。

![影像](assets/media-sets/image-set-3.png)

_旋轉集示例_

上述旋轉集的URL可能顯示為：

![影像](assets/media-sets/spin-set.png)

- 瞭解有關旋轉集的詳細資訊 [快速啟動到旋轉集](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/spin-sets/quick-start-spin-sets.html)。
- 瞭解如何 [建立旋轉集](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/spin-sets/creating-spin-set.html#creating-a-spin-set)。

## 混合媒體集

這是組合集。 它允許您將以前的任何集合，並將視頻添加到單個查看器中。 在此工作流中，您首先建立任何元件集，然後將它們組合到一個混合媒體集中。

![影像](assets/media-sets/image-set-4.png)

_混合媒體集示例_

上述混合媒體集的URL可能顯示為：

![影像](assets/media-sets/image-set-url-1.png)

- 瞭解有關混合媒體集的詳細資訊 [快速啟動混合媒體集](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/mixed-media-sets/quick-start-mixed-media-sets.html)。

- 瞭解如何 [建立混合媒體集](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/mixed-media-sets/creating-mixed-media-set.html#creating-a-mixed-media-set)。

要在網站上顯示縮放影像、集合或視頻，請在Dynamic Media Classic「查看器」中將其稱為。 Dynamic Media Classic公司包括富媒體資產的觀眾，如斯沃琪集，旋轉集，視頻等。

瞭解有關 [AEM Assets和Dynamic Media Classic觀眾](https://experienceleague.adobe.com/docs/dynamic-media-developer-resources/library/viewers-aem-assets-dmc/c-html5-s7-aem-asset-viewers.html)。

## 批次集預設集

到目前為止，我們一直在討論如何使用「Dynamic Media Classic構建」功能手動構建集。 但是，只要您有標準化的命名約定，就可以使用批集預設自動建立映像集和旋轉集。

每個預設都是一組唯一命名的、自包含的指令，這些指令定義了如何使用與定義的命名約定相匹配的影像來構造集合。 在預設中，首先為要在集合中分組的資產定義命名約定。 然後可以建立「批集預設」以引用這些影像。

雖然您可以自己建立預設(可在 **設定>應用程式設定>批集預設** )，作為最佳做法，您應讓咨詢團隊或技術支援為您設定。 原因如下：

- 批集預設設定可能很複雜 — 它們由規則運算式提供支援，除非您是開發人員，否則此語法可能不熟悉或令人困惑。
- 建立後，預設情況下會開啟它們。 沒有「撤消」功能。 如果您開始上載數千個影像，並且您的預設配置不正確，則最終可能會出現數百或數千個必須手動查找和刪除的損壞集。

先前建議了一種簡單的命名約定，它很容易構建成批集預設。 但是，由於預設非常靈活，它們可以處理複雜的命名策略。 簡言之，屬於某個集的映像應通過某個通用名稱捆綁在一起 — 通常是SKU號或產品ID。 在Dynamic Media Classic，您可以為要用於預設的所有影像指定預設的命名約定，也可以建立多個預設，每個預設具有不同的命名規則。

「批集預設」僅在上載時應用；映像上傳後無法運行。 因此，在開始載入所有影像之前，必須規劃命名約定並獲取一個預設。

建立預設後，公司管理員可以選擇它們是活動的還是非活動的。 活動，表示它們將顯示在上載頁面的 **作業選項**，而非活動預設將保持隱藏。

瞭解如何 [建立批集預設](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html#creating-a-batch-set-preset)。

### 在上載時使用批集預設

建立批集預設後，在上載時如何使用這些預設：

1. 按一下 **上載** 選擇 **從案頭** 或 **通過FTP**。
2. 按一下 **作業選項**。
3. 開啟 **批集預設** 選項，然後選中或取消選中預設以將其用於上載。
4. 上載完成後，在資料夾中查找完成的集。

瞭解有關 [批集預設](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html#batch-set-presets)。
