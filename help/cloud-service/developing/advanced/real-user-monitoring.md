---
title: Real User Monitoring (RUM) — 真實使用者監控(RUM)
description: 瞭解AEMas a Cloud Service網站中的Real User Monitoring (RUM)。
version: Cloud Service
feature: Operations
topic: Performance
role: Admin, Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 0
jira: KT-11861
thumbnail: KT-11861.png
last-substantial-update: 2024-03-18T00:00:00Z
source-git-commit: 7c80bb25b79a77c4a0bb2bbedf8a7c338177b857
workflow-type: tm+mt
source-wordcount: '647'
ht-degree: 0%

---


# Real User Monitoring (RUM) — 真實使用者監控(RUM)

瞭解AEMas a Cloud Service網站中的Real User Monitoring (RUM)。 瞭解如何啟用RUM、會收集哪些資料以及如何使用RUM資料來最佳化網站上的使用者體驗。

## 概觀

Real User Monitoring (RUM)是一種用於 _收集、測量和分析使用者互動和體驗_ 與網站即時互動。 它提供網站訪客如何與您的網站互動的深入分析，包括他們的行為、效能和整體體驗。 這是透過將小段JavaScript程式碼插入網站頁面中來達成。

RUM使用JavaScript程式碼在使用者的瀏覽器與您的網站互動時直接擷取資料。 此資料可用於識別和診斷效能問題、最佳化使用者體驗，以及改善業務成果。

AEMas a Cloud Service中的RUM功能可全面檢視您網站的使用者體驗。 它會擷取使用者造訪的每個頁面(URL)的下列關鍵量度：

- [最大內容繪製(LCP)](https://web.dev/articles/lcp)  — 測量載入效能。
- [累計版面配置位移(CLS)](https://web.dev/articles/cls)  — 測量視覺穩定性。
- [下一幅繪畫的互動(INP)](https://web.dev/articles/inp)  — 測量互動性。
- 頁面檢視 — 測量頁面被檢視的次數。

此外，也能擷取網站的404個錯誤和頁面檢檢視表。

LCP、CLS和INP量度屬於 [核心Web重要功能](https://web.dev/articles/vitals) 這是一組與網站速度、回應速度及視覺穩定性相關的量度。 Google會使用這些量度來測量網站上的使用者體驗，這些量度對搜尋引擎排名很重要。

## 啟用RUM

若要為您的AEMCS網站啟用RUM，請參閱 [如何設定真實使用者監視服務](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/content-requests#how-to-set-up-the-rum-service).

AEMCS中RUM的主要詳細資訊如下：

- RUM僅適用於AEMCS的發佈服務，這表示JavaScript程式碼只會插入在發佈環境中。
- 此 `com.adobe.granite.webvitals.WebVitalsConfig` OSGi設定可控制包含和排除路徑，這些是存放庫路徑，而非URL路徑。
- 預設 `/content` 包括路徑。
- 若要排除路徑，請新增 `AEM_WEBVITALS_EXCLUDE` Cloud Manager環境變數，請參閱 [新增環境變數](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables#add-variables). 路徑以逗號分隔。
- OOTB程式碼負責將JavaScript程式碼插入頁面中。

### 驗證

若要確認您的網站是否已啟用RUM，請檢視已發佈頁面的HTML來源，並搜尋下列指令碼區塊：

```html
...

<!-- Added before the closing </head> tag -->
<script type="module">
    window.RUM_BASE = 'https://rum.hlx.page/';
    import { sampleRUM } from 'https://rum.hlx.page/.rum/@adobe/helix-rum-js@^1/src/index.js';
    sampleRUM('top');
    window.addEventListener('load', () => sampleRUM('load'));
    document.addEventListener('click', () => sampleRUM('click'));
</script>

...

<!-- Added before the closing </body> tag -->
<script type="module">
    window.RUM_BASE = 'https://rum.hlx.page/';
    import { sampleRUM } from 'https://rum.hlx.page/.rum/@adobe/helix-rum-js@^1/src/index.js';
    sampleRUM('lazy');
    sampleRUM('cwv');
</script>
```

## RUM資料彙集

- RUM資料的收集方式： `sampleRUM()` 函式中，方法是傳遞查核點名稱。 在上述範例中，查核點為 `top`， `load`， `click`， `lazy`、和 `cwv`.
- 查核點是載入頁面並與頁面互動的序列中的已命名事件。

另請參閱 [Real User Monitoring Service與隱私權](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/content-requests#rum-service-and-privacy) 和 [正在收集哪些資料](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/content-requests#what-data-is-being-collected) 以取得更多詳細資料。

## RUM資料檢視

若要檢視您所需的RUM資料 `domainkey`，Adobe會將其提供為RUM設定的一部分。 RUM控制面板位於 [https://data.aem.live/](https://data.aem.live/) 而且您可以使用網域金鑰和url來存取它。

例如，底下熒幕擷圖顯示AEM WKND網站的RUM控制面板。

![RUM控制面板](./assets/rum/RUM-Dashboard-WKND.png)

RUM儀表板提供下列重要深入分析：

- **績效計量** - LCP、CLS、INP和Pageviews。
- **錯誤量度** - 404錯誤。
- **Pageview圖表**  — 一段時間內的頁面檢視次數。

## 如何使用RUM資料

使用上述深入分析，您可以最佳化網站上的使用者體驗。 例如：

- 減少LCP、CLS和INP，以改善頁面載入效能和互動性。 另請參閱 [如何改善LCP](https://web.dev/articles/lcp#improve-lcp)， [如何改善CLS](https://web.dev/articles/cls#improve-cls) 和 [如何改善INP](https://web.dev/articles/inp#improve-inp)以取得更多詳細資料。
- 若要改善使用者體驗，請修正404錯誤。
- 若要瞭解使用者行為並最佳化內容，請分析頁面檢檢視表。

Adobe建議定期審查RUM儀表板，尤其是在主要或次要版本之後。

另請參閱 [哪些人可以受益於Real使用者監控服務](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/content-requests#who-can-benefit-from-rum-service) 以取得更多詳細資料。
