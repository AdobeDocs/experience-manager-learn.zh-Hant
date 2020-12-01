---
title: AEM雲端服務的本機開發環境
description: Adobe Experience Manager(AEM)本機開發環境概觀。
feature: null
topics: development
version: cloud-service
doc-type: article
activity: troubleshoot
audience: developer
kt: 3290
thumbnail: 32565.jpg
translation-type: tm+mt
source-git-commit: 69c1767098cc9da8ec0ae2bd83d25417d330f393
workflow-type: tm+mt
source-wordcount: '477'
ht-degree: 0%

---


# 地方開發環境設定

本教學課程將逐步介紹如何使用AEM做為Cloud Service SDK，為Adobe Experience Manager(AEM)設定本機開發環境。 其中包括開發、建立和編譯AEM專案所需的開發工具，以及本機執行時間，讓開發人員在透過Adobe Cloud Manager將新功能部署至AEM做為雲端服務之前，先在本機快速驗證新功能。

>[!VIDEO](https://video.tv.adobe.com/v/32565/?quality=12&learn=on)

![AEM作為雲端服務本端開發環境技術堆疊](./assets/overview/aem-sdk-technology-stack.png)

AEM的本機開發環境可分為三個邏輯群組：

+ __AEM Project__&#x200B;包含自訂AEM應用程式的自訂程式碼、設定和內容。
+ __本機AEM Runtime__&#x200B;會在本機執行AEM Author和Publish服務的本機版本。
+ __Local Dispatcher Runtime__，它運行Apache HTTP Web Server和Dispatcher的本地版本。

本教學課程將逐步說明如何安裝和設定上圖中反白顯示的項目，為AEM開發提供穩定的本機開發環境。

## 檔案系統組織

本教學課程將AEM的位置建立為Cloud Service SDK工件和AEM專案程式碼，如下所示：

+ `~/aem-sdk` 是組織資料夾，內含AEM以雲端服務SDK形式提供的各種工具
+ `~/aem-sdk/author` 包含AEM Author Service
+ `~/aem-sdk/publish` 包含AEM Publish Service
+ `~/aem-sdk/dispatcher` 包含Dispatcher Tools
+ `~/code/<project name>` 包含自訂AEM Project原始碼

請注意，`~`是用戶目錄的速記。 在Windows中，這相當於`%HOMEPATH%`;

## AEM專案的開發工具

AEM專案是自訂的程式碼庫，包含透過Cloud Manager部署至AEM的程式碼、設定和內容，做為雲端服務。 基線項目結構是透過[AEM Project Maven Archetype](https://github.com/adobe/aem-project-archetype)產生。

教學課程的本節說明如何：

+ 安裝 [!DNL Java]
+ 安裝[!DNL Node.js]（和npm）
+ 安裝 [!DNL Maven]
+ 安裝 [!DNL Git]

[設定AEM專案的開發工具](./development-tools.md)

## 本機AEM Runtime

AEM as a Cloud Service SDK提供[!DNL QuickStart Jar]，可執行AEM的本機版本。 [!DNL QuickStart Jar]可用來在本機執行AEM Author Service或AEM Publish Service。 請注意，雖然[!DNL QuickStart Jar]提供本機開發體驗，但[!DNL QuickStart Jar]中並未包含AEM中所有雲端服務可用的功能。

教學課程的本節說明如何：

+ 安裝 [!DNL Java]
+ 下載AEM SDK
+ 運行[!DNL AEM Author Service]
+ 運行[!DNL AEM Publish Service]

[設定本機AEM執行階段](./aem-runtime.md)

## 本機[!DNL Dispatcher]執行時期

AEM as a Cloud Service SDK&#39;s Dispatcher Tools提供設定本機[!DNL Dispatcher]執行階段所需的一切。 [!DNL Dispatcher] 工具是以 [!DNL Docker]命令列工具為基礎，可將 [!DNL Apache HTTP] Web Server和組態檔 [!DNL Dispatcher] 轉換為相容格式，並將它們部署至容 [!DNL Dispatcher] 器中執 [!DNL Docker] 行。

教學課程的本節說明如何：

+ 下載AEM SDK
+ 安裝[!DNL Dispatcher]工具
+ 執行本機[!DNL Dispatcher]執行時期

[設定 [!DNL Dispatcher] LocalRuntime](./dispatcher-tools.md)
