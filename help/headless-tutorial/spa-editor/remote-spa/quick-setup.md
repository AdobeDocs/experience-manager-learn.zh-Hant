---
title: 快速設定SPA編輯器和遠端SPA
description: 瞭解如何在15分鐘內啟動並執行遠端SPA和AEM SPA Editor！
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer
level: Beginner
jira: KT-7629
thumbnail: 333181.jpg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: ef7a1dad-993a-4c47-a9fb-91fa73de9b5d
duration: 647
hide: true
source-git-commit: f95907146983d2315d48f793d38ebb1172a7bae4
workflow-type: tm+mt
source-wordcount: '838'
ht-degree: 13%

---

# 快速設定

{{spa-editor-deprecation}}

快速設定是快速逐步解說，說明如何安裝和執行WKND應用程式及當做遠端SPA，並使用AEM SPA編輯器加以撰寫。

快速設定會直接將您帶至本教學課程的結束狀態。

>[!VIDEO](https://video.tv.adobe.com/v/333181?quality=12&learn=on)

_快速設定的影片逐步解說_

## 先決條件

本教學課程需要以下項目：

+ [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=zh-Hant)
+ [Node.js v18](https://nodejs.org/en/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ 僅限macOS的必要條件
   + [Xcode](https://developer.apple.com/xcode/)或[Xcode命令列工具](https://developer.apple.com/xcode/resources/)
+ [aem-guides-wknd.all-2.1.0.zip或更高版本](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql原始碼(branch： feature/spa-editor)](https://github.com/adobe/aem-guides-wknd-graphql/tree/feature/spa-editor)


本教學課程假設：

+ 以 [Microsoft® Visual Studio 程式碼](https://visualstudio.microsoft.com/)作為 IDE
+ 工作目錄是 `~/Code/wknd-app`
+ 執行 AEM SDK，作為 `http://localhost:4502` 上的製作服務
+ 使用具備 `admin` 密碼的本機 `admin` 帳戶執行 AEM SDK
+ 在 `http://localhost:3000` 上執行 SPA

## 啟動AEM SDK快速入門

使用預設的`admin/admin`認證在連線埠4502上下載並安裝AEM SDK Quickstart。

1. [下載最新的AEM SDK](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&orderby.sort=desc&layout=list&p.offset=0&p.limit=1)
1. 將AEM SDK解壓縮至`~/aem-sdk`
1. 執行AEM SDK Quickstart Jar

   ```
   $ java -jar aem-sdk-quickstart-xxx.jar
   
   # Provide `admin` as the admin user's password
   ```

AEM SDK會在[http://localhost:4502](http://localhost:4502)上啟動並自動啟動。 Log in using the following credentials:

+ Username: `admin`
+ Password: `admin`

## Download and install WKND Site package

This tutorial has a dependency on __WKND 2.1.0+&#39;s__ project (for content).

1. [Download the latest version of `aem-guides-wknd.all.x.x.x.zip`](https://github.com/adobe/aem-guides-wknd/releases)
1. Log in to AEM SDK&#39;s Package Manager at [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) with the `admin` credentials.
1. __Upload__ the `aem-guides-wknd.all.x.x.x.zip` downloaded in step 1
1. Tap the __Install__ button for the entry `aem-guides-wknd.all-x.x.x.zip`

## Download and install WKND App SPA packages

To perform a quick setup, AEM packages are provided here that contain the tutorial&#39;s final  AEM configuration and content.

1. [Download `wknd-app.all.x.x.x.zip`](./assets/quick-setup/wknd-app.all-1.0.0-SNAPSHOT.zip)
1. [Download `wknd-app.ui.content.sample.x.x.x.zip`](./assets/quick-setup/wknd-app.ui.content.sample-1.0.1.zip)
1. Log in to AEM SDK&#39;s Package Manager at [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) with the `admin` credentials.
1. __Upload__ the `wknd-app.all.x.x.x.zip` downloaded in step 1
1. Tap the __Install__ button for the entry `wknd-app.all.x.x.x.zip`
1. __Upload__ the `wknd-app.ui.content.sample.x.x.x.zip` downloaded in step 2
1. Tap the __Install__ button for the entry `wknd-app.ui.content.sample.x.x.x.zip`

## Download the WKND App source

Download the WKND App&#39;s source code by from Github.com, and switch the branch containing the changes to the SPA performed in this tutorial.

```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ git clone --branch feature/spa-editor https://github.com/adobe/aem-guides-wknd-graphql.git
$ cd aem-guides-wknd-graphql
```

## Start the SPA application

From the project&#39;s root, install the SPA projects npm dependencies and run the application.

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ npm install
$ npm run start
```

If there are errors when running `npm install` try the following steps:

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ rm -f package-lock.json
$ npm install --legacy-peer-deps
$ npm run start
```

Verify that the SPA is running at [http://localhost:3000](http://localhost:3000).

## Author content in AEM SPA Editor

Before authoring content arrange your browser windows such that AEM Author (`http://localhost:4502`) is on the left, and the remote SPA  (`http://localhost:3000`) runs on the right. 這種排列方式可讓您檢視對AEM來源內容的變更如何立即反映在SPA中。

1. 以`admin`身分登入[AEM SDK作者服務](http://localhost:4502)
1. 導覽至&#x200B;__網站> WKND應用程式>我們> en__
1. 編輯&#x200B;__WKND應用程式首頁__
1. 切換至&#x200B;__編輯__&#x200B;模式

### 編寫首頁檢視的固定元件

1. 點選文字&#x200B;__WKND Adventures__&#x200B;以啟動固定標題元件（以硬式編碼寫入SPA的「首頁」檢視）
1. 點選「標題」元件動作列上的&#x200B;__扳手__&#x200B;圖示
1. 變更標題元件的內容並儲存
1. 重新整理在`http://localhost:3000`上執行的SPA，並檢視變更是否反映出來

### 編寫首頁檢視的容器元件

1. 仍在編輯&#x200B;__WKND應用程式首頁__&#x200B;時……
1. 展開&#x200B;__SPA Editor的側欄__ （在左側）
1. 點選&#x200B;__元件__&#x200B;圖示
1. 從容器元件新增、變更或移除元件，這些元件位於WKND標誌下方以及固定標題元件上方
1. 重新整理在`http://localhost:3000`上執行的SPA，並檢視變更是否反映出來

### 在動態路由上編寫容器元件

1. 在SPA編輯器中切換至&#x200B;__預覽__&#x200B;模式
1. 點選&#x200B;__巴厘島衝浪營__&#x200B;卡，並導覽至其動態路線
1. 新增、變更或移除位於&#x200B;__行程__&#x200B;標題上方的容器元件
1. 重新整理在`http://localhost:3000`上執行的SPA，並檢視變更是否反映出來

__WKND應用程式首頁> Adventure__ _下的新AEM頁面必須_&#x200B;具有與對應冒險內容片段名稱相符的AEM頁面名稱。 這是因為AEM頁面對應的SPA路由是根據路由的最後一個區段，也就是內容片段的名稱。

## 恭喜！

您剛剛快速瞭解了AEM SPA Editor如何透過可編輯的受控區域來增強SPA！ 如果您有興趣 — 請檢視教學課程的其餘部分，但請務必重新開始，因為在此快速設定中，您的本機開發環境現在處於教學課程的結束狀態！
