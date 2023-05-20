---
title: 建立HTML5Forms
description: 建立和配置HTML5窗體
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
kt: 4419
thumbnail: kt-4419.jpg
topic: Development
role: User
level: Beginner
exl-id: 67a01c41-d284-4518-adb5-21702e22ccfa
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 53af8fbc20ff21abf8778bbc165b5ec7fbdf8c8f
workflow-type: tm+mt
source-wordcount: '481'
ht-degree: 0%

---

# 建立HTML5窗體

HTML5表單是Adobe Experience Manager的一種新功能，它提供以HTML5格式呈現XFA表單模板(xdp)。 此功能允許在不支援基於XFA的PDF的移動設備和案頭瀏覽器上呈現表單。 HTML5表單不僅支援XFA表單模板的現有功能，還為移動設備添加了新功能，如手寫簽名。

## 必備條件

請確保你有AEM Forms的正在處理的實例。 請關注 [安裝指南](https://experienceleague.adobe.com/docs/experience-manager-65/forms/install-aem-forms/osgi-installation/installing-configuring-aem-forms-osgi.html) 安裝和配置AEM Forms

## 建立第一個HTML5窗體

1. [下載並解壓zip檔案的內容](assets/assets.zip)。 zip檔案包含xdp和資料檔案
2. [導航到Forms和文檔](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
3. 按一下「建立」 — >「檔案上載」
4. 選擇步驟2中下載的xdp模板

## 預覽為HTML

可以以HTML5格式或PDF格式預覽xdp。 要以HTML5格式預覽xdp，請執行以下步驟

* 點擊新上載的xdp ，然後按一下 _預覽 — >預覽為HTML_。 應將xdp呈現為HTML5

>[!NOTE]
>選擇時 _預覽為PDF_ 選項呈現的PDF將不會顯示在瀏覽器中，因為AEM Forms呈現需要Acrobat插件的動態pdf。您必須下載PDF並使用Adobe Acrobat/Reader開啟它才能查看


## 以資料預覽

要使用資料檔案以HTML5格式預覽xdp，請執行以下步驟：

* 點擊新上載的xdp ，然後按一下 _預覽 — >使用資料預覽_。 瀏覽並選擇資料檔案，然後按一下 _預覽_。
* 您應看到以HTML5格式呈現的預填充了資料的模板

## 瀏覽xdp模板的高級屬性

xdp模板的高級屬性允許您指定發佈日期、提交處理程式、表單的呈現配置檔案、預填充服務等。 要查看模板的高級屬性，請在xdp上按一下 _屬性 — >高級_。 在這裡，您將找到許多屬性。 其中有些屬性在此處。

**提交URL**  — 此URL將處理HTML5表單提交。 下節課我們將介紹此內容。 如果未在此處指定提交URL，則會調用預設的提交處理程式，將表單資料返回到瀏覽器。

**HTML渲染配置檔案** -HTML5表單的概念是「配置檔案」，這些配置檔案作為REST端點公開，以啟用表單模板的移動渲染。 預設渲染配置檔案的大多數時間都應足以渲染表單。 如果預設渲染配置檔案不滿足您的需要，則 [自定義配置檔案](https://experienceleague.adobe.com/docs/experience-manager-65/forms/html5-forms/custom-profile.html) 可以建立並與窗體關聯。

**預填充服務**  — 預填充服務通常用於用從後端資料源讀取的資料填充表單。
