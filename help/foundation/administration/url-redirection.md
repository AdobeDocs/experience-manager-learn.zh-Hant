---
title: URL重新導向
description: 了解在AEM中執行URL重新導向的各種選項。
version: 6.4, 6.5, Cloud Service
topic: Development, Administration
feature: Operations, Dispatcher
role: Developer, Architect
level: Intermediate
kt: 11466
last-substantial-update: 2022-10-14T00:00:00Z
index: y
source-git-commit: d5645e975aa290392348cc69d078b24921a7d13a
workflow-type: tm+mt
source-wordcount: '816'
ht-degree: 1%

---


# URL重新導向

URL重新導向是網站作業的常見方面。 架構師和管理員面臨挑戰，無法找到最佳解決方案來管理URL重新導向，提供彈性並縮短重新導向部署時間。

請務必熟悉 [AEM 6.x)](https://experienceleague.adobe.com/docs/experience-manager-learn/dispatcher-tutorial/chapter-2.html#the-%E2%80%9Clegacy%E2%80%9D-setup) 和 [AEMas a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/architecture.html#runtime-architecture) 基礎設施。 主要差異為：

1.  AEMas a Cloud Service [內建CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html)不過，客戶可以在AEM管理的CDN之前提供CDN(BYOCDN)。
1.  AEM 6.x(不論是內部部署或Adobe Managed Services(AMS))皆不包含AEM管理的CDN，且客戶必須自行安裝。

AEM 6.x和AEMas a Cloud Service的其他AEM服務（AEM Author/Publish和Dispatcher）在其他概念上類似。

AEM URL重新導向解決方案如下：

|  | 以AEM專案程式碼管理並部署 | 可依行銷/內容團隊進行變更 | AEM asCloud Service相容 | 重新導向執行發生的位置 |
|---------------------------------------------------|:-----------------------:|:---------------------:|:---------------------:| :---------------------:|
| [透過自攜CDN在Edge](#at-edge-via-bring-your-own-cdn) | ✘ | ✘ | ✔ | Edge/CDN |
| [Apache `mod_rewrite` 規則作為Dispatcher設定 ](#apache-mod_rewrite-module) | ✔ | ✘ | ✔ | Dispatcher |
| [ACS Commons — 重定向映射管理器](#redirect-map-manager) | ✘ | ✔ | ✘ | Dispatcher |
| [ACS Commons — 重定向管理器](#redirect-manager) | ✘ | ✔ | ✔ | AEM |


## 解決方案選項

以下是解決方案選項，依順序排列為更接近網站訪客的瀏覽器。

### 透過自攜CDN在Edge

有些CDN服務會在Edge層級提供重新導向解決方案，因此減少往返來源的次數。 請參閱 [Akamai邊緣重新導向程式](https://techdocs.akamai.com/cloudlets/docs/what-edge-redirector), [AWS CloudFront函式](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cloudfront-functions.html). 請洽詢您的CDN服務供應商，以取得邊緣層級重新導向功能。

在Edge或CDN層級管理重新導向有其效能優勢，但這些重新導向並非AEM的一部分，而是獨立專案。 管理和部署重新導向規則的周全程式對於避免問題至關重要。


### Apache `mod_rewrite` 模組

常見的解決方案使用 [Apache模組mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html). 此 [AEM專案原型](https://github.com/adobe/aem-project-archetype) 為兩者提供dispatcher專案結構 [AEM 6.x](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.ams#file-structure) 和 [AEMas a Cloud Service](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud#file-structure) 專案。 預設（不可變）和自訂重寫規則定義於 `conf.d/rewrites` 資料夾和重寫引擎已開啟 `virtualhosts` 在埠上監聽 `80` via `conf.d/dispatcher_vhost.conf` 檔案。 範例實作可在 [AEM WKND Sites專案](https://github.com/adobe/aem-guides-wknd/tree/main/dispatcher/src/conf.d/rewrites).

在AEMas a Cloud Service中，這些重新導向規則是作為AEM程式碼的一部分進行管理，並透過Cloud Manager部署 [Web層配置管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html#web-tier-config-pipelines) 或 [全堆疊管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html#full-stack-pipeline). 因此，您的AEM專屬程式將可供管理、部署及追蹤重新導向規則。

大部分的CDN服務會根據其重新導向的快取，快取HTTP 301和302重新導向 `Cache-Control` 或 `Expires` 標題。 這有助於避免在Apache/Dispatcher起始的初始重新導向後往返。


### ACS AEM公域

內有兩種功能 [ACS AEM公域](https://adobe-consulting-services.github.io/acs-aem-commons/) 來管理URL重新導向。 請注意，ACS AEM Commons是由社區管理的開放原始碼專案，不受Adobe支援。

#### 重新導向地圖管理器

[重新導向地圖管理器](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-map-manager/index.html) 可讓AEM 6.x管理員輕鬆維護和發佈 [Apache RewriteMap](https://httpd.apache.org/docs/2.4/rewrite/rewritemap.html) 檔案，而不需要直接存取Apache Web伺服器，或需要重新啟動Apache Web伺服器。 此功能可讓權限使用者在AEM的主控台中建立、更新和刪除重新導向規則，而無需開發團隊或AEM部署的協助。 重新導向地圖管理器為 **不相容AEMas a Cloud Service**.

#### 重新導向管理器

[重新導向管理器](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/index.html) 可讓AEM中的使用者輕鬆維護和發佈AEM的重新導向。 該實現基於Java™ servlet過濾器，具有典型的JVM資源消耗。 此功能也消除了對AEM開發團隊和AEM部署的相依性。 重新導向管理器同時 **AEMas a Cloud Service** 和 **AEM 6.x** 相容。 請注意，雖然初始重新導向的請求必須由AEM Publish服務產生301/302，但（大部分）CDN快取301/302預設會允許在Edge/CDN重新導向後續的請求。


## 哪個解決方案適合實作

以下是幾個判斷正確解決方案的條件。 此外，貴組織的IT和行銷程式應該有助於挑選合適的解決方案。

1. 讓行銷團隊或超級使用者無需AEM開發團隊和AEM發行、部署週期便可管理重新導向規則。
1. 驗證、跟蹤和恢復更改或降低風險的過程。
1. 可用性 _主題專業知識_ for **透過CDN服務在Edge** 解決方案。

