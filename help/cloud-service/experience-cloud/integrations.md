---
title: AEMas a Cloud Service與Adobe Experience Cloud整合
description: 了解AEM as a Cloud Service支援的與其他Adobe Experience Cloud產品整合。
version: Cloud Service
feature: Integrations
topic: Integrations
role: Leader, Architect, Admin, Developer
level: Beginner
kt: 10718
thumbnail: KT-10718.png
last-substantial-update: 2022-10-02T00:00:00Z
mini-toc-levels: 1
exl-id: 9e856dcc-f042-4e9d-bf97-dd4f72e837e3
source-git-commit: d0b13fd37f1ed42042431246f755a913b56625ec
workflow-type: tm+mt
source-wordcount: '958'
ht-degree: 9%

---

# AEMas a Cloud Service與Adobe Experience Cloud整合

了解AEM as a Cloud Service支援的與其他Adobe Experience Cloud產品整合。
按一下Experience Cloud產品以取得如何設定和使用整合的檔案。

|  | AEM Sites | AEM Assets | AEM Forms |
|-------------------------------------------------------------------|:---------:|:----------:|:---------:|
| [Acrobat Sign](#adobe-acrobat-sign) |  |  | ✔ |
| 廣告 |  |  |  |
| [分析](#adobe-analytics) | ✔ | ✔ | ✔ |
| Audience Manager |  |  |  |
| [Campaign Classic](#adobe-campaign-classic) | ✔ |  |  |
| Campaign Standard |  |  |  |
| [商務](#adobe-commerce) | ✔ | ✔ |  |
| Customer Journey Analytics |  |  |  |
| [Experience Platform標籤](#adobe-experience-platform-tags) | ✔ |  | ✔ |
| [Journey Optimizer](#adobe-journey-optimizer) |  | ✔ |  |
| [學習管理員](#adobe-learning-manager) | ✔ |  |  |
| Marketo Engage |  |  |  |
| Real-time CDP |  |  |  |
| [Sensei](#adobe-sensei) | ✔ | ✔ | ✔ |
| [目標](#adobe-target) | ✔ |  |  |
| [Workfront](#adobe-workfront) |  | ✔ |  |


## Adobe Acrobat Sign

Adobe Acrobat Sign(前身為Adobe Sign)改善法律、銷售、薪資、HR和其他領域的檔案處理工作流程，以啟用AEM Forms最適化表單的電子簽名工作流程。

### AEM Forms

+ [設定Adobe Acrobat Sign整合](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adobe-sign-integration-adaptive-forms.html)
+ [AEM Forms和Adobe Acrobat Sign教學課程](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/forms-and-sign/introduction.html)

## Adobe Analytics

Adobe Analytics與AEMas a Cloud Service的整合可讓您追蹤內容活動，並從客戶歷程的任何位置分析資料。 此外，還可獲得多功能報告、預測智慧等。

### AEM Sites

+ [設定Adobe Analytics整合](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-analytics.html)
+ [AEM Sites與Analytics教學課程](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/analytics/collect-data-analytics.html)
+ Adobe客戶端資料層(ACDL)

   + [擴充AEM WCM核心元件中的ACDL](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/extending.html)
   + [將ACDL與AEM WCM核心元件整合](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/integrations.html)
   + [使用ACDL處理事件導向資料](https://experienceleague.adobe.com/docs/adobe-developers-live-events/events/2021/oct2021/adobe-client-data-layer.html)
   + [Adobe用戶端資料層(ACDL)教學課程](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html)

### AEM Assets

+ [Assets Insights概觀](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/manage/assets-insights.html)
+ [設定Assets Insights](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/manage/assets-insights.html#configure-asset-insights)
+ [Assets Insights教學課程](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/advanced/asset-insights-launch-tutorial.html)

### AEM Forms

+ [設定Adobe Analytics整合](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/integrate-aem-forms-with-adobe-analytics.html)

## Adobe Campaign Classic

Adobe Campaign Classic與AEMas a Cloud Service的整合可讓您直接在Adobe Experience Manager中管理電子郵件傳送內容和表單，同時使用Adobe Campaign Classic進行個人化和傳送電子郵件。

### AEM Sites

+ [與Adobe Campaign Classic整合](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-campaign-classic.html#configure-user)
+ [建立Adobe Experience Manager電子報](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/creating-newsletter.html)
+ [AEM電子郵件核心元件檔案](https://github.com/adobe/aem-core-email-components#aem-email-core-components)

## Adobe Commerce

Adobe Commerce與AEMas a Cloud Service的整合，讓品牌能夠更快地擴展和創新，以區分商務體驗，並捕捉加速的線上支出。 AEM with Commerce結合了Experience Manager中身臨其境、全通路及個人化的體驗與任何數量的商務解決方案，為購物歷程的所有階段帶來與眾不同的體驗，縮短實現價值的時間，並推動更高的轉換。

### AEM Sites

+ [AEM內容與商務使用手冊](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/content-and-commerce/home.html)


## Adobe Experience Platform標籤

Adobe Experience Platform標籤(原稱為AdobeLaunch、DTM)可與AEM緊密整合，提供簡單的部署和管理方式 [analytics](#adobe-analytics), [目標定位](#adobe-target)、行銷和廣告標籤，是吸引客戶體驗的必要條件。

### AEM Sites

+ [Experience Platform標籤使用手冊](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [Experience Platform標籤教學課程](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html)

### AEM Forms

+ [Experience Platform標籤使用手冊](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [Experience Platform標籤教學課程](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html)

## Adobe Journey Optimizer

Adobe Journey Optimizer可協助您為來自單一應用程式的數百萬客戶安排全通路行銷活動和一對一交流，而整個歷程都透過智慧決策和深入分析而最佳化。

### AEM Assets

+ [整合AEM Assets Essentials與Adobe Journey Optimizer](https://experienceleague.adobe.com/docs/journey-optimizer-learn/tutorials/create-messages/create-email-content-with-the-message-editor.html)

## Adobe學習管理員

Adobe學習管理員(原稱Adobe Captivate Prime)為客戶和員工提供個人化學習。

### AEM Sites

+ [將AEM Sites與Adobe學習管理員整合](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-learning-manager.html)

## Adobe Sensei

Adobe Sensei提供AI和機器學習技術，可透過智慧標籤、智慧裁切、視覺搜尋等來轉變內容管理流程！

### AEM Sites

+ [摘要內容片段中的文字](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-variations.html#summarizing-text)

### AEM Assets

+ [影像智慧標籤](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/metadata/image-smart-tags.html)
+ [影像的自訂智慧標籤](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/metadata/custom-smart-tags.html)
+ [影片智慧標籤](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/metadata/video-smart-tags.html)
+ [智慧型裁切](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/dynamic-media/smart-crop-feature-video-use.html)
+ [視覺搜尋](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/search-and-discovery/search.html)

### AEM Forms

+ [automated forms conversion服務](https://experienceleague.adobe.com/docs/aem-forms-automated-conversion-service/using/configure-service.html)


## Adobe Target

Adobe Target與AEM as a Cloud Service整合，為每位一般使用者提供最佳化的網頁體驗，且全部由AEM的內容提供支援。

### AEM Sites

+ [設定Adobe Target整合](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html)
+ 體驗片段至Target

   + [將體驗片段發佈至Target](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html)
   + [將體驗片段發佈為JSON至Target](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html)

+ [搭配使用AEM Context Hub和Target](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/personalization/audiences.html#creating-an-adobe-target-audience-using-the-audience-console)
+ [AEM Sites和Target教學課程](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/target/overview.html)

## Adobe Workfront

Adobe Workfront與AEM的整合Cloud Service可簡化數位資產建立、協作和生命週期管理的流程。

### AEM Assets

+ [配置Workfront enhanced connector](https://experienceleague.adobe.com/docs/experience-manager-learn/assets-essentials/workfront/configure.html)
+ [Workfront enhanced connector影片](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/workfront/enhanced-connector/basics.html)
+ AEM Assets Essentials

   + [Adobe Workfront for Assets Essentials使用手冊](https://one.workfront.com/s/document-item?bundleId=the-new-workfront-experience&amp;topicId=Content%2FDocuments%2FAdobe_Workfront_for_Experience_Manager_Assets_Essentials%2F_workfront-for-aem-asset-essentials.htm)
   + [Adobe Workfront和Assets Essentials影片](https://experienceleague.adobe.com/docs/experience-manager-learn/assets-essentials/workfront/configure.html)
