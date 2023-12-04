---
title: 影像、色票、迴轉和混合媒體集
description: Dynamic Media Classic最有用且功能強大的功能之一，是支援建立豐富媒體集，例如影像、色票、迴轉和混合媒體集。 瞭解每個多媒體集是什麼，以及如何在Dynamic Media Classic中建立每個型別。 然後深入瞭解批次集預設集，它會在上傳時自動建立多媒體集的流程。
feature: Dynamic Media Classic, Image Sets, Mixed Media Sets, Spin Sets
topic: Content Management
role: User
level: Beginner
doc-type: Tutorial
exl-id: 45c86ff2-d991-46a7-a8d1-25c9fec142d9
duration: 353
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '1359'
ht-degree: 1%

---

# 影像、色票、迴轉和混合媒體集 {#media-sets}

Dynamic Media Classic集合超越了單一影像以動態調整大小和縮放，可提供更豐富的線上體驗。 教學課程的此章節將探索如何在Dynamic Media Classic中建立下列多媒體集：

- 影像集
- 色票集
- 迴轉集
- 混合媒體集

它也會說明如何使用批次集預設集透過上傳來自動建立集合。

## 關於集合的所有您永遠想要瞭解的事項

在基本動態大小和縮放旁，集合可能是最廣泛使用的Dynamic Media Classic子產品。 集合本質上是不包含實際影像的「虛擬」資產，但包含與其他影像和/或視訊的一組關係。 這些套裝的主要吸引力在於，它們是「現成可用」的迷你應用程式。 也就是說，每個已設定的檢視器含有其專屬的邏輯和介面，您只需在網站上呼叫檢視器即可。 此外，它們只需要您追蹤每個集的單一資產ID，而不需要自己管理所有成員資產和關係。

當您建立資產集時，該資產集會作為個別資產進行管理，必須先標籤為發佈並發佈，然後才能從URL提供服務。 其所有成員資產也必須發佈。

### 集合型別

讓我們瞭解您可以在Dynamic Media Classic中建立的四種型別集：影像、色票、迴轉和混合媒體集。

## 影像集

這是最常見的集合型別。 您通常會將其用於相同專案的替代檢視。 它由多個影像組成，您可以按一下該影像的相關縮圖，將這些影像載入檢視器。

![影像](assets/media-sets/image-set-1.jpg)

_影像集範例_

上述影像集的URL可能顯示為：

![影像](assets/media-sets/image-set-url-1.png)

- 進一步瞭解影像集，使用 [影像集快速入門](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sets/quick-start-image-sets.html).
- 瞭解如何 [建立影像集](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sets/creating-image-set.html#creating-an-image-set).

### 色票集

此型別的集合通常用於顯示相同專案的彩色檢視。 它由影像和色票配對組成。

「色票」和「影像集」的主要差異在於「色票集」使用不同的影像作為可點選的色票，而「影像集」則使用原始影像的微型、可點選的縮圖版本。

色票集不會為影像上色（常見的誤解）。 只是交換影像，完全等同於影像集。 您可以透過Photoshop製作迷你色票影像，或是分別拍攝每個顏色，或是使用Dynamic Media Classic中的裁切工具從其中一個彩色影像中製作色票。

![影像](assets/media-sets/image-set-2.jpg)

_色票集範例_

上述色票集的URL可能顯示為：

![影像](assets/media-sets/image-set_url.png)

- 進一步瞭解使用時的色票集 [色票集快速入門](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/swatch-sets/quick-start-swatch-sets.html).
- 瞭解如何 [建立色票集](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/swatch-sets/creating-swatch-set.html#creating-a-swatch-set).

### 迴轉集

此集合通常用於顯示專案的360度檢視。 像色票集一樣，迴轉集不會使用3D魔術，真正的工作是從各個角度建立許多影像的像片。 檢視器僅可讓您在影像之間切換，如同停止動畫一樣。

「迴轉集」可以沿著單一軸在一個方向迴轉，或者如果交替建立為「2D迴轉集」 — 在多個軸上迴轉。 例如，汽車可以在所有車輪都位於地面時旋轉，然後可以「翻轉」並在其後輪上旋轉。 若為正確設定的2D迴轉集，每個軸的每列影像數應相同。 換言之，如果您在兩軸上旋轉，則需要兩倍於單一角度旋轉的影像。

![影像](assets/media-sets/image-set-3.png)

_迴轉集範例_

上述迴轉集的URL可能顯示為：

![影像](assets/media-sets/spin-set.png)

- 進一步瞭解迴轉集，使用 [快速啟動迴轉集](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/spin-sets/quick-start-spin-sets.html).
- 瞭解如何 [建立迴轉集](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/spin-sets/creating-spin-set.html#creating-a-spin-set).

## 混合媒體集

這是組合集。 它可讓您合併任何先前的組合，以及將視訊新增至單一檢視器。 在此工作流程中，您會先建立任何元件集，然後將它們組合成混合媒體集。

![影像](assets/media-sets/image-set-4.png)

_混合媒體集範例_

上述混合媒體集的URL可能顯示為：

![影像](assets/media-sets/image-set-url-1.png)

- 進一步瞭解混合媒體集與 [混合媒體集快速入門](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/mixed-media-sets/quick-start-mixed-media-sets.html).

- 瞭解如何 [建立混合媒體集](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/mixed-media-sets/creating-mixed-media-set.html#creating-a-mixed-media-set).

若要在網站上顯示縮放、集合或視訊的影像，請在Dynamic Media Classic的「檢視器」中將其呼叫。 Dynamic Media Classic包含豐富的媒體資產檢視器，例如色票集、迴轉集、視訊和許多其他。

進一步瞭解 [AEM Assets和Dynamic Media Classic的檢視器](https://experienceleague.adobe.com/docs/dynamic-media-developer-resources/library/viewers-aem-assets-dmc/c-html5-s7-aem-asset-viewers.html).

## 大量集預設集

到目前為止，我們一直在討論如何使用Dynamic Media Classic建置功能手動建置集合。 不過，只要您有標準化的命名慣例，您仍可使用批次集預設集來自動建立影像集和迴轉集。

每個預設集都是一組名稱唯一、內含的指示，用於定義如何使用符合所定義命名慣例的影像來建構集合。 在預設集中，您首先會為要分組到一組中的資產定義命名慣例。 然後可以建立批次集預設集來參考這些影像。

雖然您可以自行建立預設集(位於 **設定>應用程式設定>批次集預設集** )，最佳實務是請您的諮詢團隊或技術支援為您設定。 原因如下：

- 批次集預設集的設定可能很複雜 — 它們由規則運算式提供支援，除非您是開發人員，否則此語法可能不熟悉或令人困惑。
- 建立後，預設會開啟這些功能。 沒有「還原」功能。 如果您開始上傳數千個影像，而預設集設定不正確，您最終可能會收到數百或數千個損毀的集合，您必須手動尋找和刪除這些集合。

先前建議使用簡易的命名慣例，以便輕鬆建置至批次集預設集。 不過，由於預設集非常靈活，因此可處理複雜的命名策略。 簡而言之，屬於同一組的影像應透過某種通用名稱繫結在一起，通常是SKU編號或產品ID。 在Dynamic Media Classic中，您可以為所有要用於預設集的影像指定預設命名慣例，也可以建立多個預設集，每個預設集都有不同的命名規則。

批次集預設集只會在上傳時套用；無法在影像上傳後執行。 因此，在開始載入所有影像之前，請務必規劃您的命名慣例，並建置預設集。

建立預設集後，公司管理員可以選擇預設集是作用中或非作用中。 啟用表示它們會顯示在下方的上傳頁面上 **工作選項**，非作用中的預設集仍會保持隱藏。

瞭解如何 [建立批次集預設集](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html#creating-a-batch-set-preset).

### 在上傳時使用批次集預設集

以下說明建立批次集預設集後，如何在上傳時使用批次集預設集：

1. 按一下 **上傳** 並選擇 **從案頭** 或 **透過FTP**.
2. 按一下 **工作選項**.
3. 開啟 **批次集預設集** 選項，然後核取或取消核取預設集以將其用於上傳。
4. 上傳完成後，請檢視資料夾中的已完成集合。

進一步瞭解 [批次集預設集](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html#batch-set-presets).
