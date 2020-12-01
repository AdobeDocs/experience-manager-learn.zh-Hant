---
title: 在AEM中編寫內容片段
description: '「內容片段」是AEM中的內容抽象，可讓文字內容獨立於其支援的頻道進行製作和管理。 '
sub-product: 內容服務
feature: content-fragments
topics: authoring, content-architecture
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 1faf22f2e664b775c11e16cb1dfa18b363a7316b
workflow-type: tm+mt
source-wordcount: '396'
ht-degree: 2%

---


# 編寫內容片段{#authoring-content-fragments}

「內容片段」是AEM中的內容抽象，可讓文字內容獨立於其支援的頻道進行製作和管理。

>[!NOTE]
>
>這些影片中涵蓋的AEM內容片段功能最初是在[AEM 6.3 + FP 19008和FP19614](https://helpx.adobe.com/experience-manager/6-3/release-notes/content-services-fragments-featurepack.html)中引進的。


AEM內容片段是文字編輯內容，可能包含一些關聯的結構化資料元素，但視為純內容，而無設計或版面資訊。 內容片段通常建立為不受通道限制的內容，以便跨通道使用和重複使用，進而將內容包住特定內容的內容。

此影片系列涵蓋AEM中內容片段的製作生命週期。 有關[傳送內容片段的詳細資訊，請參閱此處](content-fragments-delivery-feature-video-use.md)。

1. 啟用和定義內容片段模型
2. 編寫內容片段
3. 下載內容片段
4. 編輯功能

## 定義內容片段模型{#defining-content-fragment-models}

>[!VIDEO](https://video.tv.adobe.com/v/22452/?quality=12&learn=on)

AEM內容片段模型（內容片段的資料結構）必須透過AEM的[[!UICONTROL Configuration Browser]](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/configurations.html)啟用，這可讓內容片段模型根據每個組態來定義。

## 建立內容片段{#creating-content-fragments}

>[!VIDEO](https://video.tv.adobe.com/v/22451/?quality=12&learn=on)

AEM設定會套用至AEM Assets檔案夾階層，以允許其「內容片段模型」建立為「內容片段」。 內容片段支援多樣化的表單製作體驗，讓內容可建模為元素的集合。

內容片段可以有多個變體，每個變體都針對內容的不同使用案例（思維，不一定是頻道）。

*匯入的運動員傳記範例：*\
**[sandra-sprient-bio.txt](assets/sandra-sprient-bio.txt)**

## 下載內容片段{#downloading-content-fragments}

>[!VIDEO](https://video.tv.adobe.com/v/22450/?quality=12&learn=on)

AEM內容片段可從AEM Author下載為包含變數、元素和中繼資料的Zip檔案。

*內容片段下載Zip檔案範例：*\
**[daniel_schreder.zip](assets/daniel_schreder.zip)**

## 內容片段編輯功能{#editorial-capabilities}

>[!VIDEO](https://video.tv.adobe.com/v/25891/?quality=12&learn=on)

>[!NOTE]
>
> [AEM 6.4 Service Pack 2](https://helpx.adobe.com/experience-manager/aem-releases-updates.html)和[AEM 6.3 Service Pack 3](https://helpx.adobe.com/experience-manager/6-3/release-notes/sp3-release-notes.html)已引入內容片段的註解和版本比較。

## 後續步驟

瞭解[傳送內容片段](content-fragments-delivery-feature-video-use.md)。

## 其他資源 {#additional-resources}

* [傳送內容片段](content-fragments-delivery-feature-video-use.md)
* [AEM WCM核心元件](https://docs.adobe.com/content/help/zh-Hant/experience-manager-core-components/using/introduction.html)
* [AEM WCM核心內容片段元件](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-component.html)

若要從視訊系列下載並安裝下列套件至AEM 6.4+執行個體，以獲得最終狀態：

**[aem_demo_fluid-experiencecontent-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**
