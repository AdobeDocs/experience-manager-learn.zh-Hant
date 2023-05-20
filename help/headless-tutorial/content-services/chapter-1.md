---
title: 第1章 — 教程設定和下載 — 內容服務
seo-title: Getting Started with AEM Content Services - Chapter 1 -  Tutorial Set up
description: Headless教程的第AEM1章，介紹教程實例AEM的基線設定。
seo-description: Chapter 1 of the AEM Headless tutorial the baseline setup for the AEM instance for the tutorial.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: f24a75f6-9062-498c-b782-7d9011aa0bcf
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '411'
ht-degree: 1%

---

# 教程設定

始終建議使用最AEM新版AEM本和WCM核心元件。

* AEM 6.5 或更新版本
* AEMWCM核心元件2.4.0或更高版本
   * 包含在 [下面的WKND移AEM動應用程式內容包](#wknd-mobile-application-packages)

在啟動本教程之前，請確保以AEM下實例 [安裝並運行](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#Default%20Local%20Install):

* **AEM作者** 上 **埠4502**
* **AEM發佈** 上 **埠4503**

## WKND移動應用程式套件{#wknd-mobile-application-packages}

在上安AEM裝以下內容包 **兩者** AEM作者和AEM發佈，使用 [!DNL AEM Package Manager]。

* [ui.apps:GitHub >資產> com.adobe.aem.guides.wknd-mobile.ui.apps-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile Empty Template Type]
   * [!DNL WKND Mobile] WCM核心元件AEM的代理元件
   * [!DNL WKND Mobile] Content ServicesAEM頁面的CSS（用於次要樣式）
* [ui.content:GitHub > com.adobe.aem.guides.wknd-mobile.ui.content-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile] 站點結構
   * [!DNL WKND Mobile] DAM資料夾結構
   * [!DNL WKND Mobile] 影像資產

在 [第七章](./chapter-7.md) 我們會 [!DNL WKND Mobile] Android Mobile App使用 [安卓工作室](https://developer.android.com/studio) 以及提供的APK（Android應用程式套件）:

* [[!DNL Android Mobile App: GitHub > Assets > wknd-mobile.x.x.x.apk]](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## 章AEM內容包

這組內容包建立相關章節和前面所有章節中描述的內容和配置。 這些包是可選的，但可加快內容建立。

* [第二章內容：GitHub >資產> com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [第三章內容：GitHub >資產> com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [第四章內容：GitHub >資產> com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [第五章內容：GitHub >資產> com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## 原始碼

項目和AEM的原始碼 [!DNL Android Mobile App] 在 [[!DNL AEM Guides - WKND Mobile GitHub Project]](https://github.com/adobe/aem-guides-wknd-mobile)。 本教程不需要構建或修改原始碼，它的提供允許在構建教程的所有方面方面實現完全透明。

如果您發現教程或代碼有問題，請留下 [GitHub問題](https://github.com/adobe/aem-guides-wknd-mobile/issues)。

## 跳到結尾

要跳到本教程的結尾， [com.adobe.aem.guides.wknd-mobile content-chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) 內容包可以安裝在 **兩者** AEM作者和AEM發佈。 請注意，內容和配置不會像在AEM Author中發佈的那樣顯示，但是，由於手動部署，AEM Publish上提供了所有必需的內容和配置，允許 [!DNL WKND Mobile App] 的子菜單。


## 下一步

* [第2章 — 定義事件內容片段模型](./chapter-2.md)
