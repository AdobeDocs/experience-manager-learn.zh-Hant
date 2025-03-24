---
title: 在AEM中開發Sling模型匯出工具
description: 此技術逐步解說會逐步解說如何設定AEM以與Sling模型匯出工具搭配使用、使用匯出工具架構增強現有的Sling模型以轉譯為JSON，以及如何使用匯出工具選項和Jackson註解來進一步自訂輸出。
version: Experience Manager 6.4, Experience Manager 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
doc-type: Technical Video
topic: Development
role: Developer
level: Intermediate
exl-id: fc321ed1-5cf7-4bbe-adc6-c4905af7b43c
duration: 932
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 0%

---

# 開發Sling模型匯出工具

此技術逐步解說會逐步解說如何設定AEM以與Sling模型匯出工具搭配使用、使用匯出工具架構增強現有的Sling模型以轉譯為JSON，以及如何使用匯出工具選項和Jackson註解來進一步自訂輸出。

Sling模型匯出工具已在Sling模型v1.3.0中推出。此新功能可讓您將新註解新增至Sling模型，以定義如何將模型匯出為不同的Java物件，或更常見的是，匯出為不同的格式，例如JSON。

Apache Sling提供Jackson JSON匯出工具，以涵蓋將Sling模型匯出為JSON物件以供程式化Web消費者(例如其他Web服務和JavaScript應用程式)使用的最常見案例。

## 為Sling模型匯出工具設定AEM

>[!VIDEO](https://video.tv.adobe.com/v/16862?quality=12&learn=on)

[!DNL Sling Model Exporter]是[!DNL Apache Sling]專案的功能，未直接繫結至AEM產品發行週期。 [!DNL Sling Model Exporter]與AEM 6.3和更新版本相容。

## [!DNL Sling Model Exporter]的使用案例

>[!VIDEO](https://video.tv.adobe.com/v/16863?quality=12&learn=on)

[!DNL Sling Model Exporter]非常適合運用已包含商業邏輯的Sling模型，這些商業邏輯透過HTL （或以前的JSP）支援HTML轉譯，並顯示與JSON相同的商業表示以供程式化Web服務或JavaScript應用程式使用。

## 建立Sling模型匯出工具

>[!VIDEO](https://video.tv.adobe.com/v/16864?quality=12&learn=on)

在[!DNL Sling Model]上啟用[!DNL Exporter]支援，就像將`@Exporter`註解新增至Java類別一樣簡單。

## 套用Sling模型匯出工具選項

>[!VIDEO](https://video.tv.adobe.com/v/16865?quality=12&learn=on)

[!DNL Sling Model Exporter]支援將每個模型的匯出工具選項傳遞至匯出工具實作，以驅動[!DNL Sling Model]最終匯出的方式。 這些選項通常會「全域」套用至[!DNL Sling Model]的匯出方式，而不是透過下述內嵌註解完成的每個資料點。

[!DNL Jackson Exporter]選項包括：

* [對應程式功能選項](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [序列化功能選項](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

## 套用[!DNL Jackson]註解

>[!VIDEO](https://video.tv.adobe.com/v/16866?quality=12&learn=on)

匯出工具實作也支援可內巢狀用至[!DNL Sling Model]類別的註解，可提供更細微的控制資料匯出方式。

* [[!DNL Jackson Exporter] 附註](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)

## 檢視程式碼 {#view-the-code}

[SampleSlingModelExporter.java](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleSlingModelExporter.java)

## 支援材料 {#supporting-materials}

* [[!DNL Jackson Mapper] 功能Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [[!DNL Jackson Serialization] 功能Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

* [[!DNL Jackson Annotations] 檔案](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)
