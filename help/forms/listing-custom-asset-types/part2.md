---
title: 在AEM Forms列出自訂資產類型
seo-title: 在AEM Forms列出自訂資產類型
description: AEM Forms自訂資產類型清單第2部分
seo-description: AEM Forms自訂資產類型清單第2部分
uuid: 6467ec34-e452-4c21-9bb5-504f9630466a
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
discoiquuid: 4b940465-0bd7-45a2-8d01-e4d640c9aedf
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '616'
ht-degree: 0%

---


# 列出AEM Forms的自定義資產類型{#listing-custom-asset-types-in-aem-forms}

## 建立自訂範本{#creating-custom-template}


為了本文，我們將建立自訂範本，以顯示相同頁面上的自訂資產類型和OOTB資產類型。 若要建立自訂範本，請依照下列指示進行

1. 建立吊索：資料夾。 將其命名為&quot; myportalcomponent &quot;
1. 新增&quot;fpContentType&quot;屬性。 將其值設定為&quot;**/libs/fd/fp/formTemplate&quot;。**
1. 新增「title」屬性，並將其值設為「自訂範本」。 這是您在Search and Lister元件下拉式清單中看到的名稱
1. 在此資料夾下建立「template.html」。 此檔案會保留程式碼以設定樣式並顯示各種資產類型。

![appsfolder](assets/appsfolder_.png)

下列程式碼會列出使用搜尋和lister元件的各種資產類型。 我們會針對每種資產類型建立個別的html元素，如data-type = &quot;videos&quot;標籤所示。 對於「videos」的資產類型，我們使用&lt;video>元素來播放內嵌影片。 對於「worddocuments」的資產類型，我們會使用不同的html標籤。

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
>第11行——請將影像src變更為指向您在DAM中選擇的影像。
>
>若要在此範本中列出最適化Forms，請建立新div，並將其資料類型屬性設為&quot;guide&quot;。 您可以複製並貼上div的data-type=&quot;printForm，並將新複製的div的data-type設為&quot;guide&quot;

## 配置搜索和Lister元件{#configure-search-and-lister-component}

在定義自訂範本後，我們現在必須將此自訂範本與「搜尋與清單」元件建立關聯。 將瀏覽器[指向此url ](http://localhost:4502/editor.html/content/AemForms/CustomPortal.html)。

切換到「設計」模式，並將段落系統配置為在允許的元件組中包含Search And Lister元件。 Search and Lister元件是Document Services組的一部分。

切換至編輯模式，並將Search and Lister元件新增至ParSys。

開啟「搜索和Lister」元件的配置屬性。 請確定已選取「資產資料夾」標籤。 選擇要從中列出搜索和線索元件中資產的資料夾。 為本文之目的，我已選取

* /content/dam/videosAndWordDocuments
* /content/dam/formsanddocuments/assettypes

![assetfolder](assets/selectingassetfolders.png)

Tab to the &quot;Display&quot; tab. 在此，您將選擇要在搜尋和清單元件中顯示資產的範本。

從下拉式清單中選取「自訂範本」，如下所示。

![搜索器](assets/searchandlistercomponent.gif)

設定您要在入口網站中列出的資產類型。 若要設定「資產清單」資產的標籤類型，並設定資產類型。 在此範例中，我們已設定下列資產類型

1. MP4檔案
1. Word檔案
1. 文檔（這是OOTB資產類型）
1. 表單範本（這是OOTB資產類型）

下列螢幕擷取畫面會顯示已設定以列出的資產類型

![assettypes](assets/assettypes.png)

現在您已設定好搜尋和Lister入口元件，是時候讓Lister開始運作了。 將瀏覽器[指向此url ](http://localhost:4502/content/AemForms/CustomPortal.html?wcmmode=disabled)。 結果應該類似於下圖。

>[!NOTE]
>
>如果您的入口網站正在發佈伺服器上列出自訂資產類型，請確定您為節點&#x200B;**/apps/fp/extensions/querybuilder**&#x200B;的&quot;fd-service&quot;使用者提供「讀取」權限

![資](assets/assettypeslistings.png)
[產類型請使用套件管理器下載並安裝此套件。](assets/customassettypekt1.zip) 其中包含範例mp4和word檔案，以及xdp檔案，這些檔案將用作資產類型，以使用搜尋和lister元件列出
