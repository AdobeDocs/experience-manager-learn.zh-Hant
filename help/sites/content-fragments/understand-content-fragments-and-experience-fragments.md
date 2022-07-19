---
title: 瞭解內容片段和體驗片段
description: 瞭解內容片段和體驗片段之間的異同，以及使用每種類型的時間和方式。
sub-product: assets, sites, content services
feature: Content Fragments, Experience Fragments
topics: headless
version: 6.3, 6.4, 6.5
doc-type: article
activity: understand
audience: all
topic: Content Management
role: User
level: Beginner
exl-id: ccbc68d1-a83e-4092-9a49-53c56c14483e
source-git-commit: fb4a39a7b057ca39bc4cd4a7bce02216c3eb634c
workflow-type: tm+mt
source-wordcount: '1016'
ht-degree: 1%

---

# 瞭解內容片段和體驗片段

Adobe Experience Manager的內容片段和經驗片段在表面上看似相似，但在不同的使用情形中，每個片段都起著關鍵作用。 瞭解內容片段和體驗片段是如何相似、不同的，以及何時以及如何使用它們。

## 內容片段與體驗片段比較

<table>
<tbody><tr><td><strong> </strong></td>
<td><strong>內容片段(CF)</strong></td>
<td><strong>體驗片段(XF)</strong></td>
</tr><tr><td><strong>定義</strong></td>
<td><ul>
<li>可重用，不限演示形式 <strong>內容</strong>，由結構化資料元素（文本、日期、引用等）組成</li>
</ul>
</td>
<td><ul>
<li>一個或多個元件的可重用組合，AEM這些元件定義了構成 <strong>體驗</strong> 這本身就是合理的</li>
</ul>
</td>
</tr><tr><td><strong>核心租戶</strong></td>
<td><ul>
<li>以內容為中心</li>
<li>由 <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-models.html?lang=en" target="_blank">結構化、基於表單、資料模型。</a></li>
<li>設計和佈局不可知。</li>
<li>該頻道負責內容片段內容（佈局和設計）的演示</li>
</ul>
</td>
<td><ul>
<li>以演示文稿為中心</li>
<li>由元件的非結構化組合定AEM義</li>
<li>定義內容的設計和佈局</li>
<li>在通道中使用「原樣」</li>
</ul>
</td>
</tr><tr><td><strong>技術詳細資訊</strong></td>
<td><ul>
<li>作為 <strong>大壩：資產</strong></li>
<li>由 <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-models.html?lang=en" target="_blank">內容片段模型</a></li>
</ul>
</td>
<td><ul>
<li>作為 <strong>cq：頁</strong></li>
<li>由可編輯模板定義</li>
<li>本機HTML格式副本</li>
</ul>
</td>
</tr><tr><td><strong>變數</strong></td>
<td><ul>
<li>主變數是正則變數</li>
<li>變體是特定於用例的，它們可與通道對齊。</li>
</ul>
</td>
<td><ul>
<li>變體是特定於渠道或上下文的</li>
<li>變體通過Live Copy保AEM持同步</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html" target="_blank">積木</a> 允許跨變體重新使用內容</li>
</ul>
</td>
</tr><tr><td><strong>功能</strong></td>
<td><ul>
<li>變數</li>
<li>版本</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#synchronizing-with-master" target="_blank">同步</a> 跨變體的內容</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-managing.html?lang=en#comparing-fragment-versions" target="_blank">可視差異</a> 內容片段版本</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#annotating-a-content-fragment" target="_blank">注釋</a> 多行文本元素</li>
<li>智慧 <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#summarizing-text" target="_blank">總結</a> 的子菜單。</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/creating-translation-projects-for-content-fragments.html?lang=en" target="_blank">翻譯/本地化</a></li>
</ul>
</td>
<td><ul>
<li>變數</li>
<li>作為即時拷貝的變體</li>
<li>版本</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en#building-blocks" target="_blank">建置區塊</a></li>
<li>註解</li>
<li>響應性佈局和預覽</li>
<li>翻譯/本地化</li>
</ul>
</td>
</tr><tr><td><strong>使用</strong></td>
<td><ul>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html" target="_blank">核心AEM元件內容片段元件</a> 用於AEM Sites、AEM Screens或經驗片段。</li>
<li>JSON導出(通過 <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview.html?lang=en" target="_blank">內AEM容服務</a> 用於第三方消費</li>
<li>通過HTTPAEM資產API進行JSON，用於第三方消耗。</li>
</ul>
</td>
<td><ul>
<li>用AEM於AEM Sites、AEM Screens或其他經驗片段的經驗片段元件。</li>
<li>導出為 <a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en" target="_blank">純HTML</a> 供第三方系統使用</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/integration/experience-fragments-target.html?lang=en" target="_blank">HTML對Adobe Target的出口</a> 針對目標產品</li>
<li>JSON導出到Adobe Target以獲得目標優惠</li>
</ul>
</td>
</tr><tr><td><strong>常見用例</strong></td>
<td><ul>
<li>高度結構化的基於資料輸入/表單的內容</li>
<li>長格式編輯內容（多行元素）</li>
<li>在渠道生命週期之外管理的內容</li>
</ul>
</td>
<td><ul>
<li>使用每渠道變體集中管理多渠道促銷宣傳資料。</li>
<li>在網站中的多個頁面中重新使用內容。</li>
<li>網站chrome(例如 頁眉和頁腳)</li>
<li>在渠道生命週期之外管理的體驗</li>
</ul>
</td>
</tr><tr><td><strong>文件</strong></td>
<td><ul>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/home.html?lang=en&amp;topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js" target="_blank">內AEM容片段使用手冊</a></li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/content-fragments-feature-video-use.html?lang=en" target="_blank">在中使用內容片AEM段</a></li>
</ul>
</td>
<td><ul>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en" target="_blank">Adobe經驗片段文檔</a></li>
</ul>
</td>
</tr></tbody></table>

## 內容片段體系結構

下圖說明了內容片段的總體AEM體系結構

!![內容片段體系結構](./assets/content-fragments-architecture.png)

+ **內容片段模型** 定義定義內容片段可捕獲和公開的內容的元素（或欄位）。
+ 的 **內容片段** 是表示邏輯內容實體的內容片段模型的實例。
+ 內容片段 **變化** 但是，遵守內容片段模型的內容有所不同。
+ 內容片段可以由以下人員公開/使用：
   + 在上使用內容片段 **AEM Sites** (或AEM Screens),AEM通過WCM核心元件的內容片段元件。
   + 將內容片段嵌入 **體驗片段** 通過AEMWCM核心元件的內容片段元件，用於任何經驗片段使用案例。
   + 通過以JSON形式公開內容片段變體內容 **內AEM容服務** 和API頁，用於只讀使用情形。
   + 通過直接調用到AEM Assets，將內容片段內容（所有變體）作為JSON直接公開 **AEM AssetsHTTP API** 用於CRUD使用案例。

## 體驗片段體系結構

!![體驗片段體系結構](./assets/experience-fragments-architecture.png)

+ **可編輯模板**，而定義者 **可編輯模板類型** 和 **AEM頁元件實現**，定義可用AEM於組成體驗片段的允許元件。
+ 的 **體驗片段** 是表示邏輯體驗的可編輯模板的實例。
+ 體驗片段 **變化** 但是，遵循可編輯模板在體驗（內容和設計）方面有所不同。
+ 體驗片段可由以下人員公開/使用：
   + 通過經驗片段元件在AEM Sites(或AEM Screens)上使AEM用經驗片段。
   + 以JSON(帶嵌入式HTML)形式通過 **內AEM容服務** 和API頁。
   + 直接將體驗片段變異 **&quot;純HTML&quot;**。
   + 將體驗片段導出到 **Adobe Target** HTML或JSON提供。
   + AEM Sites本機支援HTML產品，但JSON產品需要自定義開發。

## 內容片段的支援材料

+ [內容片段使用手冊](https://experienceleague.adobe.com/docs/experience-manager-65/assets/home.html?lang=en&amp;topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js)
+ [在中使用內容片AEM段](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/content-fragments-feature-video-use.html?lang=en)
+ [WCMAEM核心元件內容片段元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html)
+ [使用內容片段和AEM無頭](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/overview.html?lang=en)
+ [Content Services入門AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview.html?lang=en)

## 經驗片段的支撐材料

+ [Adobe經驗片段文檔](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en)
+ [瞭解AEM經驗片段](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)
+ [使用AEM經驗片段](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)
+ [使用AEM經驗片段與Adobe Target](https://medium.com/adobetech/experience-fragments-and-adobe-target-d8d74381b9b2)
