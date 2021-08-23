---
title: 建立HTML5 Forms
description: 建立和設定HTML5表單
feature: 行動表單
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 4419
thumbnail: kt-4419.jpg
topic: 開發
role: User
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '484'
ht-degree: 1%

---


# 建立HTML5表單

HTML5表單是Adobe Experience Manager中的新功能，提供HTML5格式的XFA表單範本(xdp)轉譯功能。 此功能可讓您在不支援XFA型PDF的行動裝置和案頭瀏覽器上轉譯表單。 HTML5表單不僅支援XFA表單範本的現有功能，也為行動裝置新增了新功能，例如手寫簽名。

## 必備條件

請確定您有有效的AEM Forms例項。 請依照[安裝指南](https://experienceleague.adobe.com/docs/experience-manager-65/forms/install-aem-forms/osgi-installation/installing-configuring-aem-forms-osgi.html)安裝及設定AEM Forms

## 建立您的第一個HTML5表單

1. [下載並解壓縮zip檔案的內容](assets/assets.zip)。zip檔案包含xdp和資料檔案
2. [導覽至Forms和檔案](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
3. 按一下「建立 — >檔案上傳」
4. 選取在步驟2中下載的xdp範本

## 預覽為HTML

xdp可以預覽為HTML5格式或PDF格式。 若要以HTML5格式預覽xdp，請遵循下列步驟

* 點選新上傳的xdp，然後按一下「_預覽 — >以HTML_&#x200B;預覽」。 您應會看到xdp呈現為HTML5

>[!NOTE]
>當您選擇&#x200B;_預覽為PDF_&#x200B;選項時，呈現的PDF不會顯示在瀏覽器中，因為AEM Forms會呈現需要Acrobat外掛程式的動態PDF。您必須下載PDF並使用Adobe Acrobat/Reader開啟它才能檢視


## 以資料預覽

若要使用資料檔案預覽HTML5格式的xdp，請執行下列步驟：

* 點選新上傳的xdp，然後按一下「_預覽 — >使用資料預覽_」。 瀏覽並選擇資料檔案，然後按一下&#x200B;_預覽_。
* 您應會看到以HTML5格式呈現的範本已預先填入資料

## 探索xdp範本的進階屬性

xdp範本的進階屬性可讓您指定發佈日期、提交處理常式、表單的轉譯設定檔、預填服務等。 若要檢視範本的進階屬性，請點選xdp ，然後按一下&#x200B;_properties -> Advanced_。 您可在此處找到許多屬性。 其中部分屬性已涵蓋於此處。

**提交URL**  — 這是將處理HTML5表單提交的URL。我們將在下一課中介紹此內容。 若未在此指定提交URL，則會叫用預設的提交處理常式，這會將表單資料傳回至瀏覽器。

**HTML呈現描述檔**  - HTML5表單的概念是描述檔，這些描述檔公開為REST端點，以啟用表單範本的行動呈現。大多數情況下，預設呈現配置檔案應足以呈現表單。 如果預設的呈現配置檔案不符合您的需要，則可以建立[自定義配置檔案](https://experienceleague.adobe.com/docs/experience-manager-64/forms/html5-forms/custom-profile.html)並與表單關聯。

**預填服務**  — 預填服務通常用於將從後端資料來源擷取的資料填入您的表單。

