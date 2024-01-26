---
title: 第1章 — 教學課程設定和下載 — 內容服務
description: AEM Headless教學課程的第1章教學課程的AEM執行個體的基準設定。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: f24a75f6-9062-498c-b782-7d9011aa0bcf
duration: 110
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '366'
ht-degree: 0%

---

# 教學課程設定

建議一律使用最新版的AEM和AEM WCM核心元件。

* AEM 6.5或更新版本
* AEM WCM Core Components 2.4.0或更新版本
   * 包含在 [以下的WKND Mobile AEM應用程式內容套件](#wknd-mobile-application-packages)

開始本教學課程之前，請確定下列AEM例項為 [已在您的本機電腦上安裝並執行](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#Default%20Local%20Install)：

* **AEM作者** 於 **連線埠4502**
* **AEM發佈** 於 **連線埠4503**

## WKND行動應用程式套件{#wknd-mobile-application-packages}

在上安裝下列AEM內容套件 **兩者** AEM Author和AEM Publish，使用 [!DNL AEM Package Manager].

* [ui.apps： GitHub >資產> com.adobe.aem.guides.wknd-mobile.ui.apps-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile Empty Template Type]
   * [!DNL WKND Mobile] AEM WCM核心元件的Proxy元件
   * [!DNL WKND Mobile] AEM Content Services頁面的CSS （適用於次要樣式）
* [ui.content： GitHub > com.adobe.aem.guides.wknd-mobile.ui.content-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile] 網站結構
   * [!DNL WKND Mobile] DAM資料夾結構
   * [!DNL WKND Mobile] 影像資產

在 [第7章](./chapter-7.md) 我們將執行 [!DNL WKND Mobile] Android行動應用程式，使用 [Android Studio](https://developer.android.com/studio) 以及提供的APK （Android應用程式套件）：

* [[!DNL Android Mobile App: GitHub > Assets > wknd-mobile.x.x.x.apk]](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## 章節AEM內容套件

這組內容套件會建立相關章節及所有先前章節中所述的內容和組態。 這些套件是選用套件，但可加快內容建立速度。

* [第2章內容： GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [第3章內容： GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [第4章內容： GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [第5章內容： GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## 原始碼

AEM專案和 [!DNL Android Mobile App] 可在 [[!DNL AEM Guides - WKND Mobile GitHub Project]](https://github.com/adobe/aem-guides-wknd-mobile). 您不需要為本教學課程建置或修改原始程式碼，提供原始程式碼是為了讓建置教學課程所有層面的方式完全透明。

若教學課程或程式碼有任何問題，請留下 [GitHub問題](https://github.com/adobe/aem-guides-wknd-mobile/issues).

## 跳至結尾

若要跳至教學課程的結尾，請前往 [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) 內容套件可安裝在 **兩者** AEM作者和AEM發佈。 請注意，內容與設定不會顯示為AEM Author中的已發佈內容，但由於手動部署，所有必要內容與設定均可在AEM Publish上取得，允許 [!DNL WKND Mobile App] 以存取內容。


## 下一步

* [第2章 — 定義事件內容片段模型](./chapter-2.md)
