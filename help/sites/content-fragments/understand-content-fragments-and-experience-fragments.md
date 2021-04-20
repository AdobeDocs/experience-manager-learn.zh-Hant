---
title: 瞭解內容片段和體驗片段
description: Adobe Experience Manager的內容片段和體驗片段在表面上看似類似，但在不同的使用案例中，每個片段都扮演關鍵角色。 瞭解內容片段和體驗片段的相似性、不同性，以及各片段的使用時機和方式。
sub-product: 資產，網站，內容服務
feature: Content Fragments, Experience Fragments
topics: headless
version: 6.3, 6.4, 6.5
doc-type: article
activity: understand
audience: all
topic: Content Management
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1007'
ht-degree: 1%

---


# 瞭解內容片段和體驗片段

Adobe Experience Manager的內容片段和體驗片段在表面上看似類似，但在不同的使用案例中，每個片段都扮演關鍵角色。 瞭解內容片段和體驗片段的相似性、不同性，以及各片段的使用時機和方式。

## 內容片段與體驗片段比較

<table>
<tbody><tr><td><strong> </strong></td>
<td><strong>內容片段(CF)</strong></td>
<td><strong>體驗片段(XF)</strong></td>
</tr><tr><td><strong>定義</strong></td>
<td><ul>
<li>可重複使用、不受表現形式限制的<strong>內容</strong>，由結構化資料元素（文字、日期、參考等）組成</li>
</ul>
</td>
<td><ul>
<li>可重複使用的、組合的一AEM個或多個元件，定義內容和呈現，形成自行有意義的<strong>體驗</strong></li>
</ul>
</td>
</tr><tr><td><strong>核心租客</strong></td>
<td><ul>
<li>內容導向</li>
<li>由<a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-models.html" target="_blank">結構化、基於表單的資料模型定義。</a></li>
<li>不受設計和版面限制。</li>
<li>頻道擁有內容片段內容（版面和設計）的簡報</li>
</ul>
</td>
<td><ul>
<li>以簡報為中心</li>
<li>由元件的非結構化組AEM成定義</li>
<li>定義內容的設計和版面</li>
<li>在頻道中使用「原樣」</li>
</ul>
</td>
</tr><tr><td><strong>技術詳細資訊</strong></td>
<td><ul>
<li>實作為<strong>dam:Asset</strong></li>
<li>由<a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-models.html" target="_blank">內容片段模型</a>定義</li>
</ul>
</td>
<td><ul>
<li>實作為<strong>cq:Page</strong></li>
<li>由可編輯範本定義</li>
<li>原生HTML轉譯</li>
</ul>
</td>
</tr><tr><td><strong>變數</strong></td>
<td><ul>
<li>主變分是正則變分</li>
<li>變化是特定使用案例，可能會與通道對齊。</li>
</ul>
</td>
<td><ul>
<li>變化是頻道或內容特定</li>
<li>變數會透過即時副本保AEM持同步</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html#BuildingBlocks" target="_blank">建立</a> 區塊式內容，以便跨變數重複使用</li>
</ul>
</td>
</tr><tr><td><strong>功能</strong></td>
<td><ul>
<li>變數</li>
<li>版本</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-variations.html#SynchronizingwithMaster" target="_blank">跨變</a> 化同步內容</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-managing.html#ComparingFragmentVersions" target="_blank">內容</a> 片段版本的視覺差異</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-variations.html#AnnotatingaContentFragment" target="_blank">多</a> 行文字元素附註</li>
<li>智慧型多行文字元素<a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-variations.html#SummarizingText" target="_blank">總結</a>。</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/creating-translation-projects-for-content-fragments.html" target="_blank">翻譯／本地化</a></li>
</ul>
</td>
<td><ul>
<li>變數</li>
<li>變化為即時副本</li>
<li>版本</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html#BuildingBlocks" target="_blank">建置區塊</a></li>
<li>註解</li>
<li>互動式版面配置與預覽</li>
<li>翻譯／本地化</li>
</ul>
</td>
</tr><tr><td><strong>使用</strong></td>
<td><ul>
<li><a href="https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-component.html" target="_blank">核心元AEM件內容片段元</a> 件，用於AEM Sites、AEM Screens或體驗片段。</li>
<li>透過<a href="https://helpx.adobe.com/experience-manager/kt/sites/using/content-services-tutorial-use.html" target="_blank">Content ServicesAEM</a>匯出JSON，以用於第三方使用</li>
<li>透過HTTPAEM資產API的JSON，以供第三方使用。</li>
</ul>
</td>
<td><ul>
<li>體驗AEM片段元件，用於AEM Sites、AEM Screens或其他體驗片段。</li>
<li>匯出為<a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html#ThePlainHTMLRendition" target="_blank">Plain HTML</a>，供第三方系統使用</li>
<li><a href="https://helpx.adobe.com/tw/experience-manager/6-5/sites/administering/using/experience-fragments-target.html" target="_blank">HTML匯出至Adobe定</a> 位以取得定位選件</li>
<li>JSON匯出至Adobe Target以取得定位選件</li>
</ul>
</td>
</tr><tr><td><strong>常見使用案例</strong></td>
<td><ul>
<li>高度結構化的資料輸入／表單內容</li>
<li>長篇編輯內容（多行元素）</li>
<li>在提供內容的通道生命週期之外管理的內容</li>
</ul>
</td>
<td><ul>
<li>使用每個通道的變化集中管理多通道宣傳資料。</li>
<li>在網站的多個頁面間重複使用內容。</li>
<li>網站色彩(例如 頁首和頁尾)</li>
<li>在通道生命週期外管理的體驗，提供</li>
</ul>
</td>
</tr><tr><td><strong>文件</strong></td>
<td><ul>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/user-guide.html?topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js" target="_blank">內AEM容片段使用指南</a></li>
<li><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-feature-video-use.html" target="_blank">使用內容片段AEM</a></li>
</ul>
</td>
<td><ul>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html" target="_blank">Adobe體驗片段檔案</a></li>
</ul>
</td>
</tr></tbody></table>

## 內容片段架構

下圖說明內容片段的整AEM體架構

!![內容片段架構](./assets/content-fragments-architecture.png)

+ **內容片** 段模型定義定義內容片段可擷取和公開的內容的元素（或欄位）。
+ **內容片段**&#x200B;是代表邏輯內容實體的內容片段模型的實例。
+ 內容片段&#x200B;**valuations**&#x200B;符合內容片段模型，但內容有變化。
+ 內容片段可由下列人員公開／使用：
   + 透過WCM核心元件的內容片段元件，在&#x200B;**AEM Sites**(或AEM ScreensAEM)上使用內容片段。
   + 透過AEMWCM核心元件的內容片段元件將內容片段內嵌在&#x200B;**體驗片段**&#x200B;中，以用於任何體驗片段使用案例。
   + 透過&#x200B;**Content ServicesAEM**&#x200B;和API頁面，將內容片段變化內容公開為JSON，以供唯讀使用案例。
   + 透過直接呼叫透過&#x200B;**AEM AssetsHTTP API**&#x200B;的CRUD使用案例，直接將內容片段內容（所有變化）公開為JSON。

## 體驗片段架構

!![體驗片段架構](./assets/experience-fragments-architecture.png)

+ **可編輯的範本**(依序由可編輯的範本類型和 **AEM頁面元件實作定義)**  ****，定義可AEM用來合成體驗片段的允許元件。
+ **體驗片段**&#x200B;是可編輯範本的例項，代表邏輯體驗。
+ 體驗片段&#x200B;**valuations**&#x200B;符合可編輯範本，但體驗（內容和設計）有變化。
+ 體驗片段可由下列人員公開／使用：
   + 透過Experience Fragment元件在AEM Sites(或AEM Screens)上使AEM用Experience Fragments。
   + 透過&#x200B;**內容服務**&#x200B;和API頁面，將體驗片段變化內容公開為JSON(含內嵌AEM HTML)。
   + 直接將體驗片段變數曝光為&#x200B;**「Plain HTML」**。
   + 將體驗片段匯出至&#x200B;**Adobe Target**&#x200B;為HTML或JSON選件。
   + AEM Sites原本支援HTML選件，但是JSON選件需要自訂開發。

## 內容片段的支援資料

+ [內容片段使用指南](https://helpx.adobe.com/experience-manager/6-5/assets/user-guide.html?topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js)
+ [使用內容片段AEM](https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-feature-video-use.html)
+ [AEMWCM核心元件的內容片段元件](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-component.html)
+ [使用內容片段AEM和內容服務](https://helpx.adobe.com/experience-manager/kt/sites/using/structured-fragments-content-services-feature-video-use.html)
+ [內容服務AEM快速入門](https://helpx.adobe.com/experience-manager/kt/sites/using/content-services-tutorial-use.html)

## 體驗片段的支援資料

+ [Adobe體驗片段檔案](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html)
+ [瞭解AEM體驗片段](https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-understand.html)
+ [使用AEM體驗片段](https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-use.html)
+ [將體驗AEM片段與Adobe Target](https://medium.com/adobetech/experience-fragments-and-adobe-target-d8d74381b9b2)
