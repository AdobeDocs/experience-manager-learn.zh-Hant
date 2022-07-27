---
title: 為SPAGraphQL部AEM署
description: 了SPA解有關GraphQL和無AEM頭的部署選項。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10587
thumbnail: KT-10587.jpg
mini-toc-levels: 2
source-git-commit: c7d2e69a9039cfbaa43d6d9b65b9fa6f69378716
workflow-type: tm+mt
source-wordcount: '528'
ht-degree: 0%

---


# 部署SPA

在本節中，我們將回顧一種部署SPA(React 、 Vue 、Angular等)的方法，該方法調用AEMGraphQL API來載入資料，但是，在此之前，我們先瞭解必須部署的高級對象。

**生成SPA應用對象：**

框架生成的文SPA件，通常 **HTML、CSS、JS**，亦即靜態生成對象。 對於React應用， `build` 目錄和 `dist` 的子菜單。
將使用這SPA些生成對象向您(例如https://HOST/my-aem-spa.html)發送請求。

**AEMGraphQL API:**

顯然，此GraphQL API終結點(`/graphql/execute.json/<PROJECT-CONFIG>/<PERSISTED-QUERY-NAME>`)必須在域上托AEM管。

總之，部SPA署體系結構有兩部分 *1。 SPA2 AEMGraphQL API層*，下面讓我們查看這兩個部分的部署選項。


## 部署選項

| 部署選項 | URLSPA | AEMGraphQL API URL | 需要CORS配置？ |
| ---------|---------- | ---------|---------- |
| **相同域** | https://**主機**/my-aem-spa.html | https://**主機**/graphql/execute.json/... | ✘ |
| **不同域** | https://**主SPA機**/my-aem-spa.html | https://**主AEM機**/graphql/execute.json/... | ✔ |

**相同域：**\
兩者 *SPA和AEMGraphQL API層* 將此選項部署到 **相同域**。 正在向SPAURI請求 `/my-aem-spa.html` &amp; GraphQL API層 `/graphql/execute.json/` 都來自同一個域。

**不同域：**\
兩者 *SPA和AEMGraphQL API層* 在此選項中， **不同域**。 正在向SPAURI請求 `/my-aem-spa.html` 由 **不同域** 比GraphQL API層 `/graphql/execute.json/` 請求。 請注意，作為此部署選項的一部分，您需要 [配置CORS](cors.md) 在實AEM例上。

>[!NOTE]
>
>必須正確配置實AEM例的CORS [在此處查看步驟](cors.md)。

### 在同一域上部署

在同一域上部署時，該域可能是 **主域** (在AEM域上)或 **主域** (關AEM閉域)，並且SPA可以在任AEM一部署元件（如CDN）上拆分傳入的GraphQL API請求([快速](https://docs.fastly.com/en/guides/routing-assets-to-different-origins)，阿卡邁， [雲前](https://aws.amazon.com/premiumsupport/knowledge-center/cloudfront-distribution-serve-content/)) [帶反向代理的HTTPD](https://httpd.apache.org/docs/2.4/howto/reverse_proxy.html)。 換句話說，您仍然會將生成對象SPA和AEMGraphQL API部署到不同的伺服器，但對於最終用戶，這些對象是從單個域傳送的，並在路由到不同目標或源伺服器的場景後面傳送的。

此外，可SPA以在中承載生成工AEM件 *不推薦。*

| 相同域部署 | CDN剝離 | HTTPD +反向代理 | 托AEM管SPA對象 |
| ---------|---------- | ---------|---------- |
| **ON域AEM** | ✔ | ✔ | ✔ |
| **關AEM閉域** | ✔ | ✔ | **N/A** |


**HTTPD +反向代理**

配置示例如下所示

>[!TIP]
>
> 以下配置是示例。 確保根據項目要求調整它們。

域AEM上

    「」
    ProxyPass &quot;/${YOUR-SPAURI}&quot; &quot;http://${SPA-HOST}/&quot;
    ProxyPassReverse &quot;/${YOUR-SPAURI}&quot; &quot;http://${SPA-HOST}/&quot;
    「」

關AEM閉域

    「」
    ProxyPass &quot;/graphql/execute.json/&quot; &quot;http://${AEM-HOST}/&quot;
    ProxyPassReverse &quot;/graphql/execute.json/&quot; &quot;http://${AEM-HOST}/&quot;
    「」




### 在不同域上部署

在此方案SPA中，生成對象部署到與AEMGraphQL API域不同的域，並且對於最終用戶，它們將從兩個獨立的域中傳遞 [CORS配置](cors.md) 必須開啟AEM。

**通SPA過不同域的應用請求**

![不同域交SPA付](assets/spa/different-domain-spa-delivery.png)


**GraphQL API上的CORS響AEM應頭**

![CORS響應頭AEMGraphQL API](assets/spa/CORS-response-header-aem-graphql-api.png)


