---
title: 快速設定SPA Editor與遠端SPA
description: 了解如何在15分鐘內使用遠端SPA和AEM SPA編輯器開始運作！
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7629
thumbnail: 333181.jpg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
exl-id: ef7a1dad-993a-4c47-a9fb-91fa73de9b5d
source-git-commit: 38a35fe6b02e9aa8c448724d2e83d1aefd8180e7
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# 快速設定

快速設定是快速逐步說明如何安裝及執行WKND應用程式和作為遠端SPA，並使用AEM SPA編輯器編寫。

快速設定會直接進入本教學課程的結束狀態。

>[!VIDEO](https://video.tv.adobe.com/v/333181?quality=12&learn=on)

_快速設定的影片逐步說明_

## 必備條件

本教學課程需要下列項目：

+ [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v18](https://nodejs.org/en/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ 僅限macOS先決條件
   + [Xcode](https://developer.apple.com/xcode/) 或 [Xcode命令列工具](https://developer.apple.com/xcode/resources/)
+ [aem-guides-wknd.all-2.1.0.zip或更新版本](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql原始碼(分支：feature/spa-editor)](https://github.com/adobe/aem-guides-wknd-graphql/tree/feature/spa-editor)


本教學課程假設：

+ [Microsoft® Visual Studio代碼](https://visualstudio.microsoft.com/) 作為IDE
+ 的工作目錄 `~/Code/wknd-app`
+ 在上執行AEM SDK as a Author服務 `http://localhost:4502`
+ 使用本機執行AEM SDK `admin` 密碼帳戶 `admin`
+ 在上執行SPA `http://localhost:3000`

## 啟動AEM SDK快速入門

在連接埠4502上下載並安裝AEM SDK Quickstart（預設） `admin/admin` 憑證。

1. [下載最新AEM SDK](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)
1. 將AEM SDK解壓縮至 `~/aem-sdk`
1. 執行AEM SDK Quickstart Jar

   ```
   $ java -jar aem-sdk-quickstart-xxx.jar
   
   # Provide `admin` as the admin user's password
   ```

AEM SDK會在 [http://localhost:4502](http://localhost:4502). 使用下列憑證登入：

+ 使用者名稱: `admin`
+ 密碼: `admin`

## 下載並安裝WKND站點包

本教學課程依賴於 __WKND 2.1.0+__ 專案（內容）。

1. [下載最新版本 `aem-guides-wknd.all.x.x.x.zip`](https://github.com/adobe/aem-guides-wknd/releases)
1. 登入AEM SDK的套件管理器，網址為 [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) 和 `admin` 憑證。
1. __上傳__ the `aem-guides-wknd.all.x.x.x.zip` 在步驟1下載
1. 點選 __安裝__ 按鈕 `aem-guides-wknd.all-x.x.x.zip`

## 下載並安裝WKND應用程式SPA套件

若要快速設定，此處提供AEM套件，其中包含教學課程的最終AEM設定和內容。

1. [下載 ](./assets/quick-setup/wknd-app.all-1.0.0-SNAPSHOT.zip)
1. [下載 ](./assets/quick-setup/wknd-app.ui.content.sample-1.0.1.zip)
1. 登入AEM SDK的套件管理器，網址為 [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) 和 `admin` 憑證。
1. __上傳__ the `wknd-app.all.x.x.x.zip` 在步驟1下載
1. 點選 __安裝__ 按鈕 `wknd-app.all.x.x.x.zip`
1. __上傳__ the `wknd-app.ui.content.sample.x.x.x.zip` 在步驟2中下載
1. 點選 __安裝__ 按鈕 `wknd-app.ui.content.sample.x.x.x.zip`

## 下載WKND應用程式來源

從Github.com下載WKND應用程式的原始碼，並切換包含本教學課程中執行之SPA變更的分支。

```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ git clone --branch feature/spa-editor https://github.com/adobe/aem-guides-wknd-graphql.git
$ cd aem-guides-wknd-graphql
```

## 啟動SPA應用程式

從專案的根目錄，安裝SPA專案npm相依性並執行應用程式。

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ npm install
$ npm run start
```

如果執行時發生錯誤 `npm install` 請嘗試下列步驟：

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ rm -f package-lock.json
$ npm install --legacy-peer-deps
$ npm run start
```

驗證SPA是否在執行 [http://localhost:3000](http://localhost:3000).

## 在AEM SPA編輯器中製作內容

編寫內容前，請先排列瀏覽器視窗，以便AEM製作(`http://localhost:4502`)位於左側，而遠端SPA(`http://localhost:3000`)在右側執行。 此安排可讓您查看AEM來源內容的變更如何立即反映在SPA中。

1. 登入 [AEM SDK作者服務](http://localhost:4502) as `admin`
1. 導覽至 __網站> WKND應用程式>我們>結束__
1. 編輯 __WKND應用首頁__
1. 切換至 __編輯__ 模式

### 製作首頁檢視的固定元件

1. 點選文字 __WKND冒險__ 啟用固定的Title元件(硬式編碼至SPA首頁檢視)
1. 點選 __扳手__ 表徵圖
1. 變更標題元件的內容並儲存
1. 重新整理執行中的SPA `http://localhost:3000` 並看到變更反映

### 製作首頁檢視的容器元件

1. 仍在編輯 __WKND應用首頁__...
1. 展開 __SPA編輯器的側欄__ （在左側）
1. 點選 __元件__ 表徵圖
1. 從位於WKND標誌下方和固定標題元件上方的容器元件中添加、更改或刪除元件
1. 重新整理執行中的SPA `http://localhost:3000` 並看到變更反映

### 在動態路線上製作容器元件

1. 切換至 __預覽__ 模式(在SPA編輯器中)
1. 點選 __巴釐島衝浪營__ 卡片並導航至其動態路線
1. 從位於 __行程__ 標題
1. 重新整理執行中的SPA `http://localhost:3000` 並看到變更反映

新的AEM頁面位於 __WKND應用首頁>冒險__ _必須_ 有一個AEM頁面名稱，該名稱與對應冒險的「內容片段」名稱相符。 這是因為SPA路由到AEM頁面的映射基於路由的最後一段，即內容片段的名稱。

## 恭喜！

您只需快速了解AEM SPA Editor如何透過可控、可編輯的區域增強SPA! 如果您有興趣，請查看教學課程的其餘部分，但請務必從頭開始，因為在此快速設定中，您的本機開發環境現在處於教學課程的結束狀態！
