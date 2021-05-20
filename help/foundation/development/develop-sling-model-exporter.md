---
title: 在AEM中開發Sling模型匯出工具
description: 本技術逐步介紹如何設定AEM以搭配Sling Model Exporter使用、使用Exporter架構來將轉譯為JSON以增強現有的Sling Model，以及如何使用Exporter選項和Jackson註解來進一步自訂輸出。
version: 6.3, 6.4, 6.5
sub-product: 基礎，內容服務
feature: API
topics: content-delivery, development, headless
activity: develop
audience: developer
doc-type: technical video
topic: 開發
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '423'
ht-degree: 0%

---


# 開發Sling模型匯出工具

本技術逐步介紹如何設定AEM以搭配Sling Model Exporter使用、使用Exporter架構來將轉譯為JSON以增強現有的Sling Model，以及如何使用Exporter選項和Jackson註解來進一步自訂輸出。

Sling Models v1.3.0導入了Sling Models。這項新功能可讓Sling Model新增註解，定義模型如何匯出為不同Java物件，或更常見的方式是序列化為不同格式，例如JSON。

Apache Sling提供Jackson JSON匯出工具，涵蓋將Sling模型匯出為JSON物件的最常見案例，以供程式化網頁消費者使用，例如其他網站服務和JavaScript應用程式。

## 為Sling模型匯出工具配置AEM

>[!VIDEO](https://video.tv.adobe.com/v/16862/?quality=12&learn=on)

[!DNL Sling Model Exporter] 是專案的功 [!DNL Apache Sling] 能，不會直接系結至AEM產品發行週期。[!DNL Sling Model Exporter] 與AEM 6.3和更新版本相容。

## [!DNL Sling Model Exporter]的使用案例

>[!VIDEO](https://video.tv.adobe.com/v/16863/?quality=12&learn=on)

[!DNL Sling Model Exporter] 最適合使用已包含透過HTL（或先前的JSP）支援HTML轉譯之商業邏輯的Sling模型，並公開與JSON相同的商業表示法，以供程式化網站服務或JavaScript應用程式使用。

## 建立Sling模型匯出工具

>[!VIDEO](https://video.tv.adobe.com/v/16864/?quality=12&learn=on)

在[!DNL Sling Model]上啟用[!DNL Exporter]支援就像將`@Exporter`注釋添加到Java類一樣簡單。

## 套用Sling模型匯出工具選項

>[!VIDEO](https://video.tv.adobe.com/v/16865/?quality=12&learn=on)

[!DNL Sling Model Exporter] 支援將每個模型導出器選項傳遞到導出器實施，以驅動最終 [!DNL Sling Model] 導出的方式。這些選項通常會套用「全域」至[!DNL Sling Model]的匯出方式，而不是透過下述內嵌註解來完成的資料點。

[!DNL Jackson Exporter] 選項包括：

* [映射器功能選項](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [序列化功能選項](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

## 應用[!DNL Jackson]注釋

>[!VIDEO](https://video.tv.adobe.com/v/16866/?quality=12&learn=on)

匯出工具實施也可支援可內嵌在[!DNL Sling Model]類別上套用的註解，以提供更精細的資料匯出控制層級。

* [[!DNL Jackson Exporter] 附註](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)

## 查看代碼{#view-the-code}

[SampleSlingModelExporter.java](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleSlingModelExporter.java)

## 支援材料{#supporting-materials}

* [[!DNL Jackson Mapper] 功能Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [[!DNL Jackson Serialization] 功能Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

* [[!DNL Jackson Annotations] 文件](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)
