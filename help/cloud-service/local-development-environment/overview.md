---
title: AEM as aCloud Service的本機開發環境
description: Adobe Experience Manager(AEM)本機開發環境概觀。
feature: 開發人員工具
topics: development
version: cloud-service
doc-type: article
activity: troubleshoot
audience: developer
kt: 3290
thumbnail: 32565.jpg
topic: 開發
role: Developer
level: Beginner
source-git-commit: 9a78cbdb5fd35e4aa7169382494dd014aa8098e9
workflow-type: tm+mt
source-wordcount: '549'
ht-degree: 2%

---


# 本地開發環境設定

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_overview"
>title="概覽"
>abstract="為AEM as aCloud Service設定本機開發環境，包括開發、建置和編譯AEM專案所需的開發工具，以及本機執行時間，讓開發人員在本機快速驗證新功能，再透過Adobe Cloud Manager部署至AEM as aCloud Service。"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html" text="開發准則"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html" text="開發基本知識"

本教學課程將逐步說明如何使用AEM作為Cloud ServiceSDK，為Adobe Experience Manager(AEM)設定本機開發環境。 其中包括開發、建置和編譯AEM專案所需的開發工具，以及本機執行時間，讓開發人員在本機快速驗證新功能，再透過Adobe Cloud Manager將新功能部署至AEM作為Cloud Service。

>[!VIDEO](https://video.tv.adobe.com/v/32565/?quality=12&learn=on)

![AEM as aCloud Service本機開發環境技術堆疊](./assets/overview/aem-sdk-technology-stack.png)

AEM的本機開發環境可分割為三個邏輯群組：

+ __AEM專案__&#x200B;包含自訂AEM應用程式的自訂程式碼、設定和內容。
+ __本機AEM執行階段__&#x200B;會在本機執行AEM製作和發佈服務的本機版本。
+ __本機Dispatcher執行階段__&#x200B;會執行Apache HTTP Web伺服器和Dispatcher的本機版本。

本教學課程會逐步說明如何安裝和設定上圖中強調的項目，為AEM開發提供穩定的本機開發環境。

## 檔案系統組織

本教學課程建立了AEM的位置，作為Cloud ServiceSDK成品和AEM專案程式碼，如下所示：

+ `~/aem-sdk` 是組織資料夾，內含AEM as aCloud ServiceSDK提供的各種工具
+ `~/aem-sdk/author` 包含AEM Author Service
+ `~/aem-sdk/publish` 包含AEM發佈服務
+ `~/aem-sdk/dispatcher` 包含Dispatcher工具
+ `~/code/<project name>` 包含自訂AEM專案原始碼

請注意，`~`是「用戶目錄」的簡稱。 在Windows中，這等同於`%HOMEPATH%`;

## AEM專案開發工具

AEM專案是自訂程式碼基底，包含透過Cloud Manager部署至AEM作為Cloud Service的程式碼、設定和內容。 基線專案結構是透過[AEM專案Maven原型](https://github.com/adobe/aem-project-archetype)產生。

本教學課程的本節說明如何：

+ 安裝 [!DNL Java]
+ 安裝[!DNL Node.js]（和npm）
+ 安裝 [!DNL Maven]
+ 安裝 [!DNL Git]

[設定AEM專案的開發工具](./development-tools.md)

## 本機AEM執行階段

AEM as a Cloud ServiceSDK提供[!DNL QuickStart Jar]，可執行AEM的本機版本。 [!DNL QuickStart Jar]可用來在本機執行AEM製作服務或AEM發佈服務。 請注意，雖然[!DNL QuickStart Jar]提供本機開發體驗，但並非AEM as aCloud Service中提供的所有功能都包含在[!DNL QuickStart Jar]中。

本教學課程的本節說明如何：

+ 安裝 [!DNL Java]
+ 下載AEM SDK
+ 運行[!DNL AEM Author Service]
+ 運行[!DNL AEM Publish Service]

[設定本機AEM執行階段](./aem-runtime.md)

## 本地[!DNL Dispatcher]運行時

AEM as aCloud ServiceSDK的Dispatcher工具提供設定本機[!DNL Dispatcher]執行階段所需的一切。 [!DNL Dispatcher] 工具是基 [!DNL Docker]於的，並提供命令行工具，以將Web伺 [!DNL Apache HTTP] 服器和配 [!DNL Dispatcher] 置檔案以相容的格式傳輸，並將它們部署 [!DNL Dispatcher] 到容器中 [!DNL Docker] 運行。

本教學課程的本節說明如何：

+ 下載AEM SDK
+ 安裝[!DNL Dispatcher]工具
+ 運行本地[!DNL Dispatcher]運行時

[設定Local [!DNL Dispatcher] Runtime](./dispatcher-tools.md)
