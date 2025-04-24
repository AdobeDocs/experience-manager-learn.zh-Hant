---
title: 使用Adobe I/O Runtime動作處理的AEM事件
description: 瞭解如何使用Adobe I/O Runtime動作處理收到的AEM事件。
version: Experience Manager as a Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 558
last-substantial-update: 2024-01-30T00:00:00Z
jira: KT-14879
thumbnail: KT-14879.jpeg
exl-id: c362011e-89e4-479c-9a6c-2e5caa3b6e02
source-git-commit: bb4f9982263a15f18b9f39b1577b61310dfbe643
workflow-type: tm+mt
source-wordcount: '548'
ht-degree: 0%

---

# 使用Adobe I/O Runtime動作處理的AEM事件

瞭解如何使用[Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/what_is_runtime/)動作處理收到的AEM事件。 此範例增強先前範例的[Adobe I/O Runtime動作和AEM事件](runtime-action.md)，在繼續此範例之前，請確定您已完成它。

>[!VIDEO](https://video.tv.adobe.com/v/3427054?quality=12&learn=on)

在此範例中，事件處理會將原始事件資料和收到的事件儲存為活動訊息，並存放在Adobe I/O Runtime儲存體中。 不過，如果事件是&#x200B;_修改的內容片段_&#x200B;型別，那麼它也會呼叫AEM作者服務來尋找修改詳細資料。 最後，它會在單頁應用程式(SPA)中顯示事件詳細資訊。

## 必要條件

若要完成本教學課程，您需要：

- 已啟用[AEM事件的AEM as a Cloud Service環境](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment)。 此外，必須在其上部署範例[WKND Sites](https://github.com/adobe/aem-guides-wknd?#aem-wknd-sites-project)專案。

- 存取[Adobe Developer Console](https://developer.adobe.com/developer-console/docs/guides/getting-started)。

- [Adobe Developer CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/)已安裝在您的本機電腦上。

- 從先前範例[Adobe I/O Runtime動作和AEM事件](./runtime-action.md#initialize-project-for-local-development)本機初始化的專案。

## AEM事件處理器動作

在此範例中，事件處理器[動作](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/)會執行下列工作：

- 將收到的事件剖析為活動訊息。
- 如果收到的事件是&#x200B;_修改的內容片段_&#x200B;型別，請回撥AEM作者服務以尋找修改詳細資料。
- 在Adobe I/O Runtime儲存空間中儲存原始事件資料、活動訊息和修改詳細資訊（如果有的話）。

若要執行上述任務，我們先將動作新增至專案，開發JavaScript模組以執行上述任務，最後更新動作程式碼以使用開發的模組。

如需完整程式碼，請參閱附加的[WKND-AEM-Eventing-Runtime-Action.zip](../assets/examples/event-processing-using-runtime-action/WKND-AEM-Eventing-Runtime-Action.zip)檔案，以下區段將重點說明主要檔案。

### 新增動作

- 若要新增動作，請執行以下命令：

  ```bash
  aio app add action
  ```

- 選取`@adobe/generator-add-action-generic`作為動作範本，將動作命名為`aem-event-processor`。

  ![新增動作](../assets/examples/event-processing-using-runtime-action/add-action-template.png)

### 開發JavaScript模組

為了執行上述工作，讓我們開發下列JavaScript模組。

- `src/dx-excshell-1/actions/aem-event-processor/eventValidator.js`模組判斷收到的事件是否為&#x200B;_已修改的內容片段_&#x200B;型別。

  ```javascript
  async function needsAEMCallback(aemEvent) {
  // create a Logger
  const logger = Core.Logger('eventValidator', {
      level: 'info',
  });
  
  let isValid = false;
  
  // verify the event is a Content Fragment Modified event
  if (
      aemEvent
      && aemEvent.ContentType === 'contentFragment'
      && aemEvent.EventName === 'modified'
  ) {
      logger.info('Processing Content Fragment Modified Event');
      isValid = true;
  }
  
  return isValid;
  }
  
  module.exports = needsAEMCallback;
  ```

- `src/dx-excshell-1/actions/aem-event-processor/loadEventDetailsFromAEM.js`模組會呼叫AEM作者服務以尋找修改詳細資料。

  ```javascript
  ...
  const auth = require('@adobe/jwt-auth');
  ...
  // Get AEM Service Credentials aka Technical Account details.
  // These are passed to the action as params and added in .env file.
  const clientId = params.AEM_SERVICECREDENTIALS_TECHNICALACCOUNT_CLIENTID;
  const technicalAccountId = params.AEM_SERVICECREDENTIALS_ID;
  const orgId = params.AEM_SERVICECREDENTIALS_ORG;
  const clientSecret = params.AEM_SERVICECREDENTIALS_TECHNICALACCOUNT_CLIENTSECRET;
  // Private key is passed as a string with \n and \r characters escaped.
  const privateKey = params.AEM_SERVICECREDENTIALS_PRIVATEKEY.replace(
      /\\n/g,
      '\n',
  ).replace(/\\r/g, '\r');
  const metaScopes = params.AEM_SERVICECREDENTIALS_METASCOPES.split(',');
  const ims = `https://${params.AEM_SERVICECREDENTIALS_IMSENDPOINT}`;
  
  // Get the access token from IMS using Adobe I/O SDK
  const { access_token } = await auth({
      clientId,
      technicalAccountId,
      orgId,
      clientSecret,
      privateKey,
      metaScopes,
      ims,
  });
  ...
  // Call AEM Author service to get the CF details using AEM Assets API
  const res = await fetch(
      `${aemAuthorHost}${cfPath.replace('/content/dam/', '/api/assets/')}.json`,
  {
    method: 'GET',
    headers: {
      'Content-Type': 'application/json',
      Authorization: `Bearer ${access_token}`,
    },
  },
  );
  
  let newValuesOfCFPropertiesAddedOrModified = {};
  // If the response is OK, get the values of the CF properties that were added or modified
  if (res.ok) {
      logger.info('AEM Event Details loaded from AEM Author instance');
      const responseJSON = await res.json();
  
      // Get the values of the CF properties that were added or modified
      if (
      responseJSON
      && responseJSON.properties
      && responseJSON.properties.elements
      ) {
      const allCurrentCFProperties = responseJSON.properties.elements;
  
      newValuesOfCFPropertiesAddedOrModified = cfPropertiesAddedOrModified.map(
          (key) => ({
          key,
          value: allCurrentCFProperties[key],
          }),
      );
      }    
  }
  ...
  ```

  請參閱[AEM服務認證教學課程](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en)，瞭解更多相關資訊。 此外，用於管理密碼和動作引數的[App Builder組態檔](https://developer.adobe.com/app-builder/docs/guides/configuration/)。

- `src/dx-excshell-1/actions/aem-event-processor/storeEventData.js`模組會將原始事件資料、活動訊息和修改詳細資料（如果有的話）儲存在Adobe I/O Runtime儲存空間。

  ```javascript
  ...
  const filesLib = require('@adobe/aio-lib-files');
  ...
  
  const files = await filesLib.init();
  
  const eventDataAsJSON = JSON.stringify({
      activity: activityMessage,
      aemEvent,
      aemEventDetails,
  });
  
  // store details in a folder named YYYY-MM-DD and a file named <eventID>.json
  const bytesWritten = await files.write(
      `${formattedDate}/${aemEvent.getEventID()}.json`,
      eventDataAsJSON,
  );
  ...
  ```

### 更新動作程式碼

最後，更新`src/dx-excshell-1/actions/aem-event-processor/index.js`上的動作程式碼以使用已開發的模組。

```javascript
...
// handle the challenge probe request, they are sent by I/O to verify the action is valid
if (params.challenge) {
    logger.info('Challenge probe request detected');
    responseMsg = JSON.stringify({ challenge: params.challenge });
} else {
    logger.info('AEM Event request received');

    // create AEM Event object from request parameters
    const aemEvent = new AEMEvent(params);

    // get AEM Event as activity message using the helper method
    const activityMessage = aemEvent.getEventAsActivityMessage();

    // determine if AEM Event requires callback to AEM Author service
    const callbackAEMForEventDetails = await needsAEMCallback(aemEvent);

    let eventDetails = {};
    if (callbackAEMForEventDetails) {
    // call AEM Author service to get specifics about the event
    eventDetails = await loadEventDetailsFromAEMAuthorService(
        aemEvent,
        params,
    );
    }

    // store AEM Event and Event details in the file system
    const storageDetails = await storeEventData(
    activityMessage,
    aemEvent,
    eventDetails || {},
    );
    logger.info(`Storage details: ${JSON.stringify(storageDetails)}`);

    // create response message
    responseMsg = JSON.stringify({
    message: 'AEM Event processed',
    activityMessage,
    });

    // response object
    const response = {
    statusCode: 200,
    body: responseMsg,
    };
    logger.info('Adobe I/O Runtime action response', response);

    // Return the response to the caller
    return response;
}
...
```

## 其他資源

- `src/dx-excshell-1/actions/model`資料夾包含`aemEvent.js`和`errors.js`檔案，動作會使用這些檔案分別剖析收到的事件和處理錯誤。
- `src/dx-excshell-1/actions/load-processed-aem-events`資料夾包含動作程式碼，SPA會使用此動作從Adobe I/O Runtime儲存空間載入已處理的AEM事件。
- `src/dx-excshell-1/web-src`資料夾包含SPA程式碼，會顯示已處理的AEM事件。
- `src/dx-excshell-1/ext.config.yaml`檔案包含動作組態和引數。

## 概念與主要要領

雖然專案之間的事件處理需求不同，但此範例的關鍵要點是：

- 可以使用Adobe I/O Runtime動作完成事件處理。
- 執行階段動作可與您的內部應用程式、協力廠商解決方案和Adobe解決方案等系統通訊。
- 執行階段動作是商務程式的入口點，是針對內容變更而設計。
