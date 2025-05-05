---
title: 第1章 — 教學課程設定和下載 — 內容服務
description: AEM Headless教學課程的第1章教學課程的AEM執行個體的基準設定。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: f24a75f6-9062-498c-b782-7d9011aa0bcf
duration: 85
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '366'
ht-degree: 1%

---

# 教學課程設定

建議一律使用最新版的AEM和AEM WCM核心元件。

* AEM 6.5或更新版本
* AEM WCM Core Components 2.4.0或更新版本
   * 包含在以下[&#128279;](#wknd-mobile-application-packages)的WKND Mobile AEM應用程式內容套件中

開始此教學課程之前，請確定下列AEM執行個體已[安裝並在您的本機電腦](https://helpx.adobe.com/tw/experience-manager/6-5/sites/deploying/using/deploy.html#Default%20Local%20Install)上執行：

* 在&#x200B;**連線埠4502**&#x200B;上的&#x200B;**AEM作者**
* 在&#x200B;**連線埠4503**&#x200B;上的&#x200B;**AEM Publish**

## WKND行動應用程式套件{#wknd-mobile-application-packages}

使用[!DNL AEM Package Manager]，在&#x200B;**both** AEM Author和AEM Publish上安裝下列AEM內容套件。

* [ui.apps： GitHub > Assets > com.adobe.aem.guides.wknd-mobile.ui.apps-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile Empty Template Type]
   * AEM WCM核心元件的[!DNL WKND Mobile]個Proxy元件
   * [!DNL WKND Mobile] AEM Content Services頁面的CSS （適用於次要樣式）
* [ui.content： GitHub > com.adobe.aem.guides.wknd-mobile.ui.content-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile]網站結構
   * [!DNL WKND Mobile] DAM資料夾結構
   * [!DNL WKND Mobile]個影像資產

在[第7](./chapter-7.md)章中，我們將使用[Android Studio](https://developer.android.com/studio)與提供的APK (Android應用程式套件)來執行[!DNL WKND Mobile]Android行動應用程式：

* [[!DNL Android Mobile App: GitHub > Assets > wknd-mobile.x.x.x.apk]](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## 章節AEM內容套件

這組內容套件會建立相關章節及所有先前章節中所述的內容和組態。 這些套件是選用套件，但可加快內容建立速度。

* [第2章內容： GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [第3章內容： GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [第4章內容： GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [第5章內容： GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## 原始碼

AEM專案和[!DNL Android Mobile App]的原始碼都可在[[!DNL AEM Guides - WKND Mobile GitHub Project]](https://github.com/adobe/aem-guides-wknd-mobile)上取得。 您不需要為本教學課程建置或修改原始程式碼，提供原始程式碼是為了讓建置教學課程所有層面的方式完全透明。

如果您發現教學課程或程式碼有問題，請留下[GitHub問題](https://github.com/adobe/aem-guides-wknd-mobile/issues)。

## 跳至結尾

為了跳到教學課程結尾，可以在&#x200B;**AEM Author和AEM Publish上同時安裝[com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)內容套件。**&#x200B;請注意，內容與設定將不會在AEM Author中顯示為已發佈，但由於是手動部署，所有必要的內容與設定都可在AEM Publish上取得，以允許[!DNL WKND Mobile App]存取內容。


## 下一步

* [第2章 — 定義事件內容片段模型](./chapter-2.md)
