---
title: 在中國開發Sling模AEM型
description: 本技術介紹了與AEMSling模型導出器一起使用的設定、使用導出器框架將格式副本作為JSON增強現有Sling模型，以及如何使用導出器選項和Jackson注釋進一步自定義輸出。
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
topics: content-delivery, development, headless
activity: develop
audience: developer
doc-type: technical video
topic: Development
role: Developer
level: Intermediate
exl-id: fc321ed1-5cf7-4bbe-adc6-c4905af7b43c
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '419'
ht-degree: 0%

---

# 開發Sling模型出口商

本技術介紹了與AEMSling模型導出器一起使用的設定、使用導出器框架將格式副本作為JSON增強現有Sling模型，以及如何使用導出器選項和Jackson注釋進一步自定義輸出。

Sling Models v1.3.0中引入了Sling Model Exporter。此新功能允許將新注釋添加到Sling模型中，該模型定義如何將模型導出為不同的Java對象，或更常見地，將模型序列化為不同的格式（如JSON）。

Apache Sling提供Jackson JSON導出器，以涵蓋將Sling模型導出為JSON對象的最常見情況，供寫程式Web使用者使用，如其他Web服務和JavaScript應用程式。

## 為SlingAEM模型導出器配置

>[!VIDEO](https://video.tv.adobe.com/v/16862?quality=12&learn=on)

[!DNL Sling Model Exporter] 是 [!DNL Apache Sling] 不直接綁定到產品AEM發佈週期。 [!DNL Sling Model Exporter] 相容AEM6.3及更高版本。

## 用於 [!DNL Sling Model Exporter]

>[!VIDEO](https://video.tv.adobe.com/v/16863?quality=12&learn=on)

[!DNL Sling Model Exporter] 非常適合於利用已包含支援通過HTL（或以前的JSP）HTML格式副本的業務邏輯的Sling Models，並將與JSON相同的業務表示形式公開給程式化Web服務或JavaScript應用程式使用。

## 建立吊帶模型導出器

>[!VIDEO](https://video.tv.adobe.com/v/16864?quality=12&learn=on)

啟用 [!DNL Exporter] 支援 [!DNL Sling Model] 和添加一樣簡單 `@Exporter` Java類的注釋。

## 應用Sling模型導出器選項

>[!VIDEO](https://video.tv.adobe.com/v/16865?quality=12&learn=on)

[!DNL Sling Model Exporter] 支援將每個模型的導出器選項傳遞給導出器實施，以推動 [!DNL Sling Model] 。 這些選項通常適用於 [!DNL Sling Model] 導出，而每個資料點可通過下面描述的內聯注釋完成。

[!DNL Jackson Exporter] 選項包括：

* [映射器功能選項](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [序列化功能選項](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

## 應用 [!DNL Jackson] 注釋

>[!VIDEO](https://video.tv.adobe.com/v/16866?quality=12&learn=on)

出口商實施還可支援注釋，這些注釋可以在 [!DNL Sling Model] 類，可提供更精細的資料導出控制級別。

* [[!DNL Jackson Exporter] 附註](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)

## 查看代碼 {#view-the-code}

[SampleSlingModelExporter.java](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleSlingModelExporter.java)

## 支撐材料 {#supporting-materials}

* [[!DNL Jackson Mapper] 功能Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [[!DNL Jackson Serialization] 功能Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

* [[!DNL Jackson Annotations] 文件](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)
