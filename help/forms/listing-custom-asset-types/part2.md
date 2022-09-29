---
title: 列出AEM Forms中的自訂資產類型
description: 列出AEM Forms中自訂資產類型的第2部分
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
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '593'
ht-degree: 0%

---

# 列出AEM Forms中的自訂資產類型 {#listing-custom-asset-types-in-aem-forms}

## 建立自訂範本 {#creating-custom-template}

為了本文的目的，我們要建立自訂範本，以在相同頁面上顯示自訂資產類型和OOTB資產類型。 若要建立自訂範本，請遵循下列指示

1. 建立Sling:/apps底下的資料夾。 將其命名為「 myportalcomponent 」
1. 新增「fpContentType」屬性。 將其值設為「**/libs/fd/ fp/formTemplate」。**
1. 新增「title」屬性，並將其值設為「自訂範本」。 這是您將在Search和Lister元件下拉清單中看到的名稱
1. 在此資料夾下建立「template.html」。 此檔案會保留程式碼，以設定樣式並顯示各種資產類型。

![appsfolder](assets/appsfolder_.png)

下列程式碼會列出使用搜尋和索引鍵元件的各種資產類型。 我們會為每種資產類型建立個別的html元素，如data-type = &quot;videos&quot;標籤所示。 對於「影片」的資產類型，我們會使用 &lt;video> 元素內嵌播放影片。 對於「worddocuments」的資產類型，我們會使用不同的html標籤。

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
>第11行 — 請將影像src變更為指向您在DAM中選擇的影像。
>
>若要在此範本中列出適用性Forms，請建立新的div，並將其資料類型屬性設為「指南」。 您可以複製並貼上div，其data-type=&quot;printForm&quot;，並將新複製div的data-type設為&quot;guide&quot;

## 配置Search和Lister元件 {#configure-search-and-lister-component}

定義自訂範本後，現在必須將此自訂範本與「搜尋及清單」元件建立關聯。 指向您的瀏覽器 [到此url ](http://localhost:4502/editor.html/content/AemForms/CustomPortal.html).

切換到「設計」模式，並將段落系統配置為在允許的元件組中包含「搜索和線索」元件。 Search和Lister元件是「文檔服務」組的一部分。

切換到編輯模式，並將Search和Lister元件添加到ParSys。

開啟「Search and Lister」元件的配置屬性。 確定已選取「資產資料夾」標籤。 選取您要從中列出搜尋和清單元件中資產的資料夾。 為了本文，我已選擇

* /content/dam/VideosAndWordDocuments
* /content/dam/formsanddocuments/assettypes

![assetfolder](assets/selectingassetfolders.png)

頁簽到「顯示」頁簽。 在此，您將選擇要在搜尋和清單元件中顯示資產的範本。

從下拉式清單中選取「自訂範本」，如下所示。

![搜索器](assets/searchandlistercomponent.gif)

設定您要在入口網站中列出的資產類型。 若要在「資產清單」中設定資產的索引標籤類型，並設定資產類型。 在此範例中，我們已設定下列資產類型

1. MP4檔案
1. Word文檔
1. 文檔（這是OOTB資產類型）
1. 表單範本（這是OOTB資產類型）

下列螢幕擷取畫面會顯示已設定以列出的資產類型

![assettypes](assets/assettypes.png)

現在您已配置了Search和Lister Portal元件，現在可以查看該Lister的實際運作。 指向您的瀏覽器 [到此url ](http://localhost:4502/content/AemForms/CustomPortal.html?wcmmode=disabled). 結果應與下圖類似。

>[!NOTE]
>
>如果您的入口網站在發佈伺服器上列出自訂資產類型，請務必為「fd-service」使用者提供節點的「讀取」權限 **/apps/fd/fp/extensions/querybuilder**

![assettypes](assets/assettypeslistings.png)
[請使用套件管理器下載並安裝此套件。](assets/customassettypekt1.zip) 其中包含示例mp4和word文檔以及xdp檔案，這些檔案用作要使用搜索和lister元件列出的資產類型
