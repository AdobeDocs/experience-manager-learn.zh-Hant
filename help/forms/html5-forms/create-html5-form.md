---
title: 建立HTML5 Forms
description: 建立及設定HTML5表單
feature: Mobile Forms
doc-type: article
version: Experience Manager 6.5
jira: KT-4419
thumbnail: kt-4419.jpg
topic: Development
role: User
level: Beginner
exl-id: 67a01c41-d284-4518-adb5-21702e22ccfa
last-substantial-update: 2019-07-07T00:00:00Z
duration: 101
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '462'
ht-degree: 0%

---

# 建立HTML5表單

HTML5 forms是Adobe Experience Manager中的新功能，可轉譯HTML5格式的XFA表單範本(xdp)。 此功能可讓您在不支援XFA型PDF的行動裝置和桌上型瀏覽器上轉譯表單。 HTML5表單不僅支援XFA表單範本的現有功能，也新增了適用於行動裝置的新功能，例如手寫簽名。

## 必備條件

請確定您有AEM Forms的工作執行個體。 請依照[安裝指南](https://experienceleague.adobe.com/docs/experience-manager-65/forms/install-aem-forms/osgi-installation/installing-configuring-aem-forms-osgi.html?lang=zh-Hant)中的指示安裝並設定AEM Forms

## 建立您的第一個HTML5表單

1. [下載並解壓縮zip檔案的內容](assets/assets.zip)。 zip檔案包含xdp和資料檔案
2. [瀏覽至Forms和檔案](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
3. 按一下「建立 — >檔案上傳」
4. 選取在步驟2中下載的xdp範本

## 以HTML預覽

可以透過HTML5格式或PDF格式預覽xdp。 若要以HTML5格式預覽xdp，請遵循下列步驟

* 點選新上傳的xdp，然後按一下「_預覽 — >以HTML預覽_」。 您應該會看到xdp呈現為HTML5

>[!NOTE]
>當您選取「_以PDF預覽_」選項時，已轉譯的PDF將不會顯示在瀏覽器中，因為AEM Forms已轉譯需要Acrobat外掛程式的動態PDF。您必須下載PDF並使用Adobe Acrobat/Reader開啟它才能檢視


## 以資料預覽

若要使用資料檔案預覽HTML5格式的xdp，請遵循下列步驟：

* 點選新上傳的xdp，然後按一下&#x200B;_預覽 — >使用資料預覽_。 瀏覽並選取資料檔案，然後按一下&#x200B;_預覽_。
* 您應該會看到範本以HTML5格式呈現並預先填入資料

## 探索xdp範本的進階屬性

xdp範本的進階屬性可讓您指定發佈日期、提交處理常式、表單的轉譯設定檔、預填服務等。 若要檢視範本的進階屬性，請點選xdp並按一下&#x200B;_屬性 — >進階_。 您可在這裡找到許多屬性。 此處說明其中一些屬性。

**提交URL** — 這是將處理您的HTML5表單提交的URL。 我們將在下一個課程中說明。 如果未在此指定提交URL，則會叫用預設提交處理常式，以將表單資料傳回瀏覽器。

**HTML轉譯設定檔** - HTML5表單具有設定檔的概念，可公開為REST端點，以啟用表單範本的行動轉譯。 預設演算設定檔大多數時候應該足以演算表單。 如果預設的轉譯器設定檔不符合您的需求，可以建立[自訂設定檔](https://experienceleague.adobe.com/docs/experience-manager-65/forms/html5-forms/custom-profile.html?lang=zh-Hant)並與表單建立關聯。

**預填服務** — 預填服務通常用於以從後端資料來源擷取的資料填入您的表單。
