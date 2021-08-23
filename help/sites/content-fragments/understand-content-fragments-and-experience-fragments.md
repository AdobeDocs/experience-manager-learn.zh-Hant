---
title: 了解內容片段和體驗片段
description: Adobe Experience Manager的內容片段和體驗片段在表面上看起來可能類似，但每個片段在不同使用案例中都扮演關鍵角色。 了解內容片段和體驗片段如何相似、不同，以及各自的使用時機和方式。
sub-product: 資產，網站，內容服務
feature: 內容片段、體驗片段
topics: headless
version: 6.3, 6.4, 6.5
doc-type: article
activity: understand
audience: all
topic: 內容管理
role: User
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '1000'
ht-degree: 1%

---


# 了解內容片段和體驗片段

Adobe Experience Manager的內容片段和體驗片段在表面上看起來可能類似，但每個片段在不同使用案例中都扮演關鍵角色。 了解內容片段和體驗片段如何相似、不同，以及各自的使用時機和方式。

## 內容片段和體驗片段比較

<table>
<tbody><tr><td><strong> </strong></td>
<td><strong>內容片段(CF)</strong></td>
<td><strong>體驗片段(XF)</strong></td>
</tr><tr><td><strong>定義</strong></td>
<td><ul>
<li>可重複使用、不受簡報限制的<strong>content</strong>，由結構化資料元素（文字、日期、參考等）組成</li>
</ul>
</td>
<td><ul>
<li>可重複使用的、複合的一或多個AEM元件，定義內容和呈現，形成<strong>experience</strong>，其本身有意義</li>
</ul>
</td>
</tr><tr><td><strong>核心租戶</strong></td>
<td><ul>
<li>以內容為中心</li>
<li>由<a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-models.html" target="_blank">結構化、表單式資料模型定義。</a></li>
<li>不受設計和佈局限制。</li>
<li>頻道擁有內容片段內容的簡報（版面和設計）</li>
</ul>
</td>
<td><ul>
<li>以演示文稿為中心</li>
<li>由AEM元件的非結構化組合定義</li>
<li>定義內容的設計和佈局</li>
<li>在管道中以「原樣」使用</li>
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
<li>由可編輯的範本定義</li>
<li>原生HTML轉譯</li>
</ul>
</td>
</tr><tr><td><strong>變數</strong></td>
<td><ul>
<li>主變異是標準變異</li>
<li>變數是特定的使用案例，可能會與通道一致。</li>
</ul>
</td>
<td><ul>
<li>變數是管道或內容特定的</li>
<li>變數會透過AEM Live Copy保持同步</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html#BuildingBlocks" target="_blank">建立</a> 跨變數重複使用的封鎖低內容</li>
</ul>
</td>
</tr><tr><td><strong>功能</strong></td>
<td><ul>
<li>變數</li>
<li>版本</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-variations.html#SynchronizingwithMaster" target="_blank"></a> 同步不同變數的內容</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-managing.html#ComparingFragmentVersions" target="_blank">內容</a> 片段版本的視覺差異</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-variations.html#AnnotatingaContentFragment" target="_blank"></a> 多行文本元素的註解</li>
<li>多行文本元素的智慧型<a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-variations.html#SummarizingText" target="_blank">總結</a>。</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/creating-translation-projects-for-content-fragments.html" target="_blank">翻譯/本地化</a></li>
</ul>
</td>
<td><ul>
<li>變數</li>
<li>即時副本形式的變數</li>
<li>版本</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html#BuildingBlocks" target="_blank">建置區塊</a></li>
<li>註解</li>
<li>回應式版面和預覽</li>
<li>翻譯/本地化</li>
</ul>
</td>
</tr><tr><td><strong>使用</strong></td>
<td><ul>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html" target="_blank">AEM核心元件內容片</a> 段元件，以用於AEM Sites、AEM Screens或體驗片段。</li>
<li>透過<a href="https://helpx.adobe.com/experience-manager/kt/sites/using/content-services-tutorial-use.html" target="_blank">AEM Content Services</a>匯出第三方使用的JSON</li>
<li>透過AEM HTTP Assets API執行JSON作業，以供第三方使用。</li>
</ul>
</td>
<td><ul>
<li>AEM體驗片段元件，以用於AEM Sites、AEM Screens或其他體驗片段。</li>
<li>導出為<a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html#ThePlainHTMLRendition" target="_blank">純HTML</a>，供第三方系統使用</li>
<li><a href="https://helpx.adobe.com/tw/experience-manager/6-5/sites/administering/using/experience-fragments-target.html" target="_blank">HTML匯出至Adobe目標</a> ，以鎖定目標選件</li>
<li>針對目標選件將JSON匯出至Adobe Target</li>
</ul>
</td>
</tr><tr><td><strong>常見使用案例</strong></td>
<td><ul>
<li>高度結構化的資料輸入/表單式內容</li>
<li>長格式編輯內容（多行元素）</li>
<li>在提供內容的管道生命週期之外管理的內容</li>
</ul>
</td>
<td><ul>
<li>使用每個渠道的變化集中管理多渠道促銷宣傳資料。</li>
<li>內容在網站的多個頁面上重複使用。</li>
<li>網站Chrome(例如 頁首與頁尾)</li>
<li>在提供體驗的管道生命週期之外管理的體驗</li>
</ul>
</td>
</tr><tr><td><strong>文件</strong></td>
<td><ul>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/user-guide.html?topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js" target="_blank">AEM內容片段使用手冊</a></li>
<li><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-feature-video-use.html" target="_blank">在AEM中使用內容片段</a></li>
</ul>
</td>
<td><ul>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html" target="_blank">Adobe體驗片段的檔案</a></li>
</ul>
</td>
</tr></tbody></table>

## 內容片段架構

下圖說明AEM內容片段的整體架構

!![內容片段架構](./assets/content-fragments-architecture.png)

+ **內容片** 段模型定義元素（或欄位），定義內容片段可擷取和公開的內容。
+ **內容片段**&#x200B;是代表邏輯內容實體的內容片段模型的例項。
+ 內容片段&#x200B;**變異**&#x200B;符合內容片段模型，但內容有變異。
+ 內容片段可由下列項目公開/使用：
   + 透過AEM WCM核心元件的內容片段元件，在&#x200B;**AEM Sites**(或AEM Screens)上使用內容片段。
   + 透過AEM WCM核心元件的內容片段元件將內容片段內嵌在&#x200B;**體驗片段**&#x200B;中，以用於任何體驗片段使用案例。
   + 針對唯讀使用案例，透過&#x200B;**AEM內容服務**&#x200B;和API頁面，將內容片段變體內容公開為JSON。
   + 針對CRUD使用案例，透過&#x200B;**AEM Assets HTTP API**&#x200B;直接呼叫AEM Assets，將內容片段內容（所有變體）公開為JSON。

## 體驗片段架構

!![體驗片段架構](./assets/experience-fragments-architecture.png)

+ **可編輯範本**(又由可編輯的范 **本類** 型和AEM頁面元 **件實作定義)**&#x200B;定義允許的AEM元件，以用於撰寫體驗片段。
+ **體驗片段**&#x200B;是可編輯範本的例項，代表邏輯體驗。
+ 體驗片段&#x200B;**variations**&#x200B;會遵循可編輯的範本，但體驗（內容和設計）中有變數。
+ 體驗片段可由下列項目公開/使用：
   + 透過AEM體驗片段元件在AEM Sites(或AEM Screens)上使用體驗片段。
   + 透過&#x200B;**AEM內容服務**&#x200B;和API頁面，將體驗片段變體內容公開為JSON（含內嵌HTML）。
   + 直接將體驗片段變異顯示為&#x200B;**&quot;Plain HTML&quot;**。
   + 將體驗片段匯出為&#x200B;**Adobe Target**，作為HTML或JSON選件。
   + AEM Sites原生支援HTML選件，但是JSON選件需要自訂開發。

## 內容片段的支援資料

+ [內容片段使用手冊](https://helpx.adobe.com/experience-manager/6-5/assets/user-guide.html?topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js)
+ [在AEM中使用內容片段](https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-feature-video-use.html)
+ [AEM WCM核心元件的內容片段元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html)
+ [使用內容片段和AEM內容服務](https://helpx.adobe.com/experience-manager/kt/sites/using/structured-fragments-content-services-feature-video-use.html)
+ [AEM Content Services快速入門](https://helpx.adobe.com/experience-manager/kt/sites/using/content-services-tutorial-use.html)

## 體驗片段的支援資料

+ [Adobe體驗片段的檔案](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html)
+ [了解AEM體驗片段](https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-understand.html)
+ [使用AEM體驗片段](https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-use.html)
+ [搭配使用AEM體驗片段和Adobe Target](https://medium.com/adobetech/experience-fragments-and-adobe-target-d8d74381b9b2)
