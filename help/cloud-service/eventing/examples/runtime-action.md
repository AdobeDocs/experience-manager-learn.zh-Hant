---
title: Adobe I/O Runtime動作和AEM事件
description: 瞭解如何使用Adobe I/O Runtime動作接收AEM事件，並檢閱事件詳細資訊，例如裝載、標題和中繼資料。
version: Experience Manager as a Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 457
last-substantial-update: 2024-01-29T00:00:00Z
jira: KT-14878
thumbnail: KT-14878.jpeg
exl-id: b1c127a8-24e7-4521-b535-60589a1391bf
source-git-commit: bb4f9982263a15f18b9f39b1577b61310dfbe643
workflow-type: tm+mt
source-wordcount: '699'
ht-degree: 0%

---

# Adobe I/O Runtime動作和AEM事件

瞭解如何使用[Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/what_is_runtime/)動作接收AEM事件，並檢閱事件詳細資料，例如裝載、標頭和中繼資料。

>[!VIDEO](https://video.tv.adobe.com/v/3427053?quality=12&learn=on)

Adobe I/O Runtime是無伺服器平台，可讓程式碼執行以回應Adobe I/O Events。 因此可協助您建立事件驅動的應用程式，而不需擔心基礎建設問題。

在此範例中，您會建立接收AEM事件並記錄事件詳細資料的Adobe I/O Runtime [動作](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/)。
https://developer.adobe.com/runtime/docs/guides/overview/what_is_runtime/

高層級步驟為：

- 在Adobe Developer Console中建立專案
- 初始化專案以進行本機開發
- 在Adobe Developer Console中設定專案
- 觸發AEM事件並驗證動作執行

## 先決條件

若要完成本教學課程，您需要：

- 已啟用[AEM事件的AEM as a Cloud Service環境](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment)。

- 存取[Adobe Developer Console](https://developer.adobe.com/developer-console/docs/guides/getting-started)。

- [Adobe Developer CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/)已安裝在您的本機電腦上。

## 在Adobe Developer Console中建立專案

若要在Adobe Developer Console中建立專案，請遵循下列步驟：

- 導覽至[Adobe Developer Console](https://developer.adobe.com/)並按一下&#x200B;**主控台**&#x200B;按鈕。

- 在&#x200B;**快速入門**&#x200B;區段中，按一下&#x200B;**從範本建立專案**。 然後在&#x200B;**瀏覽範本**&#x200B;對話方塊中，選取&#x200B;**App Builder**&#x200B;範本。

- 視需要更新專案標題、應用程式名稱並新增工作區。 然後，按一下&#x200B;**儲存**。

  ![在Adobe Developer Console中建立專案](../assets/examples/runtime-action/create-project.png)


## 初始化專案以進行本機開發

若要將Adobe I/O Runtime動作新增至專案，您必須初始化專案以進行本機開發。 在本機電腦開啟終端機上，導覽至您要初始化專案的位置，然後依照下列步驟進行：

- 執行以初始化專案

  ```bash
  aio app init
  ```

- 選取`Organization`、您在上一步建立的`Project`以及工作區。 在`What templates do you want to search for?`步驟中，選取`All Templates`選項。

  ![組織 — 專案 — 選擇 — 初始化專案](../assets/examples/runtime-action/all-templates.png)

- 從範本清單中選取`@adobe/generator-app-excshell`選項。

  ![擴充性範本 — 初始化專案](../assets/examples/runtime-action/extensibility-template.png)

- 在您最愛的IDE中開啟專案，例如VSCode。

- 選取的&#x200B;_擴充性範本_ (`@adobe/generator-app-excshell`)提供一般執行階段動作，程式碼在`src/dx-excshell-1/actions/generic/index.js`檔案中。 讓我們更新以保持簡單，記錄事件詳細資料並傳回成功回應。 不過在下個範例中，會增強來處理已收到的AEM事件。

  ```javascript
  const fetch = require("node-fetch");
  const { Core } = require("@adobe/aio-sdk");
  const {
  errorResponse,
  getBearerToken,
  stringParameters,
  checkMissingRequestInputs,
  } = require("../utils");
  
  // main function that will be executed by Adobe I/O Runtime
  async function main(params) {
  // create a Logger
  const logger = Core.Logger("main", { level: params.LOG_LEVEL || "info" });
  
  try {
      // 'info' is the default level if not set
      logger.info("Calling the main action");
  
      // log parameters, only if params.LOG_LEVEL === 'debug'
      logger.debug(stringParameters(params));
  
      const response = {
      statusCode: 200,
      body: {
          message: "Received AEM Event, it will be processed in next example",
      },
      };
  
      // log the response status code
      logger.info(`${response.statusCode}: successful request`);
      return response;
  } catch (error) {
      // log any server errors
      logger.error(error);
      // return with 500
      return errorResponse(500, "server error", logger);
  }
  }
  
  exports.main = main;
  ```

- 最後，透過執行在Adobe I/O Runtime上部署更新的動作。

  ```bash
  aio app deploy
  ```

## 在Adobe Developer Console中設定專案

若要接收AEM事件並執行上一步建立的Adobe I/O Runtime動作，請在Adobe Developer Console中設定專案。

- 在Adobe Developer Console中，導覽至上一步建立的[專案](https://developer.adobe.com/console/projects)，然後按一下以開啟專案。 選取`Stage`工作區，這是部署動作的位置。

- 按一下&#x200B;**[新增服務**]按鈕，然後選取&#x200B;**API**&#x200B;選項。 在&#x200B;**新增API**&#x200B;強制回應視窗中，選取&#x200B;**Adobe服務** > **I/O管理API**，然後按一下&#x200B;**下一步**，遵循其他設定步驟，然後按一下&#x200B;**儲存已設定的API**。

  ![新增服務 — 設定專案](../assets/examples/runtime-action/add-io-management-api.png)

- 同樣地，按一下&#x200B;**新增服務**&#x200B;按鈕並選取&#x200B;**事件**&#x200B;選項。 在&#x200B;**新增事件**&#x200B;對話方塊中，選取&#x200B;**Experience Cloud** > **AEM Sites**，然後按一下&#x200B;**下一步**。 按照其他設定步驟操作，選取AEMCS執行個體、事件型別和其他詳細資訊。

- 最後，在&#x200B;**如何接收事件**&#x200B;步驟中，展開&#x200B;**執行階段動作**&#x200B;選項，並選取在上一步建立的&#x200B;_一般_&#x200B;動作。 按一下&#x200B;**儲存已設定的事件**。

  ![執行階段動作 — 設定專案](../assets/examples/runtime-action/select-runtime-action.png)

- 檢閱事件註冊詳細資料，以及&#x200B;**偵錯追蹤**&#x200B;標籤，並驗證&#x200B;**挑戰探查**&#x200B;要求與回應。

  ![活動註冊詳細資料](../assets/examples/runtime-action/debug-tracing-challenge-probe.png)


## 觸發AEM事件

若要從已在上述AEM專案中註冊的AEM as a Cloud Service環境觸發Adobe Developer Console事件，請遵循下列步驟：

- 透過[Cloud Manager](https://my.cloudmanager.adobe.com/)存取並登入您的AEM as a Cloud Service作者環境。

- 根據您的&#x200B;**訂閱事件**，建立、更新、刪除、發佈或取消發佈內容片段。

## 檢閱事件詳細資料

完成上述步驟後，您應該會看到系統將AEM事件傳遞至一般動作。

您可以在事件登入詳細資訊的&#x200B;**偵錯追蹤**&#x200B;標籤中檢閱事件詳細資訊。

![AEM活動詳細資料](../assets/examples/runtime-action/aem-event-details.png)


## 後續步驟

在下個範例中，我們將增強此動作以處理AEM事件、回撥AEM作者服務以取得內容詳細資料、在Adobe I/O Runtime儲存空間中儲存詳細資料，並透過單頁應用程式(SPA)顯示它們。
