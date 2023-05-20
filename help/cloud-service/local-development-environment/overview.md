---
title: 地方發展環AEM境as a Cloud Service
description: Adobe Experience Manager(AEM)地方發展環境概述。
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
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '546'
ht-degree: 14%

---

# 建立地方發展環境 {#local-development-environment-set-up}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_overview"
>title="概觀"
>abstract="為 AEM as a Cloud Service 設定本機開發環境包括開發、建置和編譯 AEM 專案所需的開發工具，以及讓開發人員透過 Adobe Cloud Manager 將新功能部署到 AEM as a Cloud Service 之前可先在本機快速進行驗證的本機執行階段。"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html" text="開發準則"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html?lang=zh-Hant" text="開發基本概念"

本教程將介紹如何使用as a Cloud ServiceSDK為Adobe Experience Manager(AEM)設定本AEM地開發環境。 包括開發、構建和編譯項目所需的開發工具AEM，以及允許開發人員在本地快速驗證新功能後，再通過Adobe雲管理器將新功能部署到AEMas a Cloud Service的本地運行時間。

>[!VIDEO](https://video.tv.adobe.com/v/32565?quality=12&learn=on)

![AEMas a Cloud Service地方開發環境技術](./assets/overview/aem-sdk-technology-stack.png)

本地開發環境AEM可分為三個邏輯組：

+ 的 __項AEM目__ 包含自定義應用程式的自定義代碼、配置和AEM內容。
+ 的 __本地運AEM行時__ 本地運行AEM Author和Publish服務的本地版本。
+ 的 __本地調度程式運行時__ 運行Apache HTTP Web Server和Dispatcher的本地版本。

本教程將介紹如何安裝和設定上圖中突出顯示的項目，為開發提供一個穩定的本地開發AEM環境。

## 檔案系統組織

本教程按如下方式建立AEM了as a Cloud ServiceSDK對象和AEM項目代碼的位置：

+ `~/aem-sdk` 是包含as a Cloud ServiceSDK提供的各種工具的組織AEM資料夾
+ `~/aem-sdk/author` 包含AEM作者服務
+ `~/aem-sdk/publish` 包含AEM發佈服務
+ `~/aem-sdk/dispatcher` 包含Dispatcher Tools
+ `~/code/<project name>` 包含自定AEM義項目原始碼

請注意 `~` 是用戶目錄的縮寫。 在Windows中，這相當於 `%HOMEPATH%`;

## 項目開發工AEM具

項AEM目是自定義代碼庫，包含通過Cloud Manager部署到as a Cloud Service的代碼、配置和內AEM容。 基線項目結構通過 [馬文AEM原型計畫](https://github.com/adobe/aem-project-archetype)。

本教程的本節介紹如何：

+ 安裝 [!DNL Java]
+ 安裝 [!DNL Node.js] （和npm）
+ 安裝 [!DNL Maven]
+ 安裝 [!DNL Git]

[設定項目開發工AEM具](./development-tools.md)

## 本機 AEM 執行階段

AEMas a Cloud ServiceSDK [!DNL QuickStart Jar] 運行本地版本的AEM。 的 [!DNL QuickStart Jar] 可用於本地運行AEM作者服務或AEM發佈服務。 請注意，當 [!DNL QuickStart Jar] 提供本地開發經驗，但並非AEMas a Cloud Service中提供的所有功能 [!DNL QuickStart Jar]。

本教程的本節介紹如何：

+ 安裝 [!DNL Java]
+ 下載AEMSDK
+ 運行 [!DNL AEM Author Service]
+ 運行 [!DNL AEM Publish Service]

[設定本地運行AEM時](./aem-runtime.md)

## 本地 [!DNL Dispatcher] 運行時

AEMas a Cloud ServiceSDK的Dispatcher Tools提供了設定本地 [!DNL Dispatcher] 運行時。 [!DNL Dispatcher] 工具是 [!DNL Docker]基於，並提供命令行工具以便傳輸 [!DNL Apache HTTP] Web伺服器和 [!DNL Dispatcher] 將配置檔案轉換為相容格式並將其部署到 [!DNL Dispatcher] 在 [!DNL Docker] 容器。

本教程的本節介紹如何：

+ 下載AEMSDK
+ 安裝 [!DNL Dispatcher] 工具
+ 運行本地 [!DNL Dispatcher] 運行時

[設定本地 [!DNL Dispatcher] 運行時](./dispatcher-tools.md)
