---
title: 第1章——教學課程設定與下載——內容服務
seo-title: 內容服AEM務快速入門——第1章——教學課程設定
description: 無頭教學課程AEM的第1章，說明教學課程AEM實例的基線設定。
seo-description: 無頭教學課程AEM的第1章，說明教學課程AEM實例的基線設定。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 0%

---


# 教學課程設定

一律建議使用最AEM新版AEM和WCM核心元件。

* AEM 6.5 或更新版本
* AEMWCM核心元件2.4.0或更新版本
   * 包含在[](#wknd-mobile-application-packages)下方的AEMWKND行動應用程式內容套件中

在啟動本教程之前，請確AEM保在本機機器上安裝並運行以下實例：[](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#Default%20Local%20Install)

* **AEM** Authoron端 **口4502**
* **AEM** Publishon  **port 4503**

## WKND移動應用程式套件{#wknd-mobile-application-packages}

使用&lt;a2/AEM>將下列內容套件安裝在&#x200B;**** AEM Author和AEM Publish上。[!DNL AEM Package Manager]

* [ui.apps:GitHub >資產> com.adobe.aem.guides.wknd-mobile.ui.apps-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile Empty Template Type]
   * [!DNL WKND Mobile] WCM核心元AEM件的Proxy元件
   * [!DNL WKND Mobile] 內AEM容服務頁面的CSS（用於次要樣式）
* [ui.content:GitHub > com.adobe.aem.guides.wknd-mobile.ui.content-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile] 網站結構
   * [!DNL WKND Mobile] DAM資料夾結構
   * [!DNL WKND Mobile] 影像資產

在[Chapter 7](./chapter-7.md)中，我們將使用[Android Studio](https://developer.android.com/studio)和提供的APK（Android應用程式套件）執行[!DNL WKND Mobile] Android行動應用程式：

* [[!DNL Android Mobile App: GitHub > Assets > wknd-mobile.x.x.x.apk]](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## 章節內AEM容包

這組內容套件會建立相關章節和所有前幾章所述的內容與設定。 這些套件是選用的，但可加速內容建立。

* [第二章內容：GitHub >資產> com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [第三章內容：GitHub >資產> com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [第四章內容：GitHub >資產> com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [第五章內容：GitHub >資產> com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## 原始碼

項目和&lt;a0/AEM>的原始碼都可在[[!DNL AEM Guides - WKND Mobile GitHub Project]](https://github.com/adobe/aem-guides-wknd-mobile)中使用。 [!DNL Android Mobile App]本教學課程不需要建立或修改原始碼，所以提供原始碼是為了讓教學課程各方面的建立完全透明。

如果您發現教學課程或程式碼有問題，請留下[GitHub問題](https://github.com/adobe/aem-guides-wknd-mobile/issues)。

## 跳到最後

為了跳至教學課程的結束，[com.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)內容套件可安裝在&#x200B;**** AEM Author和AEM Publish上。 請注意，內容和設定不會顯示為「AEM作者」中的發佈，但是，由於有手動部署，所有必要的內容和設定都可在「AEM發佈」上使用，讓[!DNL WKND Mobile App]存取內容。


## 下一步

* [第2章——定義事件內容片段模型](./chapter-2.md)
