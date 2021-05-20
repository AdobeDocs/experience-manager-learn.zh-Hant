---
title: 了解AEM中的Sling模型匯出工具
description: Apache Sling Models 1.3.0導入了Sling Model Exporter，這是將Sling Model物件匯出或序列化為自訂抽象化的典雅方式。 本文並列使用Sling模型填入HTL指令碼的傳統使用案例，並搭配使用Sling模型匯出器架構將Sling模型序列化為JSON。
version: 6.3, 6.4, 6.5
sub-product: 基礎，內容服務
feature: API
topics: development, content-delivery, headless
activity: understand
audience: developer, architect
doc-type: article
topic: 開發
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '573'
ht-degree: 0%

---


# 了解[!DNL Sling Model Exporter]

Apache [!DNL Sling Models] 1.3.0引入了[!DNL Sling Model Exporter]，這是一種將[!DNL Sling Model]對象導出或序列化為自定義抽象化的典雅方法。 本文並列使用[!DNL Sling Models]填入HTL指令碼的傳統使用案例，並運用[!DNL Sling Model Exporter]架構將[!DNL Sling Model]序列化為JSON。

## 傳統Sling模型HTTP要求流程

[!DNL Sling Models]的傳統使用案例是為資源或請求提供業務抽象，為HTL指令碼（或先前的JSP）提供用於訪問業務函式的介面。

常見的模式是開發[!DNL Sling Models]，以表示AEM元件或頁面，並使用[!DNL Sling Model]物件以資料饋送HTL指令碼，而瀏覽器中會顯示的HTML結尾結果。

### Sling模型HTTP要求流程

![Sling模型要求流程](./assets/understand-sling-model-exporter/sling-model-request-flow.png)

1. [!DNL HTTP GET] 會在AEM中要求資源。

   範例: `HTTP GET /content/my-resource.html`

1. 會根據請求資源的`sling:resourceType`解析適當的指令碼。

1. 指令碼會將請求或資源調整到所需的[!DNL Sling Model]。

1. 指令碼使用[!DNL Sling Model]物件來產生HTML轉譯。

1. 指令碼產生的HTML會在HTTP回應中傳回。

此傳統模式在產生HTML時非常有效，因為[!DNL Sling Model]可透過HTL輕鬆運用。 建立JSON或XML等結構化資料較為繁瑣，因為HTL不會自然借鑒於這些格式的定義。

## [!DNL Sling Model Exporter] HTTP要求流程

Apache [!DNL Sling Model Exporter]隨附Sling提供的Jackson Exporter，可自動將&quot;ordinal&quot; [!DNL Sling Model]物件序列化為JSON。 Jackson導出器雖然可配置性相當強，但其核心會檢查[!DNL Sling Model]對象，並使用任何&quot;getter&quot;方法作為JSON鍵，使用getter返回值作為JSON值生成JSON。

[!DNL Sling Models]的直接序列化可讓使用傳統[!DNL Sling Model]請求流程建立的HTML回應來服務一般Web請求（請參閱上文），但也會公開網站服務或JavaScript應用程式可使用的JSON轉譯。

![Sling模型匯出器HTTP要求流程](./assets/understand-sling-model-exporter/sling-model-exporter-request-flow.png)

*此流程說明使用提供的Jackson Exporter產生JSON輸出的流程。使用自訂匯出工具會遵循相同的流程，但會使用其輸出格式。*

1. 對AEM中的資源提出HTTPGET請求，該資源具有已向[!DNL Sling Model]的導出器註冊的選擇器和擴展。

   範例: `HTTP GET /content/my-resource.model.json`

1. Sling會將請求的資源的`sling:resourceType`、選取器和擴充功能解析為動態產生的Sling Exporter Servlet,Sling Exporter Servlet會與Exporter對應至[!DNL Sling Model]。
1. 解析的Sling導出器Servlet針對從請求或資源調用的[!DNL Sling Model]對象調用[!DNL Sling Model Exporter]（由Sling模型適配器確定）。
1. 導出器會根據導出器選項和導出器特定的Sling模型注釋序列化[!DNL Sling Model]，並將結果返回到Sling導出器Servlet。
1. Sling匯出工具Servlet會傳回HTTP回應中[!DNL Sling Model]的JSON轉譯。

>[!NOTE]
>
>雖然Apache Sling專案提供將[!DNL Sling Models]序列化為JSON的Jackson匯出工具，但匯出工具架構也支援自訂匯出工具。 例如，項目可以實施將[!DNL Sling Model]序列化為XML的自定義導出器。

>[!NOTE]
>
>[!DNL Sling Model Exporter] *序列化* [!DNL Sling Models]不僅如此，它還可以將它們導出為Java對象。 匯出至其他Java物件在HTTP要求流程中不會發揮作用，因此不會出現在上圖中。

## 支援材料

* [ [!DNL Sling Model Exporter] ApacheFramework檔案](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
