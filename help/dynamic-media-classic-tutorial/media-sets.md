---
title: 影像、色票、回轉和混合媒體集
description: Dynamic Media經典最有用、最強大的功能之一，就是支援建立多媒體集，例如影像、色票、回轉和混合媒體集。 瞭解各種豐富式媒體集，以及如何在Dynamic Media經典中建立各種類型。 接著，進一步瞭解「批次集預設集」，此預設集可自動化上傳時建立多媒體集的程式。
sub-product: 動態媒體
feature: Dynamic Media Classic, Image Sets, Mix Media Sets, Spin Sets
doc-type: tutorial
topics: sets, development, authoring, configuring
audience: all
activity: use
topic: Content Management
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1471'
ht-degree: 1%

---


# 影像、色票、回轉和混合媒體集{#media-sets}

Dynamic Media經典系列可超越單一影像，以動態調整大小和縮放，提供更豐富的線上體驗。 教學課程的本節將探討如何在Dynamic Media經典中建立下列多媒體集：

- 影像集
- 色票集
- 迴轉集
- 混合媒體集

此外，還將說明如何使用「批次集預設集」，透過上傳自動建立集。

## 你一直想知道的關於場景的一切

除了基本的動態調整大小和縮放功能外，集合可能是最廣泛使用的Dynamic Media經典子產品。 集合實質上是「虛擬」資產，不含實際影像，但是包含與其他影像和／或視訊的一組關係。 電影的主要吸引力在於，它們是可「立即上架」的微型應用程式。 也就是說，每個設定檢視器都包含其專屬的邏輯和介面，因此您只需在網站上呼叫這些檢視器即可。 此外，它們只要求您追蹤每組資產ID，而不需親自管理所有會員資產和關係。

當您建立集合時，該集合會被管理為個別資產，必須標籤為發佈並發佈，才能從URL提供。 其所有會員資產也必須發佈。

### 集類型

讓我們來瞭解一下您在Dynamic Media經典中可以建立的四種集合：影像、色票、回轉和混合媒體集。

## 影像集

這是最常見的集合類型。 您通常會將它用於相同項目的替代檢視。 它包含多張影像，您可按一下該影像的相關縮圖，以載入檢視器。

![影像](assets/media-sets/image-set-1.jpg)

_影像集範例_

上述影像集的URL可能會顯示為：

![影像](assets/media-sets/image-set-url-1.png)

- 透過[快速入門影像集](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/image-sets/quick-start-image-sets.html)進一步瞭解影像集。
- 瞭解如何[建立影像集](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/image-sets/creating-image-set.html#creating-an-image-set)。

### 色票集

此類型的集通常用於顯示相同項目的彩色視圖。 它由成對的影像和色票組成。

「色票」和「影像集」的主要差異在於，「色票集」使用不同的影像做為可點選的色票，而「影像集」則使用原始影像的縮圖版本。

色票集不會為影像上色（常見的誤解）。 這些影像只是在交換，就像在影像集中一樣。 迷你色票影像可能是使用Photoshop製作的，每種顏色都可能單獨拍攝，或者Dynamic Media經典的裁切工具可能用來從彩色影像中製作色票。

![影像](assets/media-sets/image-set-2.jpg)

_色票集範例_

上述色票集的URL可能會顯示為：

![影像](assets/media-sets/image-set_url.png)

- 使用[快速開始使用色票集](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/swatch-sets/quick-start-swatch-sets.html)進一步瞭解色票集。
- 瞭解如何[建立色票集](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/swatch-sets/creating-swatch-set.html#creating-a-swatch-set)。

### 迴轉集

此集通常用於顯示項目的360度視圖。 如同色票集，回轉集不使用3D魔力— 真正的工作是從各方面製作許多影像像片。 檢視器可讓您在影像之間切換，就像停止動畫一樣。

「回轉集」可以沿單個軸沿一個方向回轉，或者如果交替建立為2D回轉集，則可以： 在多個軸上旋轉。 例如，汽車可以在所有車輪都在地面時進行旋轉，然後也可以在其後輪上「翻轉」並旋轉。 若是正確設定2D回轉集，每個軸的每列影像數應相同。 換言之，如果您在兩軸上旋轉，則需要的影像數是單一角度旋轉的兩倍。

![影像](assets/media-sets/image-set-3.png)

_回轉集範例_

上述回轉集的URL可能會顯示為：

![影像](assets/media-sets/spin-set.png)

- 透過[快速開始回轉集](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/spin-sets/quick-start-spin-sets.html)進一步瞭解回轉集。
- 瞭解如何[建立回轉集](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/spin-sets/creating-spin-set.html#creating-a-spin-set)。

## 混合媒體集

這是組合集。 它可讓您將任何先前的組合，以及新增視訊到單一檢視器中。 在此工作流中，您首先建立任何元件集，然後將它們組合到一個混合媒體集中。

![影像](assets/media-sets/image-set-4.png)

_混合媒體集示例_

上述混合媒體集的URL可能會顯示為：

![影像](assets/media-sets/image-set-url-1.png)

- 使用[快速入門到混合媒體集](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/mixed-media-sets/quick-start-mixed-media-sets.html)進一步瞭解混合媒體集。

- 瞭解如何[建立混合媒體集](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/mixed-media-sets/creating-mixed-media-set.html#creating-a-mixed-media-set)。

若要在您的網站上顯示影像以進行縮放、設定或視訊，請在Dynamic Media經典影像的「檢視器」中將其命名。 Dynamic Media經典影像包括多媒體資產的檢視器，例如色票集、回轉集、視訊和其他許多。

進一步瞭解[AEM Assets和Dynamic MediaClassic的檢視器](https://docs.adobe.com/content/help/en/dynamic-media-developer-resources/library/viewers-aem-assets-dmc/c-html5-s7-aem-asset-viewers.html)。

## 批次集預設集

到目前為止，我們一直在討論如何使用Dynamic Media經典構建功能手動構建集。 不過，只要您有標準化的命名慣例，就可以使用批次集預設集自動建立影像集和回轉集。

每個預設集都是一組唯一命名、獨立的指令，這些指令定義如何使用符合所定義命名慣例的影像來建構該組。 在預設集中，您會先為想要在集合中分組的資產定義命名慣例。 然後可以建立批次集預設集以參考這些影像。

雖然您可以自行建立預設集（這些預設集位於&#x200B;**設定>應用程式設定>批次設定預設集**&#x200B;下），但您最好由諮詢團隊或技術支援為您設定。 原因如下：

- 批次集預設集可能很複雜，要進行設定— 它們採用規則運算式，除非您是開發人員，否則此語法可能不熟悉或令人困惑。
- 在建立後，預設會開啟這些功能。 沒有「還原」功能。 如果您開始上傳數千張影像，而您的預設集設定不正確，則可能會有數百或數千組中斷的集合，您必須手動尋找並刪除。

先前已建議使用簡單的命名慣例，可輕鬆建立至批次集預設集。 不過，由於預設集非常有彈性，所以可處理複雜的命名策略。 簡言之，屬於一組的影像應該用某種共同名稱來連接在一起。 通常是SKU編號或產品ID。 在Dynamic Media經典影像中，您可以告訴它預設的命名規則，以用於預設集，或者您可以建立多個預設集，每個預設集具有不同的命名規則。

批次集預設集僅在上傳時套用；在上傳影像後，就無法執行這些動作。 因此，在開始載入所有影像之前，請務必規劃您的命名慣例並取得建立的預設集。

在建立預設集後，公司管理員可以選擇預設集是活動中或非活動中。 作用中表示這些預設集會顯示在「作業選項」下的上傳頁面上，而非作用中的預設集則會保持隱藏。****

瞭解如何[建立批次集預設集](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/setup/application-setup.html#creating-a-batch-set-preset)。

### 在上傳時使用批次集預設集

在建立批次集預設集後，如何在上傳時使用這些預設集：

1. 按一下&#x200B;**上傳**&#x200B;並選擇&#x200B;**從案頭**&#x200B;或&#x200B;**通過FTP**。
2. 按一下&#x200B;**作業選項**。
3. 開啟&#x200B;**批次設定預設集**&#x200B;選項，並勾選或取消勾選預設集，以便與上傳搭配使用。
4. 上傳完成後，請在您的檔案夾中尋找完成的集合。

進一步瞭解[批次設定預設集](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/setup/application-setup.html#batch-set-presets)。
