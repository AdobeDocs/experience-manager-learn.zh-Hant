---
title: 第1章 — 設定和下載教學課程 — 內容服務
seo-title: AEM內容服務快速入門 — 第1章 — 教學課程設定
description: AEM Headless教學課程的第1章，說明本教學課程的AEM例項的基線設定。
seo-description: AEM Headless教學課程的第1章，說明本教學課程的AEM例項的基線設定。
feature: 內容片段、API
topic: 無頭式、內容管理
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '444'
ht-degree: 0%

---


# 教學課程設定

一律建議使用最新版AEM和AEM WCM核心元件。

* AEM 6.5 或更新版本
* AEM WCM核心元件2.4.0或更新版本
   * 包含在[WKND行動AEM應用程式內容套件中](#wknd-mobile-application-packages)下方

開始本教學課程之前，請確定下列AEM執行個體已安裝[，並在您的本機電腦上執行](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#Default%20Local%20Install):

* **AEM** Authon埠 **4502**
* **AEM** Publishon **埠4503**

## WKND移動應用程式包{#wknd-mobile-application-packages}

使用[!DNL AEM Package Manager]在&#x200B;**** AEM製作和AEM發佈上安裝下列AEM內容套件。

* [ui.apps:GitHub >資產> com.adobe.aem.guides.wknd-mobile.ui.apps-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile Empty Template Type]
   * [!DNL WKND Mobile] AEM WCM核心元件的Proxy元件
   * [!DNL WKND Mobile] AEM Content Services頁面的CSS（適用於次要樣式）
* [ui.content:GitHub > com.adobe.aem.guides.wknd-mobile.ui.content-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile] 網站結構
   * [!DNL WKND Mobile] DAM資料夾結構
   * [!DNL WKND Mobile] 影像資產

在[第7章](./chapter-7.md)中，我們將使用[Android Studio](https://developer.android.com/studio)和提供的APK（Android應用程式套件）執行[!DNL WKND Mobile] Android行動應用程式：

* [[!DNL Android Mobile App: GitHub > Assets > wknd-mobile.x.x.x.apk]](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## 章節AEM內容套件

這組內容包將建立相關章節和前面所有章節中描述的內容和配置。 這些套件為選用，但可加快建立內容的速度。

* [第二章內容：GitHub >資產> com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [第三章內容：GitHub >資產> com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [第四章內容：GitHub >資產> com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [第五章內容：GitHub >資產> com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## 原始碼

AEM專案和[!DNL Android Mobile App]的原始碼均可在[[!DNL AEM Guides - WKND Mobile GitHub Project]](https://github.com/adobe/aem-guides-wknd-mobile)上使用。 不需要為本教學課程建立或修改原始碼，提供原始碼是為了讓教學課程所有方面的建立完全透明。

若您發現教學課程或程式碼的問題，請保留[ GitHub問題](https://github.com/adobe/aem-guides-wknd-mobile/issues)。

## 跳到結尾

若要略過至教學課程結尾，[com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)內容套件可安裝在&#x200B;**兩個** AEM Author和AEM Publish上。 請注意，內容和設定不會如「AEM作者」中所發佈，但由於進行手動部署，所有必要的內容和設定都將可在AEM Publish上使用，讓[!DNL WKND Mobile App]存取內容。


## 下一步

* [第2章 — 定義事件內容片段模型](./chapter-2.md)
