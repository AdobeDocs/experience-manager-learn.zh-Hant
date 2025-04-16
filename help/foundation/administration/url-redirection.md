---
title: URL重新導向
description: 瞭解在AEM中執行URL重新導向的各種選項。
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Development, Administration
feature: Operations, Dispatcher
role: Developer, Architect
level: Intermediate
jira: KT-11466
last-substantial-update: 2024-10-22T00:00:00Z
index: y
doc-type: Article
exl-id: 8e64f251-e5fd-4add-880e-9d54f8e501a6
duration: 164
source-git-commit: 62887c6251b09ac22664cfeb9c5513363efb555e
workflow-type: tm+mt
source-wordcount: '961'
ht-degree: 0%

---

# URL重新導向

URL重新導向是網站作業中常見的方面。 架構師和管理員必須尋找最佳解決方案，瞭解如何及到哪裡管理URL重新導向，以提供彈性及快速的重新導向部署時間。

請務必熟悉[AEM (6.x) aka AEM Classic](https://experienceleague.adobe.com/en/docs/experience-manager-learn/dispatcher-tutorial/chapter-2)和[AEM as a Cloud Service](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/overview/architecture)基礎結構。 主要差異為：

1. AEM as a Cloud Service有[內建CDN](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn)，不過客戶可以在AEM管理的CDN前提供CDN (BYOCDN)。
1. AEM 6.x (無論內部部署或Adobe Managed Services (AMS)均不包含AEM管理的CDN，且客戶必須自備。

其他AEM服務(AEM製作/發佈和Dispatcher)在AEM 6.x和AEM as a Cloud Service之間的概念類似。

AEM的URL重新導向解決方案如下：

|                                                   | 管理並部署為AEM專案程式碼 | 可依行銷/內容團隊變更 | AEM as Cloud Service相容 | 發生重新導向執行的位置 |
|---------------------------------------------------|:-----------------------:|:---------------------:|:---------------------:| :---------------------:|
| [透過AEM管理的CDN在Edge](#at-edge-via-aem-managed-cdn) | ✔ | ✘ | ✔ | Edge/CDN （內建） |
| [在Edge，透過自備CDN (BYOCDN)](#at-edge-via-bring-your-own-cdn) | ✘ | ✘ | ✔ | Edge/CDN (BYOCDN) |
| [Apache `mod_rewrite`規則為Dispatcher設定](#apache-mod_rewrite-module) | ✔ | ✘ | ✔ | Dispatcher |
| [ACS Commons — 重新導向地圖管理員](#redirect-map-manager) | ✘ | ✔ | ✔ | Dispatcher |
| [ACS Commons — 重新導向管理員](#redirect-manager) | ✘ | ✔ | ✔ | AEM / DISPATCHER |
| [ `Redirect`頁面屬性](#the-redirect-page-property) | ✘ | ✔ | ✔ | AEM |


## 解決方案選項

以下是解決方案選項，依離網站訪客瀏覽器較近的順序排列。

### 透過AEM管理的CDN前往Edge {#at-edge-via-aem-managed-cdn}

此選項僅適用於AEM as a Cloud Service客戶。

[AEM管理的CDN](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn)提供Edge層級的重新導向解決方案，因此可減少到來源的來回次數。 [伺服器端重新導向](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#server-side-redirectors)功能可讓您在AEM專案程式碼中設定重新導向規則，並使用[設定管道](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager)進行部署。 CDN設定檔(`cdn.yaml`)大小不應超過100KB。

在Edge或CDN層級管理重新導向具有效能優勢。

### 在Edge，透過自備CDN

有些CDN服務提供了Edge層級的重新導向解決方案，因此減少了前往原點的往返次數。 請參閱[Akamai Edge重新導向程式](https://techdocs.akamai.com/cloudlets/docs/what-edge-redirector)、[AWS CloudFront函式](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cloudfront-functions.html)。 請洽詢您的CDN服務提供者，以瞭解Edge層級重新導向功能。

在Edge或CDN層級管理重新導向具有效能優勢，不過這些不受管理為AEM的一部分，而是分散式專案。 定義良好的流程來管理和部署重新導向規則對避免問題至關重要。


### Apache `mod_rewrite`模組

通用解決方案使用[Apache模組mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html)。 [AEM專案原型](https://github.com/adobe/aem-project-archetype)提供[Dispatcher 6.x](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.ams#file-structure)和[AEM as a Cloud Service](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud#file-structure)專案的AEM專案結構。 預設（不可變）和自訂重寫規則定義於`conf.d/rewrites`資料夾中，且透過`conf.d/dispatcher_vhost.conf`檔案在連線埠`80`上接聽的`virtualhosts`的重寫引擎已開啟。 [AEM WKND Sites專案](https://github.com/adobe/aem-guides-wknd/tree/main/dispatcher/src/conf.d/rewrites)中提供範例實作。

在AEM as a Cloud Service中，這些重新導向規則是作為AEM程式碼的一部分來管理，並透過Cloud Manager [網頁層設定管道](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines)或[完整棧疊管道](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines)來部署。 因此，您的AEM專案特定程式可用於管理、部署和追蹤重新導向規則。

大部分的CDN服務都會根據其`Cache-Control`或`Expires`標頭快取HTTP 301和302重新導向。 它有助於避免在Apache/Dispatcher中起始的初始重新導向後出現來回次數。


### ACS AEM公域

[ACS AEM Commons](https://adobe-consulting-services.github.io/acs-aem-commons/)中有兩項功能可用來管理URL重新導向。 請注意，ACS AEM Commons是社群運作的開放原始碼專案，不受Adobe支援。

#### 重新導向地圖管理員

[重新導向地圖管理員](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-map-manager/index.html)可協助AEM管理員輕鬆維護和發佈[Apache RewriteMap](https://httpd.apache.org/docs/2.4/rewrite/rewritemap.html)檔案，而不需要直接存取Apache Web伺服器或重新啟動Apache Web伺服器。 此功能可讓使用者從AEM中的主控台建立、更新和刪除重新導向規則，無需開發團隊或AEM部署協助。 重新導向地圖管理員同時是&#x200B;**AEM as a Cloud Service** （請參閱[可用的管道URL重新導向](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/pipeline-free-url-redirects)策略和相關的[教學課程](https://experienceleague.adobe.com/en/docs/experience-manager-learn/foundation/administration/implementing-pipeline-free-url-redirects#acs-commons---redirect-map-manager)）和&#x200B;**AEM 6.x**&#x200B;相容。

#### 重新導向管理員

[重新導向管理員](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/index.html)可讓AEM中的使用者輕鬆維護和發佈AEM中的重新導向。 實作是以Java™ servlet篩選器為基礎，因此是典型的JVM資源消耗。 此功能也會消除對AEM開發團隊和AEM部署的相依性。 重新導向管理員與&#x200B;**AEM as a Cloud Service**&#x200B;和&#x200B;**AEM 6.x**&#x200B;相容。 雖然初始的重新導向請求必須命中AEM Publish服務來產生301/302 （大多數） CDN的快取301/302 （依預設），以允許後續請求被重新導向到Edge/CDN。

[重新導向管理員](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/index.html)也支援&#x200B;**AEM as a Cloud Service**&#x200B;的[管線免除URL重新導向](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/pipeline-free-url-redirects)策略，方法是透過[將重新導向編譯成文字檔](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/subpages/rewritemap.html) （適用於[Apache RewriteMap](https://httpd.apache.org/docs/2.4/rewrite/rewritemap.html)），因此它允許更新Apache Web Server中使用的重新導向，而不需要直接存取它或重新啟動它。 如需詳細資訊，請參閱[教學課程](https://experienceleague.adobe.com/en/docs/experience-manager-learn/foundation/administration/implementing-pipeline-free-url-redirects#acs-commons---redirect-manager)。 在此案例中，初始重新導向要求點選Apache Web Server，而不是AEM Publish服務。

### `Redirect`頁面屬性

來自[進階索引標籤](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/sites-console/page-properties.html)的現成(OOTB) `Redirect`頁面屬性可讓內容作者定義目前頁面的重新導向位置。 此解決方案最適合每個頁面重新導向案例，且沒有可檢視及管理頁面重新導向的中央位置。

## 哪個解決方案適合實施

以下是一些判斷正確解決方案的條件。 此外，貴組織的IT和行銷流程應該有助於挑選正確的解決方案。

1. 讓行銷團隊或超級使用者在沒有AEM開發團隊和AEM部署的情況下管理重新導向規則。
1. 管理、驗證、追蹤及回覆變更或降低風險的程式。
1. 透過CDN服務&#x200B;**解決方案為**&#x200B;的Edge提供&#x200B;_主題專業知識_。
