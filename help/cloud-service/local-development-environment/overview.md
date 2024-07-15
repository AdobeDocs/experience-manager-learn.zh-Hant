---
title: AEM as a Cloud Service的本機開發環境
description: Adobe Experience Manager (AEM)本機開發環境概觀。
feature: Developer Tools
version: Cloud Service
doc-type: article
jira: KT-3290
thumbnail: 32565.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-01T00:00:00Z
exl-id: 8b12f34c-be98-4f47-853c-411bb601990c
duration: 835
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '530'
ht-degree: 12%

---

# 本機開發環境設定 {#local-development-environment-set-up}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_overview"
>title="概觀"
>abstract="為 AEM as a Cloud Service 設定本機開發環境包括開發、建置和編譯 AEM 專案所需的開發工具，以及讓開發人員透過 Adobe Cloud Manager 將新功能部署到 AEM as a Cloud Service 之前可先在本機快速進行驗證的本機執行階段。"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html?lang=zh-Hant" text="開發準則"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html?lang=zh-Hant" text="開發基本概念"

本教學課程會逐步解說如何使用AEM as a Cloud Service SDK為Adobe Experience Manager (AEM)設定本機開發環境。 其中包括開發、建置及編譯AEM專案所需的開發工具，以及可讓開發人員透過AdobeCloud Manager將新功能部署到AEM as a Cloud Service之前，先在本機快速驗證這些功能的本機執行時間。

>[!VIDEO](https://video.tv.adobe.com/v/32565?quality=12&learn=on)

![AEM as a Cloud Service本機開發環境技術棧疊](./assets/overview/aem-sdk-technology-stack.png)

AEM的本機開發環境可以分成三個邏輯群組：

+ __AEM專案__&#x200B;包含自訂AEM應用程式的自訂程式碼、設定和內容。
+ 在本機執行AEM Author和Publish服務的本機版本的&#x200B;__本機AEM執行階段__。
+ 執行Apache HTTP Web Server和Dispatcher本機版本的&#x200B;__本機Dispatcher執行階段__。

本教學課程將逐步說明如何安裝和設定上圖中醒目提示的專案，為AEM開發提供穩定的本機開發環境。

## 檔案系統組織

本教學課程已建立AEM as a Cloud Service SDK成品和AEM專案程式碼的位置，如下所示：

+ `~/aem-sdk`是包含AEM as a Cloud Service SDK所提供各種工具的組織資料夾
+ `~/aem-sdk/author`包含AEM作者服務
+ `~/aem-sdk/publish`包含AEM Publish服務
+ `~/aem-sdk/dispatcher`包含Dispatcher工具
+ `~/code/<project name>`包含自訂AEM專案原始碼

請注意，`~`是使用者目錄的簡稱。 在Windows中，這相當於`%HOMEPATH%`；

## AEM專案的開發工具

AEM專案是自訂程式碼基底，包含透過Cloud Manager部署至AEM as a Cloud Service的程式碼、設定和內容。 基準專案結構是透過[AEM專案Maven原型](https://github.com/adobe/aem-project-archetype)產生。

教學課程的此區段會示範如何：

+ 安裝[!DNL Java]
+ 安裝[!DNL Node.js] （和npm）
+ 安裝[!DNL Maven]
+ 安裝[!DNL Git]

[設定AEM專案的開發工具](./development-tools.md)

## 本機 AEM 執行階段

AEM as a Cloud Service SDK提供的[!DNL QuickStart Jar]可執行本機版本的AEM。 [!DNL QuickStart Jar]可用於在本機執行AEM Author Service或AEM Publish Service。 請注意，雖然[!DNL QuickStart Jar]提供本機開發體驗，但並非所有AEM as a Cloud Service中可用的功能都包含在[!DNL QuickStart Jar]中。

教學課程的此區段會示範如何：

+ 安裝[!DNL Java]
+ 下載AEM SDK
+ 執行[!DNL AEM Author Service]
+ 執行[!DNL AEM Publish Service]

[設定本機AEM執行階段](./aem-runtime.md)

## 本機[!DNL Dispatcher]執行階段

AEM as a Cloud Service SDK的Dispatcher Tools提供設定本機[!DNL Dispatcher]執行階段所需的一切。 [!DNL Dispatcher]工具以[!DNL Docker]為基礎，並提供命令列工具來將[!DNL Apache HTTP]網頁伺服器和[!DNL Dispatcher]設定檔傳輸成相容的格式，並將它們部署到[!DNL Docker]容器中執行的[!DNL Dispatcher]。

教學課程的此區段會示範如何：

+ 下載AEM SDK
+ 安裝[!DNL Dispatcher]工具
+ 執行本機[!DNL Dispatcher]執行階段

[設定本機 [!DNL Dispatcher] 執行階段](./dispatcher-tools.md)
