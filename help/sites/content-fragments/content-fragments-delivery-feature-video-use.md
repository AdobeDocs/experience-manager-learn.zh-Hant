---
title: 在AEM中傳送內容片段
seo-title: Delivering Content Fragments in Adobe Experience Manager
description: 內容片段與版面無關，可直接在AEM Sites中搭配核心元件使用，或以無頭方式傳送至下游頻道。
seo-description: Content Fragments, independent of layout, can be used directly in AEM Sites with Core Components or can be delivered in a headless manner to downstream channels.
feature: Content Fragments
topics: authoring, content-architecture
audience: all
doc-type: feature video
activity: use
version: 6.4, 6.5
uuid: 045473d2-5abe-4414-b91c-d369f3069ead
discoiquuid: 912e0c41-83cf-49f7-b515-09519b6718c1
topic: Content Management
role: User
level: Beginner
exl-id: 525cd30c-05bf-4f17-b61b-90609ce757ea
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '548'
ht-degree: 1%

---

# 傳送內容片段 {#delivering-content-fragments}

Adobe Experience Manager(AEM)內容片段是文字型編輯內容，可能包含一些相關聯的結構化資料元素，但不含設計或版面資訊，視為純內容。 內容片段通常建立為不受管道限制的內容，以用於不同管道並加以重複使用，而管道又會將內容包裝在內容特定的體驗中。

內容片段與版面無關，可直接在AEM Sites中搭配核心元件使用，或以無頭方式傳送至下游頻道。

此影片系列涵蓋使用內容片段的傳送選項。 定義和的詳細資訊 [您可以在此處找到製作內容片段](content-fragments-feature-video-use.md).

1. 在網頁上使用內容片段
2. 使用AEM內容服務將內容片段顯示為JSON
3. 使用Assets HTTP API

## 在網頁中使用內容片段 {#using-content-fragments-in-web-pages}

>[!VIDEO](https://video.tv.adobe.com/v/22449/?quality=12&learn=on)

內容片段可在AEM Sites頁面上使用，或透過AEM WCM核心元件以類似方式使用體驗片段。 [內容片段元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html).

可使用AEM樣式系統來設定內容片段元件的樣式，以視需要顯示內容。

## 將內容片段顯示為JSON {#exposing-content-fragments-as-json}

>[!VIDEO](https://video.tv.adobe.com/v/22448/?quality=12&learn=on)

AEM Content Services可方便建立AEM頁面式HTTP端點，將內容轉譯為標準化JSON格式。

上述影片使用 [內容片段元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html) 以公開個別內容片段。 此 [內容片段清單元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-list.html) 是新元件，可讓作者定義查詢，以動態方式將內容片段清單填入頁面。 需要公開多個內容片段時，偏好使用內容片段清單元件。

*內容服務端點JSON裝載範例：*\
**[maters.json](assets/athletes.json)**

## 使用Assets HTTP API

>[!VIDEO](https://video.tv.adobe.com/v/26390/?quality=12&learn=on)

AEM 6.5率先推出，現在透過資產HTTP API增強了對內容片段的支援。 這可讓開發人員針對內容片段執行建立、讀取、更新和刪除(CRUD)作業。

*POSTMAN請求範例：*
**[CRUD-CFM-API-We.Retail.postman_collection.json](assets/CRUD-CFM-API-We.Retail.postman_collection.json)**

## 要使用的傳送方法

### Web Channel

透過Web頻道傳送內容片段的方法透過搭配AEM Sites使用內容片段元件相當簡單明瞭。

### 無頭

以JSON形式顯示內容片段有兩個選項，可在無頭使用案例中支援第三方管道：

1. 當主要使用案例是透過第三方管道傳送內容片段以供使用（唯讀），請使用AEM內容服務和Proxy API頁面(影片#2)。 內容服務架構針對公開的資料提供更多彈性和選項。 開發人員也可以擴充內容服務架構，以增強和/或豐富資料。

2. 當第三方管道需要修改和/或更新內容片段時，請使用資產HTTP API(影片#3)。 典型的使用案例是在AEM製作環境中擷取第三方內容。

## 其他資源 {#additional-resources}

* [製作內容片段](content-fragments-feature-video-use.md)
* [AEM WCM核心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html)
* [AEM WCM核心內容片段元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html)

若要從影片系列下載並安裝以下套件，請在AEM 6.4+執行個體上執行最終狀態：\
**[aem_demo_fluid-experiencecontent-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**
