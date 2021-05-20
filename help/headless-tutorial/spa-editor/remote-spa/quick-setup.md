---
title: 快速設定SPA Editor與遠端SPA
description: 了解如何在15分鐘內使用遠端SPA和AEM SPA編輯器開始運作！
topic: 無頭、SPA、開發
feature: SPA編輯器，核心元件， API，開發
role: Developer, Architect
level: Beginner
kt: 7629
thumbnail: 333181.jpg
source-git-commit: 73c75f8dac85615f4ed2dfdcc2ee4d0e9e5d161a
workflow-type: tm+mt
source-wordcount: '801'
ht-degree: 4%

---


# 快速設定

快速設定是快速逐步說明如何安裝及執行WKND應用程式和作為遠端SPA，並使用AEM SPA編輯器編寫。

快速設定會直接進入本教學課程的結束狀態。

>[!VIDEO](https://video.tv.adobe.com/v/333181/?quality=12&learn=on)

_快速設定的影片逐步說明_

## 必備條件

本教學課程需要下列項目：

+ [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v14+](https://nodejs.org/en/)
+ [npm v7+](https://www.npmjs.com/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ 僅限macOS的必要條件
   + [](https://developer.apple.com/xcode/) Xcode或 [Xcode命令列工具](https://developer.apple.com/xcode/resources/)
+ [aem-guides-wknd.all.0.3.0zip或更新版本](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql原始碼](https://github.com/adobe/aem-guides-wknd-graphql)


本教學課程假設：

+ [Microsoft® Visual Studio代](https://visualstudio.microsoft.com/) 碼作為IDE
+ `~/Code/wknd-app`的工作目錄
+ 在`http://localhost:4502`上以作者服務形式執行AEM SDK
+ 使用密碼為`admin`的本機`admin`帳戶執行AEM SDK
+ 在`http://localhost:3000`上執行SPA

## 啟動AEM SDK快速入門

在連接埠4502上下載並安裝具有預設`admin/admin`憑證的AEM SDK Quickstart。

1. [下載最新AEM SDK](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)
1. 將AEM SDK解壓縮至`~/aem-sdk`
1. 執行AEM SDK Quickstart Jar

   ```
   $ java -jar aem-sdk-quickstart-xxx.jar
   
   # Provide `admin` as the admin user's password
   ```

AEM SDK將在[http://localhost:4502](http://localhost:4502)上啟動並自動啟動。 使用下列憑證登入：

+ 使用者名稱: `admin`
+ 密碼: `admin`

## 下載並安裝WKND站點包

本教學課程相依於&#x200B;__WKND 0.3.0+的__&#x200B;專案（針對內容）。

1. [下載最新版本  `aem-guides-wknd.all.x.x.x.zip`](https://github.com/adobe/aem-guides-wknd/releases)
1. 使用`admin`憑證登入AEM SDK的套件管理器，網址為[http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr)。
1. ____ 上傳 `aem-guides-wknd.all.x.x.x.zip` 在步驟1下載的
1. 點選&#x200B;__Install__&#x200B;按鈕以取得`aem-guides-wknd.all-x.x.x.zip`項目

## 下載並安裝WKND應用程式SPA套件

若要快速設定，會提供AEM套件，其中包含教學課程的最終AEM設定和內容。

1. [下載 ](./assets/quick-setup/wknd-app.all-1.0.0-SNAPSHOT.zip)
1. [下載 ](./assets/quick-setup/wknd-app.ui.content.sample-1.0.0.zip)
1. 使用`admin`憑證登入AEM SDK的套件管理器，網址為[http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr)。
1. ____ 上傳 `wknd-app.all.x.x.x.zip` 在步驟1下載的
1. 點選&#x200B;__Install__&#x200B;按鈕以取得`wknd-app.all.x.x.x.zip`項目
1. ____ 上傳 `wknd-app.ui.content.sample.x.x.x.zip` 在步驟2中下載的
1. 點選&#x200B;__Install__&#x200B;按鈕以取得`wknd-app.ui.content.sample.x.x.x.zip`項目

## 下載WKND應用程式來源

從Github.com下載WKND應用程式的原始碼，並切換包含本教學課程中執行之SPA變更的分支。

```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ git clone https://github.com/adobe/aem-guides-wknd-graphql.git
$ cd aem-guides-wknd-graphql
$ git checkout -b feature/spa-editor
$ git pull origin feature/spa-editor
```

## 啟動SPA應用程式

從專案的根目錄，安裝SPA專案npm相依性並執行應用程式。

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ npm install
$ npm run start
```

如果運行`npm install`時出錯，請嘗試下列步驟：

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ rm -f package-lock.json
$ npm install --legacy-peer-deps
$ npm run start
```

驗證SPA是否在[http://localhost:3000](http://localhost:3000)執行。

## 在AEM SPA編輯器中製作內容

製作內容之前，請先排列瀏覽器視窗，讓AEM Author(`http://localhost:4502`)位於左側，而遠端SPA(`http://localhost:3000`)則位於右側。 此安排可讓您查看AEM來源內容的變更如何立即反映在SPA中。

1. 以`admin`登入[AEM SDK作者服務](http://localhost:4502)
1. 導覽至「__Sites > WKND應用程式> us > en__」
1. 編輯&#x200B;__WKND應用首頁__
1. 切換到&#x200B;__Edit__&#x200B;模式

### 製作首頁檢視的固定元件

1. 點選文字&#x200B;__WKND Adventures__&#x200B;以啟動固定標題元件(硬式編碼至SPA首頁檢視)
1. 點選「標題」元件動作列上的&#x200B;__扳手__&#x200B;圖示
1. 變更標題元件的內容並儲存
1. 重新整理`http://localhost:3000`上執行的SPA，並查看是否反映變更

### 製作首頁檢視的容器元件

1. 仍在編輯&#x200B;__WKND應用首頁__&#x200B;時……
1. 展開&#x200B;__SPA編輯器的側欄__（位於左側）
1. 點選&#x200B;__元件__&#x200B;圖示
1. 從位於WKND標誌下方和固定標題元件上方的容器元件中添加、更改或刪除元件
1. 重新整理`http://localhost:3000`上執行的SPA，並查看是否反映變更

### 在動態路線上製作容器元件

1. 在SPA編輯器中切換至&#x200B;__預覽__&#x200B;模式
1. 點選&#x200B;__Bali Surf Camp__&#x200B;卡片並導覽至其動態路線
1. 從位於&#x200B;__Interial__&#x200B;標題上方的容器元件中新增、變更或移除元件
1. 重新整理`http://localhost:3000`上執行的SPA，並查看是否反映變更

__WKND應用程式首頁>冒險__ _下的新AEM頁面必須_&#x200B;具有與對應冒險內容片段名稱相符的AEM頁面名稱。 這是因為SPA路由到AEM頁面的映射基於路由的最後一段，即內容片段的名稱。

## 恭喜！

您只需快速了解AEM SPA Editor如何透過可控、可編輯的區域增強SPA! 如果您有興趣，請查看教學課程的其餘部分，但請務必從頭開始，因為在此快速設定中，您的本機開發環境現在處於教學課程的結束狀態！
