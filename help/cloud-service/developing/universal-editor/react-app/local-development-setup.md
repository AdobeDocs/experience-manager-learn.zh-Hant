---
title: 本機開發設定
description: 了解如何設定通用編輯器適用的本機開發環境，以便您可以編輯範例 React 應用程式的內容。
version: Experience Manager as a Cloud Service
feature: Developer Tools, Headless
topic: Development, Content Management
role: Developer
level: Intermediate
doc-type: Tutorial
duration: 189
last-substantial-update: 2024-04-19T00:00:00Z
jira: KT-15359
thumbnail: KT-15359.png
exl-id: 47bef697-5253-493a-b9f9-b26c27d2db56
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '787'
ht-degree: 100%

---

# 本機開發設定

了解如何設定本機開發環境，以便使用 AEM 通用編輯器來編輯 React 應用程式的內容。

## 先決條件

若要依照本教學課程內容進行，以下為必要條件：

- 基本的 HTML 和 JavaScript 技能。
- 必須在本機安裝以下工具：
   - [Node.js](https://nodejs.org/en/download/)
   - [Git](https://git-scm.com/downloads)
   - IDE 或程式碼編輯器，例如 [Visual Studio Code](https://code.visualstudio.com/)
- 下載並安裝以下項目：
   - [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime#download-the-aem-as-a-cloud-service-sdk)：包含在本機執行 AEM Author 和 Publish 以進行開發所使用的 Quickstart Jar。
   - [通用編輯器服務](https://experienceleague.adobe.com/zh-hant/docs/experience-cloud/software-distribution/home)：通用編輯器服務的本機副本，具有部分功能，並可以從軟體分發入口網站下載。
   - [local-ssl-proxy](https://www.npmjs.com/package/local-ssl-proxy#local-ssl-proxy)：一個使用自我簽署憑證來進行本機開發的簡單本機 SSL HTTP Proxy。AEM 通用編輯器需要 React 應用程式的 HTTPS URL 才能將其載入編輯器中。

## 本機設定

請按照下列步驟設定本機開發環境：

### AEM SDK

若要為 WKND Teams React 應用程式提供內容，請在本機 AEM SDK 中安裝下列封裝。

- [WKND Teams - 內容套件](./assets/basic-tutorial-solution.content.zip)：包含內容片段模型、內容片段和存留的 GraphQL 查詢。
- [WKND Teams - 設定封裝](./assets/basic-tutorial-solution.ui.config.zip)：包含跨來源資源共用 (CORS) 和權杖驗證處理常式設定。CORS 讓非 AEM 網頁屬性可以進行瀏覽器型用戶端呼叫以存取 AEM 的 GraphQL API，而權杖驗證處理常式則用於驗證每個發送到 AEM 的請求。

  ![WKND Teams - 封裝](./assets/wknd-teams-packages.png)

### React 應用程式

要設定 WKND Teams React 應用程式，請按照以下步驟操作：

1. 原地複製 `basic-tutorial` 解決方案分支中的 [WKND Teams React 應用程式](https://github.com/adobe/aem-guides-wknd-graphql/tree/solution/basic-tutorial)。

   ```bash
   $ git clone -b solution/basic-tutorial git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 導覽至 `basic-tutorial` 目錄並在程式碼編輯器中開啟目錄。

   ```bash
   $ cd aem-guides-wknd-graphql/basic-tutorial
   $ code .
   ```

1. 安裝相依性並啟動 React 應用程式。

   ```bash
   $ npm install
   $ npm start
   ```

1. 在瀏覽器中連結至 [http://localhost:3000](http://localhost:3000)，開啟 WKND Teams React 應用程式。接著會顯示團隊成員清單及其詳細資訊。React 應用程式的內容由本機 AEM SDK 使用 GraphQL API (`/graphql/execute.json/my-project/all-teams`) 提供，而您可以使用瀏覽器的網路標籤驗證這件事。

   ![WKND Teams - React 應用程式](./assets/wknd-teams-react-app.png)

### 通用編輯器服務

若要設定&#x200B;**本機**&#x200B;通用編輯器服務，請按照下列步驟操作：

1. 從[軟體分發入口網站](https://experience.adobe.com/downloads)下載最新版本的通用編輯器服務。

   ![軟體分發 - 通用編輯器服務](./assets/universal-editor-service.png)

1. 將所下載的 zip 檔案解壓縮並複製 `universal-editor-service.cjs` 檔案到名為 `universal-editor-service` 的新目錄。

   ```bash
   $ unzip universal-editor-service-vproduction-<version>.zip
   $ mkdir universal-editor-service
   $ cp universal-editor-service.cjs universal-editor-service
   ```

1. 在 `universal-editor-service` 目錄中建立 `.env` 檔案，並新增下列環境變數：

   ```bash
   # The port on which the Universal Editor service runs
   UES_PORT=8000
   # Disable SSL verification
   UES_TLS_REJECT_UNAUTHORIZED=false
   ```

1. 啟動本機通用編輯器服務。

   ```bash
   $ cd universal-editor-service
   $ node universal-editor-service.cjs
   ```

上述命令在 `8000` 連接埠上啟動通用編輯器服務，而您應該會看到以下輸出：

```bash
Either no private key or certificate was set. Starting as HTTP server
Universal Editor Service listening on port 8000 as HTTP Server
```

### 本機 SSL HTTP Proxy

AEM 通用編輯器必須安裝 React 應用程式才能透過 HTTPS 提供服務。我們設定一個使用自我簽署憑證的本機 SSL HTTP Proxy 進行本機開發。

請按照下列步驟設定本機 SSL HTTP Proxy 並透過 HTTPS 提供 AEM SDK 和通用編輯器服務：

1. 全域安裝 `local-ssl-proxy` 封裝。

   ```bash
   $ npm install -g local-ssl-proxy
   ```

1. 啟動兩個本機 SSL HTTP Proxy 實例來執行下列服務：

   - 為 AEM SDK 啟動的本機 SSL HTTP Proxy，使用連接埠 `8443`。
   - 為通用編輯器服務啟動的本機 SSL HTTP Proxy，使用連接埠 `8001`。

   ```bash
   # AEM SDK local SSL HTTP proxy on port 8443
   $ local-ssl-proxy --source 8443 --target 4502
   
   # Universal Editor service local SSL HTTP proxy on port 8001
   $ local-ssl-proxy --source 8001 --target 8000
   ```

### 更新 React 應用程式以便使用 HTTPS

若要讓 WKND Teams React 應用程式使用 HTTPS，請按照下列步驟操作：

1. 在終端機中按下 `Ctrl + C` 來停止 React。
1. 更新 `package.json` 檔案，將 `HTTPS=true` 環境變數包含在 `start` 指令碼中。

   ```json
   "scripts": {
       "start": "HTTPS=true react-scripts start",
       ...
   }
   ```

1. 更新 `.env.development` 檔案中的 `REACT_APP_HOST_URI`，以便使用 HTTPS 通訊協定以及 AEM SDK 的本機 SSL HTTP Proxy 連接埠。

   ```bash
   REACT_APP_HOST_URI=https://localhost:8443
   ...
   ```

1. 更新 `../src/proxy/setupProxy.auth.basic.js` 檔案以便使用搭配 `secure: false` 選項的寬鬆 SSL 設定。

   ```javascript
   ...
   module.exports = function(app) {
   app.use(
       ['/content', '/graphql'],
       createProxyMiddleware({
       target: REACT_APP_HOST_URI,
       changeOrigin: true,
       secure: false, // Ignore SSL certificate errors
       // pass in credentials when developing against an Author environment
       auth: `${REACT_APP_BASIC_AUTH_USER}:${REACT_APP_BASIC_AUTH_PASS}`
       })
   );
   };
   ```

1. 啟動 React 應用程式。

   ```bash
   $ npm start
   ```

## 驗證設定

透過上述步驟設定好本機開發環境後，我們來驗證設定是否正確。

### 本機驗證

請確認以下服務透過 HTTPS 在本機執行，您可能需要在瀏覽器中接受自我簽署憑證的安全性警告：

1. WKND Teams React 應用程式在 [https://localhost:3000](https://localhost:3000)
1. AEM SDK 在 [https://localhost:8443](https://localhost:8443)
1. 通用編輯器服務在 [https://localhost:8001](https://localhost:8001)

### 在通用編輯器中載入 WKND Teams React 應用程式

我們在通用編輯器中載入 WKND Teams React 應用程式來驗證設定：

1. 在瀏覽器中開啟通用編輯器 https://experience.adobe.com/#/aem/editor。若系統提示，請使用您的 Adobe ID 登入。

1. 在通用編輯器的「網站 URL」輸入欄位中輸入 WKND Teams React 應用程式的 URL，然後按一下 `Open`。

   ![通用編輯器 - 網站 URL](./assets/universal-editor-site-url.png)

1. WKND Teams React 應用程式在通用編輯器中載入&#x200B;**但您還不能編輯內容**。您需要檢測 React 應用程式，才能使用通用編輯器編輯內容。

   ![通用編輯器 - WKND Teams React 應用程式](./assets/universal-editor-wknd-teams.png)


## 下一步

了解如何[檢測 React 應用程式以編輯內容](./instrument-to-edit-content.md)。
