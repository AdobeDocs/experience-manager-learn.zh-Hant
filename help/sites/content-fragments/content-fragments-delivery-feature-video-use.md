---
title: 將內容片段傳送至
seo-title: 在Adobe Experience Manager提供內容片段
description: 內容片段與版面無關，可直接與核心元件一起在AEM Sites使用，或以無頭方式傳送至下游通道。
seo-description: 內容片段與版面無關，可直接與核心元件一起在AEM Sites使用，或以無頭方式傳送至下游通道。
sub-product: 內容服務
feature: 內容片段
topics: authoring, content-architecture
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
uuid: 045473d2-5abe-4414-b91c-d369f3069ead
discoiquuid: 912e0c41-83cf-49f7-b515-09519b6718c1
topic: 內容管理
role: 業務從業人員
level: 初學者
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '599'
ht-degree: 2%

---


# 傳送內容片段{#delivering-content-fragments}

Adobe Experience Manager(AEM)內容片段是文字編輯內容，可能包含一些與之相關的結構化資料元素，但視為純粹的內容，而無設計或版面資訊。 內容片段通常建立為不受通道限制的內容，以便跨通道使用和重複使用，進而將內容包住特定內容的內容。

內容片段與版面無關，可直接與核心元件一起在AEM Sites使用，或以無頭方式傳送至下游通道。

此影片系列涵蓋使用內容片段的傳送選項。 有關定義和[編寫內容片段的詳細資訊，請參閱](content-fragments-feature-video-use.md)。

1. 在網頁上使用內容片段
2. 使用內容服務將內容片段公開AEM為JSON
3. 使用資產HTTP API

## 在網頁{#using-content-fragments-in-web-pages}中使用內容片段

>[!VIDEO](https://video.tv.adobe.com/v/22449/?quality=12&learn=on)

「內容片段」可以用於AEM Sites頁面，或以類似方式使用「體驗片段」,AEM使用「WCM核心元件」的「內容片段元件」[](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-component.html)。

內容片段元件可使用樣式系統AEM來設定樣式，以視需要顯示內容。

## 將內容片段公開為JSON {#exposing-content-fragments-as-json}

>[!VIDEO](https://video.tv.adobe.com/v/22448/?quality=12&learn=on)

Content ServicesAEM可協助建立以頁AEM面為基礎的HTTP端點，將內容轉譯為標準化的JSON格式。

上述視訊使用[內容片段元件](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-component.html)來公開個別的內容片段。 [內容片段清單元件](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-list.html)是新元件，可讓作者定義查詢，以動態填入頁面的內容片段清單。 當需要公開多個內容片段時，偏好使用內容片段清單元件。

*範例Content Services端點JSON裝載：*\
**[materies.json](assets/athletes.json)**

## 使用資產HTTP API

>[!VIDEO](https://video.tv.adobe.com/v/26390/?quality=12&learn=on)

6.5中首AEM先推出的是使用資產HTTP API增強對內容片段的支援。 這可讓開發人員輕鬆對內容片段執行「建立」、「讀取」、「更新」和「刪除」(CRUD)操作。

*範例POSTMAN請求：*
**[CRUD-CFM-API-We.Retail.postman_collection.json](assets/CRUD-CFM-API-We.Retail.postman_collection.json)**

## 使用哪一種傳送方法

### Web Channel

透過Web頻道傳送內容片段的方法很簡單，只要搭配使用內容片段元件與AEM Sites。

### 無頭

將內容片段公開為JSON有兩種選項，以支援無頭使用案例中的第三方頻道：

1. 當主要AEM使用案例是傳送內容片段供第三方頻道使用（唯讀）時，請使用「內容服務」和「Proxy API」頁面（視訊#2）。 Content Services架構提供更靈活的彈性和選項，讓資料公開。 開發人員也可以延伸內容服務架構，以增強和／或豐富資料。

2. 當第三方頻道需要修改及／或更新內容片段時，請使用資產HTTP API（視訊#3）。 典型的使用案例是在作者環境中吸收第三方AEM內容。

## 其他資源 {#additional-resources}

* [編寫內容片段](content-fragments-feature-video-use.md)
* [AEMWCM核心元件](https://docs.adobe.com/content/help/zh-Hant/experience-manager-core-components/using/introduction.html)
* [AEMWCM核心內容片段元件](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-component.html)

若要從視訊系列下載並安裝AEM下列套件至6.4+執行個體，以取得最終狀態：\
**[aem_demo_fluid-experiencecontent-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**
