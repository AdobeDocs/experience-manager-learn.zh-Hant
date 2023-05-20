---
title: 瞭解Sling模型導出AEM器
description: Apache Sling Models 1.3.0引入了Sling Model Exporter，這是一種將Sling Model對象導出或序列化為自定義抽象的優雅方法。 本文將使用Sling Models填充HTL指令碼的傳統使用情形與利用Sling Model Exporter框架將Sling Model序列化為JSON並列。
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
topics: development, content-delivery, headless
activity: understand
audience: developer, architect
doc-type: article
topic: Development
role: Developer
level: Beginner
exl-id: 03cdf5d1-3253-44c9-ae1f-ec5d3c562427
source-git-commit: 2f02a4e202390434de831ce1547001b2cef01562
workflow-type: tm+mt
source-wordcount: '569'
ht-degree: 0%

---

# 瞭解 [!DNL Sling Model Exporter]

阿帕奇 [!DNL Sling Models] 1.3.0介紹 [!DNL Sling Model Exporter]，導出或序列化的優雅方式 [!DNL Sling Model] 對象到自定義抽象。 將傳統的使用案例並列 [!DNL Sling Models] 填充HTL指令碼，並 [!DNL Sling Model Exporter] 序列化框架 [!DNL Sling Model] JSON。

## 傳統Sling模型HTTP請求流

傳統的用例 [!DNL Sling Models] 是為資源或請求提供業務抽象，它為HTL指令碼（或以前的JSP）提供訪問業務功能的介面。

共同模式正在發展 [!DNL Sling Models] 表示AEM元件或頁面，並使用 [!DNL Sling Model] 對象，以向HTL指令碼提供資料，並在瀏覽器中顯示HTML的結束結果。

### Sling模型HTTP請求流

![吊具模型請求流](./assets/understand-sling-model-exporter/sling-model-request-flow.png)

1. [!DNL HTTP GET] 在中請求資源AEM。

   範例: `HTTP GET /content/my-resource.html`

1. 基於請求資源的 `sling:resourceType`，解析相應的指令碼。

1. 指令碼將使請求或資源適應所需 [!DNL Sling Model]。

1. 指令碼使用 [!DNL Sling Model] 對象，以生成HTML格式副本。

1. 指令碼生成的HTML在HTTP響應中返回。

這種傳統模式在以HTML為生成 [!DNL Sling Model] 可通過HTL輕鬆利用。 建立JSON或XML等更結構化的資料是一項繁瑣得多的工作，因為HTL不會自然而然地遵循這些格式的定義。

## [!DNL Sling Model Exporter] HTTP請求流

阿帕奇 [!DNL Sling Model Exporter] 附帶Sling,Jackson Exporter自動序列化「普通」 [!DNL Sling Model] 對象到JSON。 傑克森出口商雖然可以配置，但其核心是檢查 [!DNL Sling Model] 對象，並使用任何「getter」方法作為JSON鍵生成JSON，而getter返回值作為JSON值。

直接系列化 [!DNL Sling Models] 允許他們使用使用傳統Web伺服器建立的HTML響應來服務正常Web請求 [!DNL Sling Model] 請求流（請參見上文），但也會公開Web服務或JavaScript應用程式可使用的JSON格式副本。

![Sling模型導出器HTTP請求流](./assets/understand-sling-model-exporter/sling-model-exporter-request-flow.png)

*此流描述使用提供的Jackson導出器生成JSON輸出的流。 使用自定義導出程式遵循相同的流，但其輸出格式相同。*

1. 對選擇器中的資源發出HTTPGET請AEM求，並向註冊 [!DNL Sling Model]出口商。

   範例: `HTTP GET /content/my-resource.model.json`

1. Sling解析請求的資源 `sling:resourceType`，選擇器和對動態生成的Sling導出器Servlet的擴展，該Sling導出器Servlet映射到 [!DNL Sling Model] 和出口商。
1. 解析的Sling導出器Servlet調用 [!DNL Sling Model Exporter] 與 [!DNL Sling Model] 根據請求或資源（由Sling Models適配器確定）調整的對象。
1. 導出程式序列化 [!DNL Sling Model] 基於導出器選項和導出器特定的Sling模型注釋，並將結果返回到Sling導出器Servlet。
1. Sling導出器Servlet返回的JSON格式副本 [!DNL Sling Model] 的子菜單。

>[!NOTE]
>
>而Apache Sling項目則提供Jackson導出器，可序列化 [!DNL Sling Models] 到JSON時，導出器框架還支援自定義導出器。 例如，項目可以實施對 [!DNL Sling Model] 到XML中。

>[!NOTE]
>
>不僅如此 [!DNL Sling Model Exporter] *序列化* [!DNL Sling Models]，也可以將其導出為Java對象。 導出到其他Java對象在HTTP請求流中不起作用，因此不會出現在上圖中。

## 支撐材料

* [阿帕奇 [!DNL Sling Model Exporter] 框架文檔](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
