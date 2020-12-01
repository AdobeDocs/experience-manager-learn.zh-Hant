---
title: 在AEM中開發Sling Model Exporator
description: 本技術逐步介紹如何設定AEM以搭配Sling Model Exporter使用、增強使用Exporter架構以轉譯為JSON的現有Sling Model，以及如何使用Exporter選項和Jackson註解進一步自訂輸出。
version: 6.3, 6.4, 6.5
sub-product: 基礎，內容服務
feature: sling-models, sling-model-exporter
topics: content-delivery, development, headless
activity: develop
audience: developer
doc-type: technical video
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '421'
ht-degree: 0%

---


# 開發Sling Model Exporator

本技術逐步介紹如何設定AEM以搭配Sling Model Exporter使用、增強使用Exporter架構以轉譯為JSON的現有Sling Model，以及如何使用Exporter選項和Jackson註解進一步自訂輸出。

Sling Model Exporter是在Sling Models v1.3.0中推出的。這項新功能可讓Sling Models新增註解，以定義Model an如何匯出為不同的Java物件，或更常見的是，序列化為不同的格式，例如JSON。

Apache Sling提供Jackson JSON匯出器，以涵蓋將Sling Models匯出為JSON物件的最常見案例，以供程式化網頁消費者（例如其他web services和JavaScript應用程式）使用。

## 設定AEM for Sling Model Exporter

>[!VIDEO](https://video.tv.adobe.com/v/16862/?quality=12&learn=on)

[!DNL Sling Model Exporter] 是專案的功能， [!DNL Apache Sling] 不直接系結至AEM產品發行週期。[!DNL Sling Model Exporter] 相容於AEM 6.3和更新版本。

## [!DNL Sling Model Exporter]的使用案例

>[!VIDEO](https://video.tv.adobe.com/v/16863/?quality=12&learn=on)

[!DNL Sling Model Exporter] 最適合運用已包含支援透過HTL（或先前的JSP）進行HTML轉譯的商業邏輯的Sling Models，並公開與JSON相同的商業表示法，以供程式化網站服務或JavaScript應用程式使用。

## 建立Sling Model Exporter

>[!VIDEO](https://video.tv.adobe.com/v/16864/?quality=12&learn=on)

在[!DNL Sling Model]上啟用[!DNL Exporter]支援就像在Java類中新增`@Exporter`註解一樣簡單。

## 套用Sling Model Exporter選項

>[!VIDEO](https://video.tv.adobe.com/v/16865/?quality=12&learn=on)

[!DNL Sling Model Exporter] 支援將每個模型導出器選項傳遞到導出器實施，以驅動最終導 [!DNL Sling Model] 出的方式。這些選項通常會套用「全域」至[!DNL Sling Model]的匯出方式，而透過下面所述的內嵌註解，則可依資料點執行。

[!DNL Jackson Exporter] 選項包括：

* [映射器功能選項](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [序列化功能選項](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

## 應用[!DNL Jackson]注釋

>[!VIDEO](https://video.tv.adobe.com/v/16866/?quality=12&learn=on)

出口商實作也可支援可內嵌在[!DNL Sling Model]類別上的註解，以提供更精細的資料匯出控制。

* [[!DNL Jackson Exporter] 附註](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)

## 檢視程式碼{#view-the-code}

[SampleSlingModelExporter.java](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleSlingModelExporter.java)

## 支援材料{#supporting-materials}

* [[!DNL Jackson Mapper] 功能Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [[!DNL Jackson Serialization] 功能Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

* [[!DNL Jackson Annotations] 文件](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)
