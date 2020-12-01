---
title: 瞭解AEM中的Sling Model Exporter
description: Apache Sling Models 1.3.0推出Sling Model Exporter，這是將Sling Model物件匯出或序列化為自訂抽象化的優雅方式。 本文並列使用Sling Models填入HTL指令碼的傳統使用案例，以及運用Sling Model Exporter架構將Sling Model序列化為JSON。
version: 6.3, 6.4, 6.5
sub-product: 基礎，內容服務
feature: sling-models, sling-model-exporter
topics: development, content-delivery, headless
activity: understand
audience: developer, architect
doc-type: article
translation-type: tm+mt
source-git-commit: 63295cbc353a796959ba2e98e3e21c188f596372
workflow-type: tm+mt
source-wordcount: '571'
ht-degree: 0%

---


# 瞭解[!DNL Sling Model Exporter]

Apache [!DNL Sling Models] 1.3.0引入了[!DNL Sling Model Exporter]，這是一種將[!DNL Sling Model]對象導出或序列化到自定義抽象中的優雅方法。 本文將使用[!DNL Sling Models]填入HTL指令碼的傳統使用案例與運用[!DNL Sling Model Exporter]架構將[!DNL Sling Model]序列化為JSON的使用案例並列。

## 傳統Sling Model HTTP請求流程

[!DNL Sling Models]的傳統使用案例是為資源或請求提供業務抽象，為HTL指令碼（或先前的JSP）提供訪問業務功能的介面。

常見的模式是開發[!DNL Sling Models]，以表示AEM元件或頁面，並使用[!DNL Sling Model]物件以顯示在瀏覽器中的HTML結果為HTL指令碼提供資料。

### Sling Model HTTP Request Flow

![Sling Model Request Flow](./assets/understand-sling-model-exporter/sling-model-request-flow.png)

1. [!DNL HTTP GET] 會在AEM中請求資源。

   範例: `HTTP GET /content/my-resource.html`

1. 根據請求資源的`sling:resourceType`，可解決適當的指令碼。

1. 指令碼會將請求或資源調整至所需的[!DNL Sling Model]。

1. Script使用[!DNL Sling Model]物件來產生HTML轉譯。

1. 指令碼產生的HTML會在HTTP回應中傳回。

這種傳統模式在產生HTML時很有效，因為[!DNL Sling Model]可透過HTL輕鬆運用。 建立更結構化的資料（例如JSON或XML）則較為麻煩，因為HTL不會自然符合這些格式的定義。

## [!DNL Sling Model Exporter] HTTP請求流程

Apache [!DNL Sling Model Exporter]隨附Sling提供的Jackson Exporter，可自動將&quot;ordinal&quot; [!DNL Sling Model]物件序列化為JSON。 Jackson匯出器雖然可設定性相當強，但其核心會檢查[!DNL Sling Model]物件，並使用任何&quot;getter&quot;方法產生JSON金鑰，而getter傳回值則產生JSON值。

直接序列化[!DNL Sling Models]可讓他們使用使用傳統[!DNL Sling Model]請求流程建立的HTML回應來服務一般Web請求（請參閱上文），同時也公開Web服務或JavaScript應用程式可使用的JSON轉譯。

![Sling Model Exporter HTTP請求流程](./assets/understand-sling-model-exporter/sling-model-exporter-request-flow.png)

*此流程說明使用提供的Jackson匯出器來產生JSON輸出的流程。使用自訂出口商會遵循相同的流程，但會遵循其輸出格式。*

1. AEM中的資源會提出HTTP GET請求，其選擇器和副檔名已註冊至[!DNL Sling Model]的匯出器。

   範例: `HTTP GET /content/my-resource.model.json`

1. Sling會解析所請求資源的`sling:resourceType`、選擇器和擴充功能，以動態產生的Sling Exporter Servlet，此Servlet會對應至具有Exporter的[!DNL Sling Model]。
1. 已解析的Sling Exporter Servlet會針對從請求或資源（由Sling Models適配表決定）調整的[!DNL Sling Model]物件來叫用[!DNL Sling Model Exporter]。
1. 匯出器會根據匯出器選項和匯出器特定Sling模型註解序列化[!DNL Sling Model]，並將結果傳回至Sling匯出器Servlet。
1. Sling Exporter Servlet會傳回HTTP回應中[!DNL Sling Model]的JSON轉譯。

>[!NOTE]
>
>雖然Apache Sling專案提供將[!DNL Sling Models]序列化為JSON的Jackson Exporter，但Exporter架構也支援自訂的Exporter。 例如，專案可實作自訂匯出器，將[!DNL Sling Model]序列化為XML。

>[!NOTE]
>
>[!DNL Sling Model Exporter] *序列化* [!DNL Sling Models]不僅可以將它們匯出為Java物件。 匯出至其他Java物件在HTTP請求流程中不會發揮作用，因此不會出現在上圖中。

## 支援材料

* [ [!DNL Sling Model Exporter] ApacheFramework文檔](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
