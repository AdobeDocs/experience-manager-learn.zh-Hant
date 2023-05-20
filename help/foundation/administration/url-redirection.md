---
title: URL重定向
description: 瞭解在中執行URL重定向的各種選AEM項。
version: 6.4, 6.5, Cloud Service
topic: Development, Administration
feature: Operations, Dispatcher
role: Developer, Architect
level: Intermediate
kt: 11466
last-substantial-update: 2022-10-14T00:00:00Z
index: y
exl-id: 8e64f251-e5fd-4add-880e-9d54f8e501a6
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '876'
ht-degree: 1%

---

# URL重定向

URL重定向是網站操作的一個常見方面。 架構師和管理員必須找到最佳解決方案，以瞭解如何和在何處管理URL重定向，從而提供靈活性和快速重定向部署時間。

確保您熟悉 [(AEM6.x)ak AEM Classic](https://experienceleague.adobe.com/docs/experience-manager-learn/dispatcher-tutorial/chapter-2.html#the-%E2%80%9Clegacy%E2%80%9D-setup) 和 [AEMas a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/architecture.html#runtime-architecture) 基礎設施。 主要區別是：

1. AEMas a Cloud Service [內置CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html)但是，客戶可以在托管CDN之前提AEM供CDN(BYOCDN)。
1. AEM6.x是本地服務還是Adobe托管服務(AMS)都不包括托管AEM的CDN，客戶必須自己提供。

其他AEM服務（AEM Author/Publish和Dispatcher）在6.x和as a Cloud Service之間在概念上相AEM似AEM。

AEMURL重定向解決方案如下：

|  | 作為項目代碼進行AEM管理和部署 | 能夠按營銷/內容團隊進行更改 | AEM與Cloud Service相容 | 執行重定向的位置 |
|---------------------------------------------------|:-----------------------:|:---------------------:|:---------------------:| :---------------------:|
| [在Edge上，提供您自己的CDN](#at-edge-via-bring-your-own-cdn) | ✘ | ✘ | ✔ | 邊緣/CDN |
| [阿帕奇 `mod_rewrite` 規則作為Dispatcher配置 ](#apache-mod_rewrite-module) | ✔ | ✘ | ✔ | Dispatcher |
| [ACS共用 — 重定向映射管理器](#redirect-map-manager) | ✘ | ✔ | ✘ | Dispatcher |
| [ACS共用 — 重定向管理器](#redirect-manager) | ✘ | ✔ | ✔ | AEM |
| [的 `Redirect` 頁屬性](#the-redirect-page-property) | ✘ | ✔ | ✔ | AEM |


## 解決方案選項

以下是按離網站訪問者瀏覽器更近的順序排列的解決方案選項。

### 在Edge上，提供您自己的CDN

一些CDN服務在邊緣級別提供重定向解決方案，從而減少往返源。 請參閱 [Akamai邊緣重定向器](https://techdocs.akamai.com/cloudlets/docs/what-edge-redirector)。 [AWSCloudFront函式](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cloudfront-functions.html)。 請咨詢您的CDN服務提供商，瞭解邊緣級別重定向功能。

在邊緣或CDN級別管理重定向具有效能優勢，但它們不是作為離散項目的一部分AEM進行管理的。 管理和部署重定向規則的深思熟慮的過程對於避免問題至關重要。


### 阿帕奇 `mod_rewrite` 模組

通用解決方案使用 [Apache模組mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html)。 的 [項AEM目原型](https://github.com/adobe/aem-project-archetype) 為兩者提供Dispatcher項目結構 [AEM 6.x](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.ams#file-structure) 和 [AEMas a Cloud Service](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud#file-structure) 項目。 預設（不可變）和自定義重寫規則在 `conf.d/rewrites` 資料夾和重寫引擎已開啟 `virtualhosts` 在埠上監聽 `80` 通過 `conf.d/dispatcher_vhost.conf` 的子菜單。 在中提供了示例實現 [WKNDAEM站點項目](https://github.com/adobe/aem-guides-wknd/tree/main/dispatcher/src/conf.d/rewrites)。

在AEMas a Cloud Service中，這些重定向規則作為代碼的一部分AEM進行管理並通過雲管理器部署 [Web層配置管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html#web-tier-config-pipelines) 或 [全堆棧管線](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html#full-stack-pipeline)。 因此，AEM您的項目特定流程可以管理、部署和跟蹤重定向規則。

大多數CDN服務會根據HTTP 301和302重定向快取它們 `Cache-Control` 或 `Expires` 標題。 這有助於避免在初始重定向源於Apache/Dispatcher後進行往返。


### ACS公AEM域

有兩種功能可在 [ACS公AEM域](https://adobe-consulting-services.github.io/acs-aem-commons/) 管理URL重定向。 請注意，ACS公AEM域是社區運營的開源項目，不受Adobe支援。

#### 重定向映射管理器

[重定向映射管理器](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-map-manager/index.html) 允許AEM6.x管理員輕鬆維護和發佈 [Apache重寫映射](https://httpd.apache.org/docs/2.4/rewrite/rewritemap.html) 不直接訪問Apache Web伺服器或需要重新啟動Apache Web伺服器的檔案。 此功能允許用戶從中的控制台建立、更新和刪除重定向規AEM則，而無需開發團隊或部署的AEM幫助。 重定向映射管理器為 **不AEMas a Cloud Service**。

#### 重定向管理器

[重定向管理器](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/index.html) 允許中的用AEM戶輕鬆維護和發佈重定向AEM。 該實現基於Java™ Servlet過濾器，因此JVM資源消耗是典型的。 此功能還消除了對開發團隊AEM和部署的依AEM賴性。 重定向管理器是 **AEMas a Cloud Service** 和 **AEM 6.x** 相容。 雖然初始重定向請求必須命中AEM發佈服務才能生成301/302（大多數）CDN的快取301/302，但預設情況下，允許後續請求在邊緣/CDN重定向。

### 的 `Redirect` 頁屬性

出廠設定(OOTB) `Redirect` 頁屬性 [「高級」頁籤](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/fundamentals/page-properties.html#advanced) 允許內容作者為當前頁面定義重定向位置。 此解決方案最適合每頁重定向方案，並且沒有用於查看和管理頁面重定向的中心位置。

## 哪種解決方案適合實施

以下是確定正確解決方案的幾個標準。 此外，您組織的IT和營銷流程應有助於選擇正確的解決方案。

1. 使營銷團隊或超級用戶無需開發團隊和部AEM署即可管理重AEM定向規則。
1. 管理、驗證、跟蹤和還原更改或降低風險的流程。
1. 可用性 _主題專業知識_ 為 **通過CDN服務在邊緣** 解決方案。
