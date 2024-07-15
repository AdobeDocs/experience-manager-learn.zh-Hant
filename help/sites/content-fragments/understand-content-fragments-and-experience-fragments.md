---
title: 內容片段和體驗片段
description: 瞭解內容片段和體驗片段之間的異同，以及何時和如何使用每種型別。
sub-product: Experience Manager Assets, Experience Manager Sites
feature: Content Fragments, Experience Fragments
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Article
exl-id: ccbc68d1-a83e-4092-9a49-53c56c14483e
duration: 168
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '829'
ht-degree: 2%

---

# 內容片段和體驗片段

Adobe Experience Manager的內容片段和體驗片段表面上看起來可能類似，但各自在不同的使用案例中會發揮關鍵作用。 瞭解內容片段和體驗片段如何相似、不同，以及何時及如何使用各片段。

## 比較

<table>
<tbody><tr><td><strong> </strong></td>
<td><strong>內容片段(CF)</strong></td>
<td><strong>體驗片段(XF)</strong></td>
</tr><tr><td><strong>定義</strong></td>
<td><ul>
<li>可重複使用、不受簡報影響的<strong>內容</strong>，由結構化資料元素（文字、日期、參考等）組成</li>
</ul>
</td>
<td><ul>
<li>由一或多個AEM元件所組成的可重複使用複合元件，定義形成<strong>體驗</strong>的內容和簡報，其本身就有意義</li>
</ul>
</td>
</tr><tr><td><strong>核心原則</strong></td>
<td><ul>
<li>以內容為中心</li>
<li>由<a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-models.html?lang=en" target="_blank">結構化、表單式資料模型所定義。</a></li>
<li>不受設計和版面限制。</li>
<li>此管道擁有內容片段內容的簡報（佈局和設計）</li>
</ul>
</td>
<td><ul>
<li>以簡報為中心</li>
<li>由AEM元件的非結構化構成所定義</li>
<li>定義內容的設計和配置</li>
<li>在管道中使用「原樣」</li>
</ul>
</td>
</tr><tr><td><strong>技術細節</strong></td>
<td><ul>
<li>已實作為<strong>dam：Asset</strong></li>
<li>由<a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-models.html?lang=en" target="_blank">內容片段模型</a>定義</li>
</ul>
</td>
<td><ul>
<li>實作為<strong>cq：Page</strong></li>
<li>由可編輯的範本定義</li>
<li>原生HTML轉譯</li>
</ul>
</td>
</tr><tr><td><strong>變化</strong></td>
<td><ul>
<li>主要變數是標準變數</li>
<li>變數因使用案例而異，可能會與管道一致。</li>
</ul>
</td>
<td><ul>
<li>變數與管道或內容有關</li>
<li>變數會透過AEM即時副本保持同步</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html" target="_blank">建置區塊</a>允許跨變數重複使用內容</li>
</ul>
</td>
</tr><tr><td><strong>功能</strong></td>
<td><ul>
<li>變化</li>
<li>版本</li>
<li>跨變數內容的<a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#synchronizing-with-master" target="_blank">同步處理</a></li>
<li>內容片段版本的<a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-managing.html?lang=en#comparing-fragment-versions" target="_blank">視覺差異</a></li>
<li>多行文字元素的<a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#annotating-a-content-fragment" target="_blank">註解</a></li>
<li>多行文字元素的智慧<a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#summarizing-text" target="_blank">摘要</a>。</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/creating-translation-projects-for-content-fragments.html?lang=en" target="_blank">翻譯/本地化</a></li>
</ul>
</td>
<td><ul>
<li>變化</li>
<li>作為即時副本的變數</li>
<li>版本</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en#building-blocks" target="_blank">建置區塊</a></li>
<li>註解</li>
<li>回應式佈局和預覽</li>
<li>翻譯/本地化</li>
<li>透過內容片段參考建立的複雜資料模型</li>
<li>應用程式內預覽</li>
</ul>
</td>
</tr><tr><td><strong>使用</strong></td>
<td><ul>
<li>透過<a href="https://experienceleague.adobe.com/landing/experience-manager/headless/developer.html?lang=zh-Hant">AEM Headless GraphQL API匯出JSON</a></li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html" target="_blank">AEM核心元件內容片段元件</a>，用於AEM Sites、AEM Screens或體驗片段。</li>
<li>透過<a href="https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview.html?lang=en" target="_blank">AEM Content Services</a>為第三方使用匯出JSON</li>
<li>JSON匯出至Adobe Target以取得鎖定目標的選件</li>
<li>透過適用於第三方消費的AEM HTTP Assets API使用JSON</li>
</ul>
</td>
<td><ul>
<li>用於AEM Sites、AEM Screens或其他體驗片段的AEM體驗片段元件。</li>
<li>匯出為<a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en" target="_blank">純HTML</a>以供第三方系統使用</li>
<li>針對目標優惠方案將<a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/integration/experience-fragments-target.html?lang=en" target="_blank">HTML匯出至Adobe Target</a></li>
<li>JSON匯出至Adobe Target以取得鎖定目標的選件</li>
</ul>
</td>
</tr><tr><td><strong>常見使用案例</strong></td>
<td><ul>
<li>推動GraphQL的Headless使用案例</li>
<li>結構化資料輸入/表單式內容</li>
<li>長式編輯內容（多行元素）</li>
<li>在提供內容的管道的生命週期之外管理的內容</li>
</ul>
</td>
<td><ul>
<li>使用每個管道的變數，集中管理多管道促銷附屬資料。</li>
<li>重複使用網站中多個頁面的內容。</li>
<li>網站顏色(例如： 頁首與頁尾)</li>
<li>在提供體驗的管道的生命週期之外管理的體驗</li>
</ul>
</td>
</tr><tr><td><strong>文件</strong></td>
<td><ul>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/home.html?lang=en&amp;topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js" target="_blank">AEM內容片段使用手冊</a></li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/content-fragments-feature-video-use.html?lang=en" target="_blank">在AEM中使用內容片段</a></li>
</ul>
</td>
<td><ul>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en" target="_blank">有關體驗片段的Adobe檔案</a></li>
</ul>
</td>
</tr></tbody></table>

## 內容片段架構

下圖說明AEM內容片段的整體架構

![內容片段架構](./assets/content-fragments-architecture.png)

+ **內容片段模型**&#x200B;定義元素（或欄位），這些元素會定義內容片段可擷取和公開的內容。
+ **內容片段**&#x200B;是代表邏輯內容實體的內容片段模型例項。
+ 內容片段&#x200B;**變數**&#x200B;會依循內容片段模式，但內容會有變數。
+ 以下人員可以公開/使用內容片段：
   + 透過AEM WCM核心元件的內容片段元件，在&#x200B;**AEM Sites** (或AEM Screens)上使用內容片段。
   + 使用AEM Headless GraphQL API使用Headless App中的&#x200B;**內容片段**。
   + 針對唯讀使用案例，透過&#x200B;**AEM Content Services**&#x200B;和API頁面將內容片段變數內容公開為JSON。
   + 針對CRUD使用案例，透過&#x200B;**AEM Assets HTTP API**&#x200B;直接呼叫AEM Assets，將內容片段內容（所有變數）以JSON形式直接公開。

## 體驗片段架構

![體驗片段架構](./assets/experience-fragments-architecture.png)

+ **可編輯的範本** (由&#x200B;**可編輯的範本型別**&#x200B;和&#x200B;**AEM Page元件實作**&#x200B;所定義)會定義可用來撰寫體驗片段的允許AEM元件。
+ **體驗片段**&#x200B;是代表邏輯體驗的可編輯範本的執行個體。
+ 體驗片段&#x200B;**變數**&#x200B;會依循可編輯的範本，但會有體驗（內容和設計）中的變數。
+ 體驗片段可以公開/使用對象：
   + 透過AEM體驗片段元件在AEM Sites (或AEM Screens)上使用體驗片段。
   + 透過&#x200B;**AEM Content Services**&#x200B;和API頁面將體驗片段變數內容公開為JSON (含內嵌HTML)。
   + 直接將體驗片段變數公開為&#x200B;**「純HTML」**。
   + 將體驗片段匯出至&#x200B;**Adobe Target**，做為HTML或JSON選件。
   + AEM Sites原生支援HTML選件，但JSON選件需要自訂開發。

## 內容片段的支援資源

+ [內容片段使用手冊](https://experienceleague.adobe.com/docs/experience-manager-65/assets/home.html?lang=en&amp;topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js)
+ [Adobe Experience Manager as a Headless CMS簡介](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/introduction.html?lang=zh-Hant)
+ [在AEM中使用內容片段](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/content-fragments-feature-video-use.html?lang=en)
+ [AEM WCM核心元件的內容片段元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html)
+ [使用內容片段和AEM Headless](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/overview.html?lang=en)
+ [AEM Content Services快速入門](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview.html?lang=en)

## 體驗片段的支援資源

+ [有關體驗片段的Adobe檔案](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en)
+ [瞭解AEM體驗片段](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)
+ [使用AEM體驗片段](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)
+ [搭配Adobe Target使用AEM體驗片段](https://medium.com/adobetech/experience-fragments-and-adobe-target-d8d74381b9b2)
