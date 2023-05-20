---
title: 在AEM Forms列出自定義資產類型
description: AEM Forms上市自定義資產類型第2部分
uuid: 6467ec34-e452-4c21-9bb5-504f9630466a
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 4b940465-0bd7-45a2-8d01-e4d640c9aedf
topic: Development
role: Developer
level: Experienced
exl-id: f221d8ee-0452-4690-a936-74bab506d7ca
last-substantial-update: 2019-07-10T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '593'
ht-degree: 0%

---

# 在AEM Forms列出自定義資產類型 {#listing-custom-asset-types-in-aem-forms}

## 建立自定義模板 {#creating-custom-template}

為此文章的目的，我們正在建立一個自定義模板，以在同一頁上顯示自定義資產類型和OOTB資產類型。 要建立自定義模板，請按照以下說明操作

1. 建立吊帶：資料夾。 將其命名為&quot; myportalcomponent &quot;
1. 添加&quot;fpContentType&quot;屬性。 將其值設定為「」**/libs/fd/fp/formTemplate」。**
1. 添加「title」屬性，並將其值設定為「自定義模板」。 這是您將在「搜索」和「Lister元件」下拉清單中看到的名稱
1. 在此資料夾下建立&quot;template.html&quot;。 此檔案將保存要樣式化的代碼並顯示各種資產類型。

![應用程式資料夾](assets/appsfolder_.png)

以下代碼列出了使用搜索和lister元件的各種類型的資產。 我們為每種類型的資產建立單獨的html元素，如data-type = &quot;videos&quot;標籤所示。 對於資產類型的「視頻」，我們使用 &lt;video> 內聯播放視頻的元素。 對於「worddocuments」的資產類型，我們使用不同的html標籤。

```html
<div class="__FP_boxes-container __FP_single-color">
   <div  data-repeatable="true">
     <div class = "__FP_boxes-thumbnail" style="float:left;margin-right:20px;" data-type = "videos">
   <video width="400" controls>
       <source src="${path}" type="video/mp4">
    </video>
         <h3 class="__FP_single-color" title="${name}" tabindex="0">${name}</h3>
     </div>
     <div class="__FP_boxes-thumbnail" style="float:left;margin-right:20px;" data-type = "worddocuments">
       <a href="/assetdetails.html${path}" target="_blank">
           <img src ="/content/dam/worddocuments/worddocument.png/jcr:content/renditions/cq5dam.thumbnail.319.319.png"/>
          </a>
          <h3 class="__FP_single-color" title="${name}" tabindex="0">${name}</h3>
     </div>
  <div class="__FP_boxes-thumbnail" style="float:left;margin-right:20px;" data-type = "xfaForm">
       <a href="/assetdetails.html${path}" target="_blank">
           <img src ="${path}/jcr:content/renditions/cq5dam.thumbnail.319.319.png"/>
          </a>
          <h3 class="__FP_single-color" title="${name}" tabindex="0">${name}</h3>
                <a href="{formUrl}"><img src="/content/dam/html5.png"></a><p>

     </div>
  <div class="__FP_boxes-thumbnail" style="float:left;margin-right:20px;" data-type = "printForm">
       <a href="/assetdetails.html${path}" target="_blank">
           <img src ="${path}/jcr:content/renditions/cq5dam.thumbnail.319.319.png"/>
          </a>
          <h3 class="__FP_single-color" title="${name}" tabindex="0">${name}</h3>
                <a href="{pdfUrl}"><img src="/content/dam/pdf.png"></a><p>
     </div>
   </div>
</div>
```

>[!NOTE]
>
>第11行 — 請更改影像src以指向您在DAM中選擇的影像。
>
>要在此模板中列出AdaptiveForms，請建立新div並將其資料類型屬性設定為「guide」。 可以複製並貼上其data-type=&quot;printForm的div，並將新複製的div的data-type設定為&quot;guide&quot;

## 配置搜索和Lister元件 {#configure-search-and-lister-component}

定義自定義模板後，現在必須將此自定義模板與「Search and Lister」元件關聯。 指向瀏覽器 [此URL ](http://localhost:4502/editor.html/content/AemForms/CustomPortal.html)。

切換到「設計」模式，並將段落系統配置為在允許的元件組中包括「搜索和李斯特」元件。 Search和Lister元件是Document Services組的一部分。

切換到編輯模式，並將Search和Lister元件添加到ParSys。

開啟「Search and Lister」元件的配置屬性。 確保選中「資產資料夾」頁籤。 選擇要從中列出搜索和線索元件中資產的資料夾。 為本文的目的，我選擇

* /content/dam/VideosAndWordDocuments
* /content/dam/formsanddocuments/assettypes

![程式集資料夾](assets/selectingassetfolders.png)

頁籤。 在此，您將選擇要在搜索和線索元件中顯示資產的模板。

從下拉清單中選擇「自定義模板」，如下所示。

![塞勒斯特](assets/searchandlistercomponent.gif)

配置要在門戶中列出的資產類型。 配置「資產清單」的資產類型頁籤，並配置資產類型。 在本示例中，我們配置了以下類型的資產

1. MP4檔案
1. Word文檔
1. 文檔（這是OOTB資產類型）
1. 表單模板（這是OOTB資產類型）

下面的螢幕抓圖顯示了為清單配置的資產類型

![程式集類型](assets/assettypes.png)

現在您已配置了Search和Lister Portal元件，是時候讓Lister開始行動了。 指向瀏覽器 [此URL ](http://localhost:4502/content/AemForms/CustomPortal.html?wcmmode=disabled)。 結果應與下面顯示的影像類似。

>[!NOTE]
>
>如果您的門戶正在發佈伺服器上列出自定義資產類型，請確保您向節點授予「fd-service」用戶「讀取」權限 **/apps/fd/fp/extensions/querybuilder**

![程式集類型](assets/assettypeslistings.png)
[請使用包管理器下載並安裝此包。](assets/customassettypekt1.zip) 這包含示例mp4和word文檔以及xdp檔案，它們用作使用搜索和lister元件列出的資產類型
