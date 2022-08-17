---
title: 快速安裝編SPA輯器和遠程設SPA備
description: 瞭解如何在15分鐘內使用遠程和編SPA輯AEM器啟SPA動和運行！
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7629
thumbnail: 333181.jpg
exl-id: ef7a1dad-993a-4c47-a9fb-91fa73de9b5d
source-git-commit: fe056006ab59a3955e5f16a23e96e9e208408cf5
workflow-type: tm+mt
source-wordcount: '797'
ht-degree: 4%

---

# 快速設定

快速設定是快速的瀏覽，演示了如何安裝和運行WKND應用，並作為遠程程式，SPA然後使用編AEM輯器編SPA寫。

快速設定將直接引導您進入本教程的結束狀態。

>[!VIDEO](https://video.tv.adobe.com/v/333181/?quality=12&learn=on)

_快速設定的視頻瀏覽_

## 必備條件

本教程需要以下內容：

+ [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v16+](https://nodejs.org/en/)
+ [npm v8+](https://www.npmjs.com/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [馬文3.6+](https://maven.apache.org/)
+ [蠢貨](https://git-scm.com/downloads)
+ macOS僅先決條件
   + [Xcode](https://developer.apple.com/xcode/) 或 [Xcode命令行工具](https://developer.apple.com/xcode/resources/)
+ [aem guides-wknd.all-2.1.0.zip或更高版本](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql原始碼(分支：功能/spa編輯器](https://github.com/adobe/aem-guides-wknd-graphql/tree/feature/spa-editor)


本教程假定：

+ [Microsoft® Visual Studio代碼](https://visualstudio.microsoft.com/) 作為IDE
+ 工作目錄 `~/Code/wknd-app`
+ 將SDKAEM作為作者服務運行於 `http://localhost:4502`
+ 使用AEM本地 `admin` 密碼帳戶 `admin`
+ 運行SPA `http://localhost:3000`

## 啟動AEMSDK快速啟動

在埠4502上AEM下載並安裝SDK快速啟動，預設為 `admin/admin` 憑據。

1. [下載最新AEMSDK](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)
1. 將SDK解AEM壓縮到 `~/aem-sdk`
1. 運行AEMSDK快速啟動Jar

   ```
   $ java -jar aem-sdk-quickstart-xxx.jar
   
   # Provide `admin` as the admin user's password
   ```

SDKAEM啟動並自動啟動 [http://localhost:4502](http://localhost:4502)。 使用以下憑據登錄：

+ 使用者名稱: `admin`
+ 密碼: `admin`

## 下載並安裝WKND站點包

本教程依賴於 __WKND 2.1.0+__ 項目（內容）。

1. [下載最新版本 `aem-guides-wknd.all.x.x.x.zip`](https://github.com/adobe/aem-guides-wknd/releases)
1. 登錄到AEMSDK的包管理器 [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) 和 `admin` 憑據。
1. __上載__ 這樣 `aem-guides-wknd.all.x.x.x.zip` 在步驟1中下載
1. 點擊 __安裝__ 按鈕 `aem-guides-wknd.all-x.x.x.zip`

## 下載並安裝WKND應用程SPA序包

要執行快速設定，AEM此處提供了包含教程最終配置和內AEM容的包。

1. [下載 ](./assets/quick-setup/wknd-app.all-1.0.0-SNAPSHOT.zip)
1. [下載 ](./assets/quick-setup/wknd-app.ui.content.sample-1.0.1.zip)
1. 登錄到AEMSDK的包管理器 [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) 和 `admin` 憑據。
1. __上載__ 這樣 `wknd-app.all.x.x.x.zip` 在步驟1中下載
1. 點擊 __安裝__ 按鈕 `wknd-app.all.x.x.x.zip`
1. __上載__ 這樣 `wknd-app.ui.content.sample.x.x.x.zip` 在步驟2中下載
1. 點擊 __安裝__ 按鈕 `wknd-app.ui.content.sample.x.x.x.zip`

## 下載WKND應用程式源

從Github.com下載WKND App的原始碼，並切換包含對本教程中執行的更改的SPA分支。

```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ git clone --branch feature/spa-editor https://github.com/adobe/aem-guides-wknd-graphql.git
$ cd aem-guides-wknd-graphql
```

## 啟動應SPA用程式

從項目的根目錄中，安裝項SPA目npm依賴項並運行應用程式。

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ npm install
$ npm run start
```

如果運行時有錯誤 `npm install` 嘗試以下步驟：

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ rm -f package-lock.json
$ npm install --legacy-peer-deps
$ npm run start
```

驗證SPA運行於 [http://localhost:3000](http://localhost:3000)。

## 在編輯器中創AEM作內SPA容

創作內容之前，請排列瀏覽器窗口，以便AEM作者(`http://localhost:4502`)位於左側，遠SPA程`http://localhost:3000`)在右邊運行。 此安排允許您查看源內容AEM的更改如何立即反映在SPA中。

1. 登錄到 [SDKAEM作者服務](http://localhost:4502) 如 `admin`
1. 導航到 __站點> WKND應用>我們> en__
1. 編輯 __WKND應用首頁__
1. 切換到 __編輯__ 模式

### 編寫「首頁」視圖的固定元件

1. 點擊文本 __WKND冒險__ 激活固定的標題元件(硬編碼到「SPA首頁」視圖)
1. 點擊 __扳__ 表徵圖
1. 更改標題元件的內容並保存
1. 刷新SPA運行時間 `http://localhost:3000` 看看這些變化

### 建立「首頁」視圖的容器元件

1. 仍在編輯 __WKND應用首頁__...
1. 展開 __編SPA輯欄__ （左）
1. 點擊 __元件__ 表徵圖
1. 添加、更改或移除位於WKND徽標下方和固定標題元件上方的容器元件中的元件
1. 刷新SPA運行時間 `http://localhost:3000` 看看這些變化

### 在動態路徑上建立容器元件

1. 切換到 __預覽__ 編輯器中SPA的模式
1. 點擊 __巴釐島衝浪營__ 卡並導航其動態路由
1. 添加、更改或刪除位於 __行程__ 標題
1. 刷新SPA運行時間 `http://localhost:3000` 看看這些變化

下AEM面的新頁 __WKND應用首頁>冒險__ _必須_ 具有與AEM相應冒險內容片段的名稱匹配的頁名。 這是因為SPA到頁AEM面映射的路由基於路由的最後一段，即內容片段的名稱。

## 恭喜！

您只需快速瞭解AEMEditor如何SPA通過可控可編SPA輯區域增強您的功能！ 如果您感興趣 — 請查看本教程的其餘部分，但確保重新開始，因為在此快速設定中，您的本地開發環境現在處於本教程的結束狀態！
