---
title: 瞭解AEM中的Sling模型匯出工具
description: Apache Sling Model 1.3.0引入了Sling模型匯出工具，這是一種將Sling模型物件匯出或序列化為自訂抽象的簡潔方式。 本文會比較使用Sling模型填入HTL指令碼的傳統使用案例，利用Sling模型匯出工具框架將Sling模型序列化為JSON。
version: Experience Manager 6.4, Experience Manager 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
doc-type: Article
topic: Development
role: Developer
level: Beginner
exl-id: 03cdf5d1-3253-44c9-ae1f-ec5d3c562427
duration: 133
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '568'
ht-degree: 0%

---

# 瞭解[!DNL Sling Model Exporter]

Apache [!DNL Sling Models] 1.3.0引入了[!DNL Sling Model Exporter]，這是一種將[!DNL Sling Model]物件匯出或序列化為自訂抽象的簡單方法。 本文會比較使用[!DNL Sling Models]填入HTL指令碼的傳統使用案例，利用[!DNL Sling Model Exporter]架構將[!DNL Sling Model]序列化為JSON。

## 傳統Sling模型HTTP請求流程

[!DNL Sling Models]的傳統使用案例是為資源或請求提供業務抽象，這提供HTL指令碼（或先前的JSP）用於存取業務功能的介面。

常見的模式是開發代表AEM元件或頁面的[!DNL Sling Models]，並使用[!DNL Sling Model]物件來向HTL指令碼提供資料，最後在瀏覽器中顯示HTML的結果。

### Sling模型HTTP請求流程

![Sling模型要求流程](./assets/understand-sling-model-exporter/sling-model-request-flow.png)

1. 在AEM中提出對資源的[!DNL HTTP GET]要求。

   範例：`HTTP GET /content/my-resource.html`

1. 根據要求資源的`sling:resourceType`，已解析適當的指令碼。

1. 指令碼會將請求或資源調整至所需的[!DNL Sling Model]。

1. 指令碼使用[!DNL Sling Model]物件來產生HTML轉譯。

1. 指令碼產生的HTML會在HTTP回應中傳回。

此傳統模式在產生HTML的情境中運作良好，因為可透過HTL輕鬆運用[!DNL Sling Model]。 建立更具結構化的資料（例如JSON或XML）是一項相當乏味的工作，因為HTL本身並不適合這些格式的定義。

## [!DNL Sling Model Exporter] HTTP要求流程

Apache [!DNL Sling Model Exporter]附帶Sling提供的Jackson Exporter，可自動將「一般」[!DNL Sling Model]物件序列化為JSON。 Jackson Exporter雖然相當可設定，但其核心會檢查[!DNL Sling Model]物件，並使用任何「getter」方法作為JSON金鑰產生JSON，並使用getter傳回值作為JSON值。

[!DNL Sling Models]的直接序列化可讓他們透過使用傳統[!DNL Sling Model]要求流程（請參閱上文）建立的HTML回應，為一般Web要求提供服務，同時也公開Web服務或JavaScript應用程式可使用的JSON轉譯。

![Sling模型匯出程式HTTP要求流程](./assets/understand-sling-model-exporter/sling-model-exporter-request-flow.png)

*此流程說明使用提供的Jackson Exporter產生JSON輸出的流程。 自訂匯出工具的使用遵循相同的流程，但採用其輸出格式。*

1. 已針對AEM中的資源提出HTTP GET要求，選取器和擴充功能已向[!DNL Sling Model]的匯出工具註冊。

   範例：`HTTP GET /content/my-resource.model.json`

1. Sling將請求資源的`sling:resourceType`、選擇器和擴充功能解析為動態產生的Sling匯出程式Servlet，該程式對應到具有匯出程式的[!DNL Sling Model]。
1. 已解析的Sling匯出工具Servlet會針對從請求或資源改寫的[!DNL Sling Model]物件叫用[!DNL Sling Model Exporter] （由Sling模型改寫決定）。
1. 匯出工具會根據匯出工具選項和匯出工具特定的Sling模型註解來序列化[!DNL Sling Model]，並將結果傳回Sling匯出工具Servlet。
1. Sling Exporter Servlet傳回HTTP回應中[!DNL Sling Model]的JSON轉譯。

>[!NOTE]
>
>雖然Apache Sling專案提供將[!DNL Sling Models]序列化為JSON的Jackson匯出工具，但匯出工具架構也支援自訂匯出工具。 例如，專案可以實作自訂匯出工具，將[!DNL Sling Model]序列化成XML。

>[!NOTE]
>
>不僅[!DNL Sling Model Exporter] *序列化* [!DNL Sling Models]，它還可以將它們匯出為Java物件。 匯出至其他Java物件在HTTP請求流程中不會扮演任何角色，因此不會出現在上圖中。

## 支援材料

* [Apache [!DNL Sling Model Exporter] 框架檔案](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
