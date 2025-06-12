---
title: AEM as a Cloud Service 的本機開發環境
description: Adobe Experience Manager (AEM) 本機開發環境概觀。
feature: Developer Tools
version: Experience Manager as a Cloud Service
doc-type: article
jira: KT-3290
thumbnail: 32565.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-01T00:00:00Z
exl-id: 8b12f34c-be98-4f47-853c-411bb601990c
duration: 835
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '530'
ht-degree: 100%

---

# 本機開發環境設定 {#local-development-environment-set-up}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_overview"
>title="概觀"
>abstract="為 AEM as a Cloud Service 設定本機開發環境包括開發、建置和編譯 AEM 專案所需的開發工具，以及讓開發人員透過 Adobe Cloud Manager 將新功能部署到 AEM as a Cloud Service 之前可先在本機快速進行驗證的本機執行階段。"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html" text="開發準則"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html?lang=zh-Hant" text="開發基本概念"

本教學課程會逐步解說如何使用 AEM as a Cloud Service SDK 設定 Adobe Experience Manager (AEM) 的本機開發環境。其中包括開發、建置和編譯 AEM 專案所需的開發工具，以及本機執行階段，讓開發人員可以在本機快速驗證新功能，然後再透過 Adobe Cloud Manager 將新功能部署至 AEM as a Cloud Service。

>[!VIDEO](https://video.tv.adobe.com/v/32565?quality=12&learn=on)

![AEM as a Cloud Service 本機開發環境技術堆疊](./assets/overview/aem-sdk-technology-stack.png)

AEM 的本機開發環境可分為三個邏輯群組：

+ __AEM Project__，內含自訂程式碼、設定和內容，即自訂 AEM 應用程式。
+ __本機 AEM 執行階段__，在本機執行本機版本的 AEM Author 和 Publish 服務。
+ __本機 Dispatcher 執行階段__，執行本機版本的 Apache HTTP 網頁伺服器和 Dispatcher。

本教學課程會逐步解說如何安裝及設定上圖中醒目標示的項目，為 AEM 開發提供穩定的本機開發環境。

## 檔案系統組織

本教學課程已建立 AEM as a Cloud Service SDK 成品及 AEM 專案程式碼的位置，如下所示：

+ `~/aem-sdk` 是一個組織性資料夾，內含 AEM as a Cloud Service SDK 所提供的各種工具
+ `~/aem-sdk/author` 內含 AEM Author 服務
+ `~/aem-sdk/publish` 內含 AEM Publish 服務
+ `~/aem-sdk/dispatcher` 內含 Dispatcher 工具
+ `~/code/<project name>` 內含自訂 AEM 專案來源程式碼

請注意，`~` 是使用者目錄的簡寫。在 Windows 中，等同於 `%HOMEPATH%`；

## AEM 專案的開發工具

AEM 專案是自訂程式碼基底，包含透過 Cloud Manager 部署至 AEM as a Cloud Service 的程式碼、設定和內容。基準線專案結構是透過 [AEM 專案 Maven 原型](https://github.com/adobe/aem-project-archetype)所產生。

教學課程的這一個區段會介紹如何：

+ 安裝 [!DNL Java]
+ 安裝 [!DNL Node.js] (及 npm)
+ 安裝 [!DNL Maven]
+ 安裝 [!DNL Git]

[設定 AEM 專案的開發工具](./development-tools.md)

## 本機 AEM 執行階段

AEM as a Cloud Service SDK 提供執行本機版本 AEM 的 [!DNL QuickStart Jar]。[!DNL QuickStart Jar] 可用於在本機執行 AEM Author 服務或 AEM Publish 服務。請注意，雖然 [!DNL QuickStart Jar] 提供本機開發體驗，但是並非 AEM as a Cloud Service 所提供的全部功能皆包含在 [!DNL QuickStart Jar] 中。

教學課程的這一個區段會介紹如何：

+ 安裝 [!DNL Java]
+ 下載 AEM SDK
+ 執行 [!DNL AEM Author Service]
+ 執行 [!DNL AEM Publish Service]

[設定本機 AEM 執行階段](./aem-runtime.md)

## 本機 [!DNL Dispatcher] 執行階段

AEM as a Cloud Service SDK 的 Dispatcher 工具會提供設定本機 [!DNL Dispatcher] 執行階段時所需的一切。[!DNL Dispatcher] 工具以 [!DNL Docker] 為基礎並提供命令列工具，可以將 [!DNL Apache HTTP] 網頁伺服器及 [!DNL Dispatcher] 設定檔案轉換為相容的格式，並將其部署至在 [!DNL Docker] 容器中執行的 [!DNL Dispatcher]。

教學課程的這一個區段會介紹如何：

+ 下載 AEM SDK
+ 安裝 [!DNL Dispatcher] 工具
+ 執行本機 [!DNL Dispatcher] 執行階段

[設定本機  [!DNL Dispatcher]  執行階段](./dispatcher-tools.md)
