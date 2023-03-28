---
title: 內容片段和體驗片段
description: 了解內容片段和體驗片段之間的相似度和差異，以及每種類型的使用時機和方式。
sub-product: Experience Manager Assets, Experience Manager Sites
feature: Content Fragments, Experience Fragments
topics: headless
version: 6.4, 6.5
doc-type: article
activity: understand
audience: all
topic: Content Management
role: User
level: Beginner
exl-id: ccbc68d1-a83e-4092-9a49-53c56c14483e
source-git-commit: 84fdbaa173a929ae7467aecd031cacc4ce73538a
workflow-type: tm+mt
source-wordcount: '1044'
ht-degree: 5%

---

# 內容片段和體驗片段

Adobe Experience Manager的內容片段和體驗片段在表面上看起來可能類似，但每個片段在不同使用案例中都扮演關鍵角色。 了解內容片段和體驗片段如何相似、不同，以及各自的使用時機和方式。

## 比較

<table>
<tbody><tr><td><strong> </strong></td>
<td><strong>內容片段(CF)</strong></td>
<td><strong>體驗片段(XF)</strong></td>
</tr><tr><td><strong>定義</strong></td>
<td><ul>
<li>可重複使用、不受演示文稿限制 <strong>內容</strong>，由結構化資料元素（文字、日期、參考等）組成</li>
</ul>
</td>
<td><ul>
<li>可重複使用的複合式一或多個AEM元件，定義內容和呈現，並形成 <strong>體驗</strong> 這是有道理的</li>
</ul>
</td>
</tr><tr><td><strong>核心租戶</strong></td>
<td><ul>
<li>以內容為中心</li>
<li>由 <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-models.html?lang=en" target="_blank">結構化、表單式、資料模型。</a></li>
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
<li>實作為 <strong>dam:Asset</strong></li>
<li>由 <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-models.html?lang=en" target="_blank">內容片段模型</a></li>
</ul>
</td>
<td><ul>
<li>實作為 <strong>cq：頁面</strong></li>
<li>由可編輯的範本定義</li>
<li>原生HTML轉譯</li>
</ul>
</td>
</tr><tr><td><strong>變化</strong></td>
<td><ul>
<li>主變異是標準變異</li>
<li>變數是特定的使用案例，可能會與通道一致。</li>
</ul>
</td>
<td><ul>
<li>變數是管道或內容特定的</li>
<li>變數會透過AEM Live Copy保持同步</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html" target="_blank">構成要素</a> 允許在不同變數間重複使用內容</li>
</ul>
</td>
</tr><tr><td><strong>功能</strong></td>
<td><ul>
<li>變化</li>
<li>版本</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#synchronizing-with-master" target="_blank">同步</a> 跨變數的內容</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-managing.html?lang=en#comparing-fragment-versions" target="_blank">可視化差異</a> 內容片段版本</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#annotating-a-content-fragment" target="_blank">註解</a> 多行文本元素</li>
<li>智慧型 <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#summarizing-text" target="_blank">摘要</a> 多行文本元素。</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/creating-translation-projects-for-content-fragments.html?lang=en" target="_blank">翻譯/本地化</a></li>
</ul>
</td>
<td><ul>
<li>變化</li>
<li>即時副本形式的變數</li>
<li>版本</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en#building-blocks" target="_blank">建置區塊</a></li>
<li>註解</li>
<li>回應式版面和預覽</li>
<li>翻譯/本地化</li>
<li>透過內容片段參考建立複雜的資料模型</li>
<li>應用程式內預覽</li>
</ul>
</td>
</tr><tr><td><strong>使用</strong></td>
<td><ul>
<li>透過匯出JSON <a href="https://experienceleague.adobe.com/landing/experience-manager/headless/developer.html">AEM無頭GraphQL API</a></li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html" target="_blank">AEM核心元件內容片段元件</a> 以用於AEM Sites、AEM Screens或體驗片段。</li>
<li>透過匯出JSON <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview.html?lang=en" target="_blank">AEM Content Services</a> 適用於第三方消費</li>
<li>針對目標選件將JSON匯出至Adobe Target</li>
<li>透過AEM HTTP Assets API執行JSON作業，以利第三方使用</li>
</ul>
</td>
<td><ul>
<li>AEM體驗片段元件，以用於AEM Sites、AEM Screens或其他體驗片段。</li>
<li>匯出為 <a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en" target="_blank">純HTML</a> 供第三方系統使用</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/integration/experience-fragments-target.html?lang=zh-Hant" target="_blank">HTML匯出至Adobe Target</a> 針對目標選件</li>
<li>針對目標選件將JSON匯出至Adobe Target</li>
</ul>
</td>
</tr><tr><td><strong>常見使用案例</strong></td>
<td><ul>
<li>透過GraphQL提供無頭式使用案例</li>
<li>結構化資料輸入/表單式內容</li>
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
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/home.html?lang=en&amp;topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js" target="_blank">AEM內容片段使用手冊</a></li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/content-fragments-feature-video-use.html?lang=en" target="_blank">在AEM中使用內容片段</a></li>
</ul>
</td>
<td><ul>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en" target="_blank">Adobe體驗片段的檔案</a></li>
</ul>
</td>
</tr></tbody></table>

## 內容片段架構

下圖說明AEM內容片段的整體架構

![內容片段架構](./assets/content-fragments-architecture.png)

+ **內容片段模型** 定義元素（或欄位），定義內容片段可擷取和公開的內容。
+ 此 **內容片段** 是代表邏輯內容實體的內容片段模型的例項。
+ 內容片段 **變化** 但是，堅持「內容片段模型」的內容有所不同。
+ 內容片段可由下列項目公開/使用：
   + 在上使用內容片段 **AEM Sites** (或AEM Screens)透過AEM WCM核心元件的內容片段元件。
   + 使用 **內容片段** 使用AEM Headless GraphQL API的無頭應用程式傳送。
   + 透過將內容片段變體內容公開為JSON **AEM Content Services** 和API頁面，以了解唯讀使用案例。
   + 透過直接呼叫AEM Assets，將內容片段內容（所有變體）直接顯示為JSON **AEM Assets HTTP API** CRUD使用案例。

## 體驗片段架構

![體驗片段架構](./assets/experience-fragments-architecture.png)

+ **可編輯的範本**，而定義者為 **可編輯的範本類型** 和 **AEM頁面元件實作**，定義可用來撰寫體驗片段的允許AEM元件。
+ 此 **體驗片段** 是可編輯範本的例項，代表邏輯體驗。
+ 體驗片段 **變化** 但是，遵循「可編輯範本」在體驗（內容和設計）上有所不同。
+ 體驗片段可由下列項目公開/使用：
   + 透過AEM體驗片段元件在AEM Sites(或AEM Screens)上使用體驗片段。
   + 透過將體驗片段變體內容公開為JSON(含內嵌HTML) **AEM Content Services** 和API頁面。
   + 直接將體驗片段變異公開為 **&quot;純HTML&quot;**.
   + 將體驗片段匯出至 **Adobe Target** 作為HTML或JSON選件。
   + AEM Sites原生支援HTML選件，但是JSON選件需要自訂開發。

## 內容片段的支援資源

+ [內容片段使用手冊](https://experienceleague.adobe.com/docs/experience-manager-65/assets/home.html?lang=en&amp;topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js)
+ [Adobe Experience Manager as a Headless CMS 簡介](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/introduction.html)
+ [在AEM中使用內容片段](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/content-fragments-feature-video-use.html?lang=en)
+ [AEM WCM核心元件的內容片段元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html)
+ [使用內容片段和AEM無頭](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/overview.html?lang=en)
+ [AEM Content Services快速入門](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview.html?lang=en)

## 體驗片段的支援資源

+ [Adobe體驗片段的檔案](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en)
+ [了解AEM體驗片段](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)
+ [使用AEM體驗片段](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)
+ [搭配使用AEM體驗片段和Adobe Target](https://medium.com/adobetech/experience-fragments-and-adobe-target-d8d74381b9b2)
