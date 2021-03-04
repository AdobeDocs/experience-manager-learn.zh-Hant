---
title: 建立HTML5Forms
description: 建立和設定HTML5表格
feature: 行動表單
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 4419
thumbnail: kt-4419.jpg
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '486'
ht-degree: 0%

---


# 建立HTML5表格

HTML5表格是Adobe Experience Manager的一項新功能，提供HTML5格式的XFA表格範本(xdp)轉換功能。 這項功能可讓您在不支援XFA PDF的行動裝置和案頭瀏覽器上轉換表單。 HTML5表格不僅支援XFA表格範本的現有功能，還新增了行動裝置的新功能，例如塗鴉簽名。

## 必備條件

請確定您有AEM Forms的工作實例。 請按照[安裝指南](https://docs.adobe.com/content/help/en/experience-manager-65/forms/install-aem-forms/osgi-installation/installing-configuring-aem-forms-osgi.html)安裝和配置AEM Forms

## 建立您的第一個HTML5表格

1. [下載並解壓縮zip檔的內容](assets/assets.zip)。zip檔案包含xdp和資料檔案
2. [導覽至Forms和檔案](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
3. 按一下「建立->檔案上傳」
4. 選擇步驟2中下載的xdp範本

## 預覽為HTML

xdp可以預覽為HTML5格式或PDF格式。 若要預覽HTML5格式的xdp，請依照下列步驟進行

* 點選新上傳的xdp，然後按一下「預覽->預覽為HTML _」。_&#x200B;您應該會看到xdp呈現為HTML5

>[!NOTE]
>當您選取「預覽為PDF」(_Preview as PDF_)選項時，轉譯的PDF將不會顯示在瀏覽器中，因為AEM Forms會轉譯需要Acrobat外掛程式的動態PDF。您必須下載PDF並使用Adobe Acrobat/Reader開啟它才能檢視


## 以資料預覽

若要使用資料檔案預覽HTML5格式的xdp，請遵循下列步驟：

* 點選新上傳的xdp，然後按一下「預覽->使用資料預覽」。 __&#x200B;瀏覽並選擇資料檔案，然後按一下&#x200B;_預覽_。
* 您應該會看到以HTML5格式呈現的範本已預先填入資料

## 探索xdp範本的進階屬性

xdp範本的進階屬性可讓您指定發佈日期、提交處理常式、表單的演算描述檔、預填服務等。 若要檢視範本的進階屬性，請點選xdp，然後按一下「_屬性->進階_」。 您可在這裡找到許多屬性。 其中有些屬性在此提供。

**提交URL**  —— 此URL將處理您的HTML5表單提交。我們將在下一課中介紹此問題。 如果未在此處指定提交URL，則會呼叫預設的提交處理常式，將表單資料傳回至瀏覽器。

**HTML Render Profile**  - HTML5表單的概念是「描述檔」，以REST端點形式公開，以啟用表單範本的行動轉譯。大多數情況下，預設渲染配置檔案都足以渲染表單。 如果預設演算描述檔不符合您的需求，則可建立自訂描述檔[並與表單建立關聯。](https://docs.adobe.com/content/help/en/experience-manager-64/forms/html5-forms/custom-profile.html)

**預填充服務** -預填充服務通常用於將從後端資料來源擷取的資料填入表單。

