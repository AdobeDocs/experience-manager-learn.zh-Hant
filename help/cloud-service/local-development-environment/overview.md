---
title: 適用於AEMas a Cloud Service的本機開發環境
description: Adobe Experience Manager(AEM)本機開發環境概觀。
feature: Developer Tools
version: Cloud Service
doc-type: article
kt: 3290
thumbnail: 32565.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-01T00:00:00Z
exl-id: 8b12f34c-be98-4f47-853c-411bb601990c
source-git-commit: d0b13fd37f1ed42042431246f755a913b56625ec
workflow-type: tm+mt
source-wordcount: '546'
ht-degree: 5%

---

# 本地開發環境設定 {#local-development-environment-set-up}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_overview"
>title="概觀"
>abstract="為AEMas a Cloud Service設定本機開發環境，包括開發、建置和編譯AEM專案所需的開發工具，以及本機執行時間，讓開發人員在本機快速驗證新功能，再透過Adobe Cloud Manager將其部署至AEMas a Cloud Service。"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html" text="開發指導方針"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html?lang=zh-Hant" text="開發基本概念"

本教學課程會逐步說明如何使用AEMas a Cloud Service SDK設定Adobe Experience Manager(AEM)的本機開發環境。 其中包括開發、建置和編譯AEM專案所需的開發工具，以及本機執行時間，讓開發人員可在本機快速驗證新功能，再透過AdobeCloud Manager部署至AEMas a Cloud Service。

>[!VIDEO](https://video.tv.adobe.com/v/32565/?quality=12&learn=on)

![AEMas a Cloud Service本機開發環境技術堆疊](./assets/overview/aem-sdk-technology-stack.png)

AEM的本機開發環境可分割為三個邏輯群組：

+ 此 __AEM專案__ 包含自訂AEM應用程式的自訂程式碼、設定和內容。
+ 此 __本機AEM執行階段__ 會在本機執行AEM製作和發佈服務的本機版本。
+ 此 __本機Dispatcher執行階段__ 會執行本機版本的Apache HTTP Web Server和Dispatcher。

本教學課程會逐步說明如何安裝和設定上圖中強調的項目，為AEM開發提供穩定的本機開發環境。

## 檔案系統組織

本教學課程建立了AEMas a Cloud ServiceSDK成品和AEM專案程式碼的位置，如下所示：

+ `~/aem-sdk` 是組織資料夾，內含AEMas a Cloud Service SDK提供的各種工具
+ `~/aem-sdk/author` 包含AEM Author Service
+ `~/aem-sdk/publish` 包含AEM發佈服務
+ `~/aem-sdk/dispatcher` 包含Dispatcher工具
+ `~/code/<project name>` 包含自訂AEM專案原始碼

請注意 `~` 是用戶目錄的簡稱。 在Windows中，這等同於 `%HOMEPATH%`;

## AEM專案開發工具

AEM專案是自訂程式碼基底，包含透過Cloud Manager部署至AEMas a Cloud Service的程式碼、設定和內容。 基線專案結構會透過 [AEM專案Maven原型](https://github.com/adobe/aem-project-archetype).

本教學課程的本節說明如何：

+ 安裝 [!DNL Java]
+ 安裝 [!DNL Node.js] （和npm）
+ 安裝 [!DNL Maven]
+ 安裝 [!DNL Git]

[設定AEM專案的開發工具](./development-tools.md)

## 本機AEM執行階段

AEMas a Cloud ServiceSDK提供 [!DNL QuickStart Jar] 執行本機版本的AEM。 此 [!DNL QuickStart Jar] 可用來在本機執行AEM Author Service或AEM Publish Service。 請注意，若 [!DNL QuickStart Jar] 提供本機開發體驗，但AEMas a Cloud Service中提供的所有功能並未包含在 [!DNL QuickStart Jar].

本教學課程的本節說明如何：

+ 安裝 [!DNL Java]
+ 下載AEM SDK
+ 執行 [!DNL AEM Author Service]
+ 執行 [!DNL AEM Publish Service]

[設定本機AEM執行階段](./aem-runtime.md)

## 本地 [!DNL Dispatcher] 執行階段

AEMas a Cloud ServiceSDK的Dispatcher工具提供設定本機SDK所需的一切 [!DNL Dispatcher] 執行階段。 [!DNL Dispatcher] 工具包括 [!DNL Docker]-based和為傳輸提供命令行工具 [!DNL Apache HTTP] Web伺服器和 [!DNL Dispatcher] 將配置檔案轉換為相容的格式，並部署至 [!DNL Dispatcher] 在 [!DNL Docker] 容器。

本教學課程的本節說明如何：

+ 下載AEM SDK
+ 安裝 [!DNL Dispatcher] 工具
+ 運行本地 [!DNL Dispatcher] 執行階段

[設定本機 [!DNL Dispatcher] 執行階段](./dispatcher-tools.md)
