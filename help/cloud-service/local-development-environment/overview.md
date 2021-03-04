---
title: 地方開發環AEM境以Cloud Service
description: Adobe Experience Manager(AEM)地方開發環境概觀。
feature: 開發人員工具
topics: development
version: cloud-service
doc-type: article
activity: troubleshoot
audience: developer
kt: 3290
thumbnail: 32565.jpg
topic: 開發
role: 開發人員
level: 初學者
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '482'
ht-degree: 2%

---


# 地方開發環境設定

本教學課程將逐步介紹如何使用Cloud ServiceSDK為Adobe Experience Manager(AEM)設AEM定本機開發環境。 其中包括開發、建立和編譯專案所需的開發工具，以及本機執行時AEM間，讓開發人員可在本機快速驗證新功能，然後再透過Adobe雲端管理器將新功能部AEM署為Cloud Service。

>[!VIDEO](https://video.tv.adobe.com/v/32565/?quality=12&learn=on)

![作AEM為Cloud Service地方開發環境技術堆棧](./assets/overview/aem-sdk-technology-stack.png)

本地開發環AEM境可分為三個邏輯組：

+ __AEM Project__&#x200B;包含自訂應用程式的自訂程式碼、設定和內AEM容。
+ __本機執AEM行階段__&#x200B;會在本機執行AEM Author和Publish服務的本機版本。
+ __Local Dispatcher Runtime__，它運行Apache HTTP Web Server和Dispatcher的本地版本。

本教學課程將逐步說明如何安裝和設定上圖中反白顯示的項目，為開發提供穩定的本機開AEM發環境。

## 檔案系統組織

本教學課程將SDK的位AEM置建立為Cloud ServiceSDK工件AEM和項目代碼：

+ `~/aem-sdk` 是組織資料夾，包含以Cloud ServiceSDKAEM形式提供的各種工具
+ `~/aem-sdk/author` 包含AEM Author Service
+ `~/aem-sdk/publish` 包含AEM Publish Service
+ `~/aem-sdk/dispatcher` 包含Dispatcher Tools
+ `~/code/<project name>` 包含自訂AEM專案原始碼

請注意，`~`是用戶目錄的速記。 在Windows中，這相當於`%HOMEPATH%`;

## 專案開發AEM工具

專AEM案是自訂的程式碼庫，包含透過Cloud Manager部署的程式碼、設定和內容AEM做為Cloud Service。 基線項目結構通過[AEM Project Maven Archetype](https://github.com/adobe/aem-project-archetype)生成。

教學課程的本節說明如何：

+ 安裝 [!DNL Java]
+ 安裝[!DNL Node.js]（和npm）
+ 安裝 [!DNL Maven]
+ 安裝 [!DNL Git]

[設定專案的開發工AEM具](./development-tools.md)

## 本機執AEM行時期

作AEM為Cloud ServiceSDK提供[!DNL QuickStart Jar]，可執行本機版本AEM。 [!DNL QuickStart Jar]可用來在本機執行AEM Author Service或AEM Publish Service。 請注意，雖然[!DNL QuickStart Jar]提供本機開發經驗，但[!DNL QuickStart Jar]中並未包含作AEM為Cloud Service的所有可用功能。

教學課程的本節說明如何：

+ 安裝 [!DNL Java]
+ 下載AEMSDK
+ 運行[!DNL AEM Author Service]
+ 運行[!DNL AEM Publish Service]

[設定本機執行AEM時期](./aem-runtime.md)

## 本機[!DNL Dispatcher]執行時期

因AEM為Cloud ServiceSDK的Dispatcher Tools提供了設定本地[!DNL Dispatcher]運行時所需的一切。 [!DNL Dispatcher] 工具是以 [!DNL Docker]命令列工具為基礎，可將 [!DNL Apache HTTP] Web Server和組態檔 [!DNL Dispatcher] 轉換為相容格式，並將它們部署至容 [!DNL Dispatcher] 器中執 [!DNL Docker] 行。

教學課程的本節說明如何：

+ 下載AEMSDK
+ 安裝[!DNL Dispatcher]工具
+ 執行本機[!DNL Dispatcher]執行時期

[設定 [!DNL Dispatcher] LocalRuntime](./dispatcher-tools.md)
