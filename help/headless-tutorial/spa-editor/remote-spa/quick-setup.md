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

+ [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=zh-Hant)
+ [Node.js v18](https://nodejs.org/en/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ 僅限macOS的必要條件
   + [Xcode](https://developer.apple.com/xcode/)或[Xcode命令列工具](https://developer.apple.com/xcode/resources/)
+ [aem-guides-wknd.all-2.1.0.zip或更新版本](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql原始碼（分支： feature/spa-editor）](https://github.com/adobe/aem-guides-wknd-graphql/tree/feature/spa-editor)


本教學課程假設：

+ [Microsoft® Visual Studio Code](https://visualstudio.microsoft.com/)作為IDE
+ `~/Code/wknd-app`的工作目錄
+ 在`http://localhost:4502`上以Author服務的形式執行AEM SDK
+ 使用密碼為`admin`的本機`admin`帳戶執行AEM SDK
+ 正在`http://localhost:3000`上執行SPA

## 啟動AEM SDK快速入門

使用預設的`admin/admin`認證在連線埠4502上下載並安裝AEM SDK快速入門。

1. [下載最新的AEM SDK](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)
1. 將AEM SDK解壓縮至`~/aem-sdk`
1. 執行AEM SDK快速入門Jar

   ```
   $ java -jar aem-sdk-quickstart-xxx.jar
   
   # Provide `admin` as the admin user's password
   ```

AEM SDK會在[http://localhost:4502](http://localhost:4502)上啟動並自動啟動。 使用下列憑證登入：

+ 使用者名稱： `admin`
+ 密碼： `admin`

## 下載及安裝WKND站台套件

此教學課程依存於&#x200B;__WKND 2.1.0+的__&#x200B;專案（針對內容）。

1. [下載最新版本的`aem-guides-wknd.all.x.x.x.zip`](https://github.com/adobe/aem-guides-wknd/releases)
1. 使用`admin`認證登入[http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr)的AEM SDK封裝管理員。
1. __上傳__&#x200B;在步驟1中下載的`aem-guides-wknd.all.x.x.x.zip`
1. 點選專案`aem-guides-wknd.all-x.x.x.zip`的&#x200B;__安裝__&#x200B;按鈕

## 下載及安裝WKND應用程式SPA套件

若要執行快速設定，此處提供AEM套件，其中包含教學課程的最後一個AEM設定和內容。

1. [下載 ](./assets/quick-setup/wknd-app.all-1.0.0-SNAPSHOT.zip)
1. [下載 ](./assets/quick-setup/wknd-app.ui.content.sample-1.0.1.zip)
1. 使用`admin`認證登入[http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr)的AEM SDK封裝管理員。
1. __上傳__&#x200B;在步驟1中下載的`wknd-app.all.x.x.x.zip`
1. 點選專案`wknd-app.all.x.x.x.zip`的&#x200B;__安裝__&#x200B;按鈕
1. __上傳__&#x200B;在步驟2中下載的`wknd-app.ui.content.sample.x.x.x.zip`
1. 點選專案`wknd-app.ui.content.sample.x.x.x.zip`的&#x200B;__安裝__&#x200B;按鈕

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

如果在執行`npm install`時發生錯誤，請嘗試下列步驟：

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ rm -f package-lock.json
$ npm install --legacy-peer-deps
$ npm run start
```

驗證SPA是否在[http://localhost:3000](http://localhost:3000)執行。

## 在AEM SPA編輯器中編寫內容

在編寫內容之前，請安排瀏覽器視窗，讓AEM Author (`http://localhost:4502`)在左側，遠端SPA (`http://localhost:3000`)在右側。 這種排列方式可讓您檢視對AEM來源內容的變更如何立即反映在SPA中。

1. 以`admin`身分登入[AEM SDK作者服務](http://localhost:4502)
1. 導覽至&#x200B;__網站> WKND應用程式>我們> en__
1. 編輯&#x200B;__WKND應用程式首頁__
1. 切換至&#x200B;__編輯__&#x200B;模式

### 編寫首頁檢視的固定元件

1. 點選文字&#x200B;__WKND Adventures__&#x200B;以啟動固定標題元件(以硬式編碼寫入SPA首頁檢視)
1. 點選「標題」元件動作列上的&#x200B;__扳手__&#x200B;圖示
1. 變更標題元件的內容並儲存
1. 重新整理在`http://localhost:3000`上執行的SPA，並檢視變更是否反映出來

### 編寫首頁檢視的容器元件

1. 仍在編輯&#x200B;__WKND應用程式首頁__&#x200B;時……
1. 展開&#x200B;__SPA編輯器的側欄__ （在左側）
1. 點選&#x200B;__元件__&#x200B;圖示
1. 從容器元件新增、變更或移除元件，這些元件位於WKND標誌下方以及固定標題元件上方
1. 重新整理在`http://localhost:3000`上執行的SPA，並檢視變更是否反映出來

### 在動態路由上編寫容器元件

1. 在SPA編輯器中切換至&#x200B;__預覽__&#x200B;模式
1. 點選&#x200B;__巴厘島衝浪營__&#x200B;卡，並導覽至其動態路線
1. 新增、變更或移除位於&#x200B;__行程__&#x200B;標題上方的容器元件
1. 重新整理在`http://localhost:3000`上執行的SPA，並檢視變更是否反映出來

__WKND應用程式首頁> Adventure__ _下的新AEM頁面必須_&#x200B;具有與對應冒險內容片段名稱相符的AEM頁面名稱。 這是因為SPA到AEM頁面對應的路由是根據路由的最後一個區段，也就是內容片段的名稱。

## 恭喜！

您剛剛快速瞭解了AEM SPA Editor如何透過可編輯的控制區域來增強SPA！ 如果您有興趣 — 請檢視教學課程的其餘部分，但請務必重新開始，因為在此快速設定中，您的本機開發環境現在處於教學課程的結束狀態！
