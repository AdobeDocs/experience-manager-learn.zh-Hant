---
title: 在AEM中開發Sling模型匯出工具
description: 本技術逐步介紹如何設定AEM以搭配Sling Model Exporter使用、使用Exporter架構來將轉譯為JSON以增強現有的Sling Model，以及如何使用Exporter選項和Jackson註解來進一步自訂輸出。
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
source-git-commit: 2f02a4e202390434de831ce1547001b2cef01562
workflow-type: tm+mt
source-wordcount: '419'
ht-degree: 0%

---

# 開發Sling模型匯出工具

本技術逐步介紹如何設定AEM以搭配Sling Model Exporter使用、使用Exporter架構來將轉譯為JSON以增強現有的Sling Model，以及如何使用Exporter選項和Jackson註解來進一步自訂輸出。

Sling Models v1.3.0導入了Sling Models。這項新功能可讓Sling Model新增註解，定義模型如何匯出為不同Java物件，或更常見的方式是序列化為不同格式，例如JSON。

Apache Sling提供Jackson JSON匯出工具，涵蓋將Sling模型匯出為JSON物件的最常見案例，以供程式化網頁消費者使用，例如其他網站服務和JavaScript應用程式。

## 為Sling模型匯出工具配置AEM

>[!VIDEO](https://video.tv.adobe.com/v/16862/?quality=12&learn=on)

[!DNL Sling Model Exporter] 是 [!DNL Apache Sling] 專案，而非直接系結至AEM產品發行週期。 [!DNL Sling Model Exporter] 與AEM 6.3和更新版本相容。

## 的使用案例 [!DNL Sling Model Exporter]

>[!VIDEO](https://video.tv.adobe.com/v/16863/?quality=12&learn=on)

[!DNL Sling Model Exporter] 最適合使用已包含商業邏輯的Sling模型，可透過HTL（或先前的JSP）支援HTML轉譯，並公開與JSON相同的商業表示法，以便透過程式化網站服務或JavaScript應用程式消耗。

## 建立Sling模型匯出工具

>[!VIDEO](https://video.tv.adobe.com/v/16864/?quality=12&learn=on)

啟用 [!DNL Exporter] 在 [!DNL Sling Model] 和添加 `@Exporter` Java類的注釋。

## 套用Sling模型匯出工具選項

>[!VIDEO](https://video.tv.adobe.com/v/16865/?quality=12&learn=on)

[!DNL Sling Model Exporter] 支援將每個模型導出器選項傳遞到導出器實施，以驅動如何 [!DNL Sling Model] 最後匯出。 這些選項通常適用於 [!DNL Sling Model] 會匯出，而不是每個資料點，您可透過下方說明的內嵌註解來完成。

[!DNL Jackson Exporter] 選項包括：

* [映射器功能選項](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [序列化功能選項](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

## 套用 [!DNL Jackson] 註解

>[!VIDEO](https://video.tv.adobe.com/v/16866/?quality=12&learn=on)

匯出工具實施也可支援可內嵌套用至 [!DNL Sling Model] 類，可提供更精細的資料匯出控制。

* [[!DNL Jackson Exporter] 附註](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)

## 檢視程式碼 {#view-the-code}

[SampleSlingModelExporter.java](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleSlingModelExporter.java)

## 支援材料 {#supporting-materials}

* [[!DNL Jackson Mapper] 功能Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [[!DNL Jackson Serialization] 功能Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

* [[!DNL Jackson Annotations] 文件](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)
