---
title: 快速設定SPA編輯器和遠端SPA
description: 瞭解如何在15分鐘內啟動並執行遠端SPA和AEM SPA Editor！
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7629
thumbnail: 333181.jpg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: ef7a1dad-993a-4c47-a9fb-91fa73de9b5d
duration: 647
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '726'
ht-degree: 2%

---

# 快速設定

快速設定是快速逐步解說，說明如何安裝和執行WKND應用程式及當做遠端SPA，並使用AEM SPA編輯器加以撰寫。

快速設定會直接將您帶至本教學課程的結束狀態。

>[!VIDEO](https://video.tv.adobe.com/v/333181?quality=12&learn=on)

_快速設定的影片逐步解說_

## 先決條件

本教學課程需要下列內容：

+ [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v18](https://nodejs.org/en/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ 僅限macOS的必要條件
   + [Xcode](https://developer.apple.com/xcode/) 或 [Xcode命令列工具](https://developer.apple.com/xcode/resources/)
+ [aem-guides-wknd.all-2.1.0.zip或更高版本](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql原始碼(branch： feature/spa-editor)](https://github.com/adobe/aem-guides-wknd-graphql/tree/feature/spa-editor)


本教學課程假設：

+ [Microsoft® Visual Studio Code](https://visualstudio.microsoft.com/) 作為IDE
+ 的工作目錄 `~/Code/wknd-app`
+ 以作者服務的形式執行AEM SDK，於 `http://localhost:4502`
+ 使用本機執行AEM SDK `admin` 具有密碼的帳戶 `admin`
+ 執行SPA於 `http://localhost:3000`

## 啟動AEM SDK快速入門

在連線埠4502上下載並安裝AEM SDK快速入門（預設值） `admin/admin` 認證。

1. [下載最新AEM SDK](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)
1. 將AEM SDK解壓縮至 `~/aem-sdk`
1. 執行AEM SDK快速入門Jar

   ```
   $ java -jar aem-sdk-quickstart-xxx.jar
   
   # Provide `admin` as the admin user's password
   ```

AEM SDK啟動並自動啟動於 [http://localhost:4502](http://localhost:4502). 使用下列憑證登入：

+ 使用者名稱： `admin`
+ 密碼： `admin`

## 下載及安裝WKND站台套件

本教學課程的相關性為 __WKND 2.1.0+的__ 專案（針對內容）。

1. [下載最新版本的 `aem-guides-wknd.all.x.x.x.zip`](https://github.com/adobe/aem-guides-wknd/releases)
1. 於登入AEM SDK的封裝管理員 [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) 使用 `admin` 認證。
1. __上傳__ 此 `aem-guides-wknd.all.x.x.x.zip` 已在步驟1下載
1. 點選 __安裝__ 專案按鈕 `aem-guides-wknd.all-x.x.x.zip`

## 下載及安裝WKND應用程式SPA套件

若要執行快速設定，此處提供AEM套件，其中包含教學課程的最後一個AEM設定和內容。

1. [下載 ](./assets/quick-setup/wknd-app.all-1.0.0-SNAPSHOT.zip)
1. [下載 ](./assets/quick-setup/wknd-app.ui.content.sample-1.0.1.zip)
1. 於登入AEM SDK的封裝管理員 [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) 使用 `admin` 認證。
1. __上傳__ 此 `wknd-app.all.x.x.x.zip` 已在步驟1下載
1. 點選 __安裝__ 專案按鈕 `wknd-app.all.x.x.x.zip`
1. __上傳__ 此 `wknd-app.ui.content.sample.x.x.x.zip` 已在步驟2下載
1. 點選 __安裝__ 專案按鈕 `wknd-app.ui.content.sample.x.x.x.zip`

## 下載WKND應用程式來源

請由Github.com下載WKND應用程式的原始程式碼，然後切換包含本教學課程中所執行SPA變更的分支。

```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ git clone --branch feature/spa-editor https://github.com/adobe/aem-guides-wknd-graphql.git
$ cd aem-guides-wknd-graphql
```

## 啟動SPA應用程式

從專案的根目錄中，安裝SPA專案npm相依性並執行應用程式。

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

驗證SPA執行於 [http://localhost:3000](http://localhost:3000).

## 在AEM SPA編輯器中編寫內容

在製作內容之前，請安排您的瀏覽器視窗，讓AEM Author (`http://localhost:4502`)在左側，而遠端SPA (`http://localhost:3000`)會在右側執行。 這種排列方式可讓您檢視對AEM來源內容的變更如何立即反映在SPA中。

1. 登入 [AEM SDK作者服務](http://localhost:4502) 作為 `admin`
1. 瀏覽至 __網站> WKND應用程式>我們>英文__
1. 編輯 __WKND應用程式首頁__
1. 切換至 __編輯__ 模式

### 編寫首頁檢視的固定元件

1. 點選文字 __WKND冒險__ 以啟動固定標題元件(以硬式編碼方式編入SPA首頁檢視)
1. 點選 __扳手__ 圖示於「標題」元件的動作列
1. 變更標題元件的內容並儲存
1. 重新整理正在執行的SPA `http://localhost:3000` 並看到變更反映在

### 編寫首頁檢視的容器元件

1. 同時仍在編輯 __WKND應用程式首頁__...
1. 展開 __SPA編輯器的側欄__ （左側）
1. 點選 __元件__ 圖示
1. 從容器元件新增、變更或移除元件，這些元件位於WKND標誌下方以及固定標題元件上方
1. 重新整理正在執行的SPA `http://localhost:3000` 並看到變更反映在

### 在動態路由上編寫容器元件

1. 切換至 __預覽__ SPA編輯器中的模式
1. 點選 __巴厘島衝浪營__ 卡片並導覽至其動態路由
1. 新增、變更或移除容器元件（位於容器元件上方）中的元件。 __行程__ 標題
1. 重新整理正在執行的SPA `http://localhost:3000` 並看到變更反映在

下的新AEM頁面 __WKND應用程式首頁>冒險__ _必須_ 具有與對應冒險的內容片段名稱相符的AEM頁面名稱。 這是因為SPA到AEM頁面對應的路由是根據路由的最後一個區段，也就是內容片段的名稱。

## 恭喜！

您剛剛快速瞭解了AEM SPA Editor如何透過可編輯的控制區域來增強SPA！ 如果您有興趣 — 請檢視教學課程的其餘部分，但請務必重新開始，因為在此快速設定中，您的本機開發環境現在處於教學課程的結束狀態！
