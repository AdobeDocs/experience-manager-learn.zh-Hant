---
title: 第1章——教學課程設定與下載——內容服務
seo-title: AEM Content Services快速入門——第1章——教學課程設定
description: AEM Headless教學課程的第1章，說明教學課程的AEM例項的基準設定。
seo-description: AEM Headless教學課程的第1章，說明教學課程的AEM例項的基準設定。
translation-type: tm+mt
source-git-commit: 5012433a5f1c7169b1a3996453bfdbd5d78e5b1c
workflow-type: tm+mt
source-wordcount: '438'
ht-degree: 0%

---


# 教學課程設定

一律建議使用最新版的AEM和AEM WCM核心元件。

* AEM 6.5 或更新版本
* AEM WCM Core Components 2.4.0或更新版本
   * 包含在[WKND Mobile AEM Application Content Package（位於](#wknd-mobile-application-packages)下方的&lt;a0/>WKND Mobile AEM應用程式內容套件）中

在開始本教學課程之前，請確定下列AEM例項已安裝並在您的本機電腦上執行：[](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#Default%20Local%20Install)

* **AEM** Authoron端 **口4502**
* **AEM** Publishon  **port 4503**

## WKND移動應用程式套件{#wknd-mobile-application-packages}

使用[!DNL AEM Package Manager]在&#x200B;**** AEM Author和AEM Publish上安裝下列AEM內容套件。

* [ui.apps:GitHub >資產> com.adobe.aem.guides.wknd-mobile.ui.apps-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile Empty Template Type]
   * [!DNL WKND Mobile] AEM WCM核心元件的Proxy元件
   * [!DNL WKND Mobile] AEM Content Services頁面的CSS（用於次要樣式）
* [ui.content:GitHub > com.adobe.aem.guides.wknd-mobile.ui.content-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile] 網站結構
   * [!DNL WKND Mobile] DAM資料夾結構
   * [!DNL WKND Mobile] 影像資產

在[Chapter 7](./chapter-7.md)中，我們將使用[Android Studio](https://developer.android.com/studio)和提供的APK（Android應用程式套件）執行[!DNL WKND Mobile] Android行動應用程式：

* [[!DNL Android Mobile App: GitHub > Assets > wknd-mobile.x.x.x.apk]](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## 第AEM內容套件

這組內容套件會建立相關章節和所有前幾章所述的內容與設定。 這些套件是選用的，但可加速內容建立。

* [第二章內容：GitHub >資產> com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [第三章內容：GitHub >資產> com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [第四章內容：GitHub >資產> com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [第五章內容：GitHub >資產> com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## 原始碼

AEM專案和[!DNL Android Mobile App]的原始碼皆可在[[!DNL AEM Guides - WKND Mobile GitHub Project]](https://github.com/adobe/aem-guides-wknd-mobile)上取得。 本教學課程不需要建立或修改原始碼，所以提供原始碼是為了讓教學課程各方面的建立完全透明。

如果您發現教學課程或程式碼有問題，請留下[GitHub問題](https://github.com/adobe/aem-guides-wknd-mobile/issues)。

## 跳到最後

為了跳至教學課程的結束，[com.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)內容套件可安裝在&#x200B;**** AEM Author和AEM Publish上。 請注意，內容和設定不會顯示為「AEM作者」中的發佈，但是，由於有手動部署，所有必要的內容和設定都可在「AEM發佈」上使用，讓[!DNL WKND Mobile App]存取內容。


## 下一步

* [第2章——定義事件內容片段模型](./chapter-2.md)
