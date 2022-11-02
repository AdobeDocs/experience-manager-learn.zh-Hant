---
title: 了解AEM中的Sling模型匯出工具
description: Apache Sling Models 1.3.0導入了Sling Model Exporter，這是將Sling Model物件匯出或序列化為自訂抽象化的典雅方式。 本文並列使用Sling模型填入HTL指令碼的傳統使用案例，並搭配使用Sling模型匯出器架構將Sling模型序列化為JSON。
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

# 了解 [!DNL Sling Model Exporter]

Apache [!DNL Sling Models] 1.3.0導入 [!DNL Sling Model Exporter]，匯出或序列化的優雅方式 [!DNL Sling Model] 對象到自定義抽象化。 本文將傳統的使用案例並列 [!DNL Sling Models] 以運用 [!DNL Sling Model Exporter] 序列化框架 [!DNL Sling Model] 轉換為JSON。

## 傳統Sling模型HTTP要求流程

的傳統使用案例 [!DNL Sling Models] 是為資源或請求提供業務抽象，為HTL指令碼（或先前的JSP）提供用於存取業務功能的介面。

共同模式正在發展 [!DNL Sling Models] 代表AEM元件或頁面，並使用 [!DNL Sling Model] 物件可讓HTL指令碼內含資料，且瀏覽器中會顯示HTML的結束結果。

### Sling模型HTTP要求流程

![Sling模型要求流程](./assets/understand-sling-model-exporter/sling-model-request-flow.png)

1. [!DNL HTTP GET] 會在AEM中要求資源。

   範例: `HTTP GET /content/my-resource.html`

1. 根據請求資源的 `sling:resourceType`，則會解析適當的指令碼。

1. 指令碼會根據需要調整請求或資源 [!DNL Sling Model].

1. 指令碼會使用 [!DNL Sling Model] 物件，以產生HTML轉譯。

1. 指令碼產生的HTML會在HTTP回應中傳回。

這種傳統模式在產生HTML為 [!DNL Sling Model] 可透過HTL輕鬆運用。 建立JSON或XML等結構化資料較為繁瑣，因為HTL不會自然借鑒於這些格式的定義。

## [!DNL Sling Model Exporter] HTTP要求流程

Apache [!DNL Sling Model Exporter] 隨附Sling提供的Jackson Exporter，會自動序列化「普通」 [!DNL Sling Model] 物件放入JSON。 傑克森出口商雖然可以很好地配置，但其核心是檢查 [!DNL Sling Model] 物件，並使用任何「getter」方法作為JSON索引鍵來產生JSON，而getter會傳回值作為JSON值。

直接序列化 [!DNL Sling Models] 可讓使用者透過使用傳統 [!DNL Sling Model] 要求流程（請參閱上文），但也會公開網站服務或JavaScript應用程式可使用的JSON轉譯。

![Sling模型匯出器HTTP要求流程](./assets/understand-sling-model-exporter/sling-model-exporter-request-flow.png)

*此流程說明使用提供的Jackson Exporter產生JSON輸出的流程。 使用自訂匯出工具會遵循相同的流程，但會遵循其輸出格式。*

1. 會針對AEM中的資源提出HTTPGET要求，且選取器和擴充功能已向 [!DNL Sling Model]出口商。

   範例: `HTTP GET /content/my-resource.model.json`

1. Sling會解析請求的資源的 `sling:resourceType`、選取器和擴充功能，以動態產生Sling Exporter Servlet，並對應至 [!DNL Sling Model] 和出口商。
1. 已解析的Sling匯出工具Servlet會叫用 [!DNL Sling Model Exporter] 與 [!DNL Sling Model] 根據要求或資源調整的物件（由Sling模型適配表決定）。
1. 導出器將 [!DNL Sling Model] 根據匯出工具選項和匯出工具專用的Sling模型註解，並將結果傳回至Sling匯出工具Servlet。
1. Sling Exporter Servlet會傳回的JSON轉譯 [!DNL Sling Model] 在HTTP回應中。

>[!NOTE]
>
>而Apache Sling專案則提供可序列化的Jackson Exporter [!DNL Sling Models] 匯出架構若設為JSON，也支援自訂匯出工具。 例如，專案可實作自訂匯出工具，序列化 [!DNL Sling Model] XML中。

>[!NOTE]
>
>不僅如此 [!DNL Sling Model Exporter] *序列化* [!DNL Sling Models]，也可以將其匯出為Java物件。 匯出至其他Java物件在HTTP要求流程中不會發揮作用，因此不會出現在上圖中。

## 支援材料

* [Apache [!DNL Sling Model Exporter] 框架文檔](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
