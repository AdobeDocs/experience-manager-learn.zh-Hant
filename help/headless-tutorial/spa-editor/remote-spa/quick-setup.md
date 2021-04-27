---
title: 快速設SPA定編輯器與遠端SPA
description: 瞭解如何在15分鐘內使用遠端和編SPA輯AEM器啟SPA動並執行！
topic: 無頭、SPA開發
feature: 編SPA輯器、核心元件、API、開發
role: Developer, Architect
level: Beginner
kt: 7629
thumbnail: kt-7629.jpeg
translation-type: tm+mt
source-git-commit: ba45a52b9dac44f0c08c819cf4778dba83f325c9
workflow-type: tm+mt
source-wordcount: '729'
ht-degree: 5%

---


# 快速設定

快速設定是快速的逐步說明，說明如何安裝和執行WKND應用程式，並以遠端身SPA份使用編輯器來AEM制SPA作它。

快速設定會直接帶您進入本教學課程的結束狀態。

## 必備條件

本教學課程需要：

+ [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v14+](https://nodejs.org/en/)
+ [npm v7+](https://www.npmjs.com/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ [aem-guides-wknd.all.0.3.0.zip或更新版本](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql原始碼](https://github.com/adobe/aem-guides-wknd-graphql)

本教學課程假設：

+ [Microsoft® Visual Studio代](https://visualstudio.microsoft.com/) 碼作為IDE
+ `~/Code/wknd-app`的工作目錄
+ 在&lt;AEMa0/>上以作者服務的身分執行SDK`http://localhost:4502`
+ 使用密碼AEM為`admin`的本機`admin`帳戶執行SDK
+ 在SPA`http://localhost:3000`上運行

## 啟動AEMSDK快速入門

使用預設的AEM`admin/admin`認證，在埠4502上下載並安裝SDK快速入門。

1. [下載最新AEM的SDK](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)
1. 將AEMSDK解壓縮至`~/aem-sdk`
1. 執行AEMSDK快速入門

   ```
   $ java -jar aem-sdk-quickstart-xxx.jar
   
   # Provide `admin` as the admin user's password
   ```

SDKAEM將在[http://localhost:4502](http://localhost:4502)上啟動並自動啟動。 使用下列憑證登入：

+ 使用者名稱: `admin`
+ 密碼: `admin`

## 下載並安裝WKND網站套件

本教程依賴於&#x200B;__WKND 0.3.0+的__&#x200B;項目（內容）。

1. [下載最新版本的  `aem-guides-wknd.all.x.x.x.zip`](https://github.com/adobe/aem-guides-wknd/releases)
1. 使用AEM`admin`憑證登入SDK的Package Manager，網址為[http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr)。
1. __上__ 載步 `aem-guides-wknd.all.x.x.x.zip` 驟1中下載的
1. 點選&#x200B;__Install__&#x200B;按鈕以取得`aem-guides-wknd.all-x.x.x.zip`項目

## 下載並安裝WKND應用程SPA式套件

要執行快速設定，AEM將提供包含教程最終配置和內AEM容的軟體包。

1. [下載 `wknd-app.all.x.x.x.zip`](./assets/quick-setup/wknd-app.all-1.0.0-SNAPSHOT.zip)
1. [下載 `wknd-app.ui.content.sample.x.x.x.zip`](./assets/quick-setup/wknd-app.ui.content.sample-1.0.0.zip)
1. 使用AEM`admin`憑證登入SDK的Package Manager，網址為[http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr)。
1. __上__ 載步 `wknd-app.all.x.x.x.zip` 驟1中下載的
1. 點選&#x200B;__Install__&#x200B;按鈕以取得`wknd-app.all.x.x.x.zip`項目
1. __上__ 載步 `wknd-app.ui.content.sample.x.x.x.zip` 驟2中下載的
1. 點選&#x200B;__Install__&#x200B;按鈕以取得`wknd-app.ui.content.sample.x.x.x.zip`項目

## 下載WKND應用程式來源

從Github.com下載WKND App的原始碼，並切換包含本教學課程中所執行變更SPA的分支。

```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
$ git checkout -b feature/spa-editor
$ git pull origin feature/spa-editor
```

## 啟動應SPA用程式

從項目的根目錄中，安裝項SPA目npm相關性並運行應用程式。

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ npm install
$ npm run start
```

如果運行`npm install`時出現錯誤，請嘗試以下步驟：

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ rm -f package-lock.json
$ npm install --legacy-peer-deps
$ npm run start
```

驗證SPA運行位置為[http://localhost:3000](http://localhost:3000)。

## 在編輯器中製作AEM內SPA容

製作內容之前，請先將瀏覽器視窗排列為AEM Author(`http://localhost:4502`)位於左側，SPA而遠端(`http://localhost:3000`)則位於右側。 此安排可讓您瞭解來源內容AEM的變更如何立即反映在SPA中。

1. 登入[AEM SDK作者服務](http://localhost:4502)作為`admin`
1. 導覽至「__網站> WKND應用程式>我們> en__」
1. 編輯&#x200B;__WKND應用程式首頁__
1. 切換到&#x200B;__編輯__&#x200B;模式

### 編寫「首頁」視圖的固定元件

1. 點選文字&#x200B;__WKND Adventures__&#x200B;以啟動固定標題元件(硬式編碼至首頁SPA檢視)
1. 點選Title元件動作列上的&#x200B;__wrench__&#x200B;圖示
1. 變更Title元件的內容並儲存
1. 重新整SPA理`http://localhost:3000`上的執行，並查看所反映的變更

### 製作首頁檢視的容器元件

1. 仍在編輯&#x200B;__WKND應用程式首頁__&#x200B;時……
1. 展開&#x200B;__SPA Editor的邊欄__（在左側）
1. 點選&#x200B;__元件__&#x200B;圖示
1. 從位於WKND標誌下方及固定標題元件上方的容器元件中新增、變更或移除元件
1. 重新整SPA理`http://localhost:3000`上的執行，並查看所反映的變更

### 在動態路由上編寫容器元件

1. 在編輯器中切換至&#x200B;__預覽__&#x200B;模SPA式
1. 點選&#x200B;__Bali Surf Camp__&#x200B;卡並導覽至其動態路線
1. 從位於&#x200B;__Interial__&#x200B;標題上方的容器元件新增、變更或移除元件
1. 重新整SPA理`http://localhost:3000`上的執行，並查看所反映的變更

## 恭喜！

您只需快速瞭解Editor如何AEM運用可控SPA制、可編SPA輯的區域來增強您的效能！ 如果您有興趣——請查看教學課程的其餘部分，但請務必重新開始，因為在此快速設定中，您的本機開發環境現在已進入教學課程的結束狀態！
