---
title: 大量屬性更新範例AEM內容片段主控台擴充功能
description: 大量更新內容片段屬性的AEM內容片段主控台擴充功能的範例。
feature: Developer Tools, Content Fragments
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11604
thumbnail: KT-11604.png
doc-type: article
last-substantial-update: 2024-01-26T00:00:00Z
exl-id: fbfb5c10-95f8-4875-88dd-9a941d7a16fd
duration: 1362
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '769'
ht-degree: 0%

---

# 大量屬性更新的擴充功能範例

>[!VIDEO](https://video.tv.adobe.com/v/3412296?quality=12&learn=on)

此範例AEM內容片段主控台擴充功能是一個[動作列](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/action-bar/)擴充功能，可將內容片段屬性大量更新為通用值。

範例擴充功能的功能流程如下：

![Adobe I/O Runtime動作流程](./assets/bulk-property-update/flow.png){align="center"}

1. 選取內容片段，然後按一下[動作列](#extension-registration)中的擴充功能按鈕，開啟[強制回應](#modal)。
2. [模型](#modal)顯示以[React Spectrum](https://react-spectrum.adobe.com/react-spectrum/)建置的自訂輸入表單。
3. 提交表單會將選取的內容片段清單和AEM主機傳送到[自訂Adobe I/O Runtime動作](#adobe-io-runtime-action)。
4. [Adobe I/O Runtime動作](#adobe-io-runtime-action)會驗證輸入並向AEM發出HTTP PUT請求以更新所選的內容片段。
5. 每個內容片段的一系列HTTP PUT可更新指定的屬性。
6. AEM as a Cloud Service會儲存內容片段的屬性更新，並傳回Adobe I/O Runtime動作的成功或失敗回應。
7. 強制回應視窗收到來自Adobe I/O Runtime動作的回應，並顯示成功的大量更新清單。

## 擴充點

此範例延伸至擴充點`actionBar`，以將自訂按鈕新增至內容片段主控台。

| AEM UI已擴充 | 擴充點 |
| ------------------------ | --------------------- |
| [內容片段主控台](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/) | [動作列](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/action-bar/) |


## 範例擴充功能

此範例使用現有的Adobe Developer Console專案，並在透過`aio app init`初始化App Builder應用程式時使用下列選項。

+ 您要搜尋哪些範本？： `All Extension Points`
+ 選擇要安裝的範本： ` @adobe/aem-cf-admin-ui-ext-tpl`
+ 您要如何命名擴充功能？： `Bulk property update`
+ 請提供您擴充功能的簡短說明： `An example action bar extension that bulk updates a single property one or more content fragments.`
+ 您想從哪個版本開始？： `0.0.1`
+ 您接下來要做什麼？
   + `Add a custom button to Action Bar`
      + 請提供按鈕的標簽名稱： `Bulk property update`
      + 您是否需要顯示按鈕的強制回應視窗？`y`
   + `Add server-side handler`
      + Adobe I/O Runtime可讓您隨選叫用無伺服器程式碼。 您要如何命名此動作？： `generic`

產生的App Builder擴充功能應用程式已更新，如下所述。

### 應用程式路由{#app-routes}

`src/aem-cf-console-admin-1/web-src/src/components/App.js`包含[React路由器](https://reactrouter.com/en/main)。

路由的邏輯集合有兩種：

1. 第一個路由將請求對應到`index.html`，這會叫用負責[延伸註冊](#extension-registration)的React元件。

   ```javascript
   <Route index element={<ExtensionRegistration />} />
   ```

1. 第二組路由會將URL對應至轉譯擴充功能模組內容的React元件。 `:selection`引數代表分隔的清單內容片段路徑。

   如果擴充功能有多個按鈕可叫用分散式動作，則每個[擴充功能註冊](#extension-registration)都會對應至此處定義的路由。

   ```javascript
   <Route
       exact path="content-fragment/:selection/bulk-property-update"
       element={<BulkPropertyUpdateModal />}
       />
   ```

### 擴充功能註冊

對應至`ExtensionRegistration.js`路由的`index.html`是AEM擴充功能的進入點，並定義：

1. 擴充功能按鈕的位置會顯示在AEM編寫體驗（`actionBar`或`headerMenu`）中
1. `getButtons()`函式中的擴充按鈕定義
1. `onClick()`函式中按鈕的點選處理常式

+ `src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
import React from "react";
import { generatePath } from "react-router";
import { Text } from "@adobe/react-spectrum";
import { register } from "@adobe/uix-guest";
import { extensionId } from "./Constants";

function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId, // Some unique ID for the extension used to facilitate communication between the extension and Content Fragment Console
      methods: {
        // Configure your Action Bar button here
        actionBar: {
          getButtons() {
            return [
              {
                id: "examples.action-bar.bulk-property-update", // Unique ID for the button
                label: "Bulk property update", // Button label
                icon: "OpenIn", // Button icon; get name from: https://spectrum.adobe.com/page/icons/ (Remove spaces, keep uppercase)
                // Click handler for the extension button
                onClick(selections) {
                  // Collect the selected content fragment paths
                  const selectionIds = selections.map(
                    (selection) => selection.id
                  );

                  // Create a URL that maps to the
                  const modalURL =
                    "/index.html#" +
                    generatePath(
                      "/content-fragment/:selection/bulk-property-update",
                      {
                        // Set the :selection React route parameter to an encoded, delimited list of paths of the selected content fragments
                        selection: encodeURIComponent(selectionIds.join("|")),
                      }
                    );

                  // Open the route in the extension modal using the constructed URL
                  guestConnection.host.modal.showUrl({
                    // The modal title
                    title: "Bulk property update",
                    url: modalURL,
                  });
                },
              },
            ];
          },
        },
      },
    });
  };

  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}

export default ExtensionRegistration;
```

### 模態視窗

每個擴充功能的路由（如[`App.js`](#app-routes)中的定義）都會對應至在擴充功能強制回應中呈現的React元件。

在此範例應用程式中，有一個模組React元件(`BulkPropertyUpdateModal.js`)具有三種狀態：

1. 正在載入，表示使用者必須等待
1. 大量屬性更新表單，可讓使用者指定要更新的屬性名稱和值
1. 大量屬性更新操作的回應，其中列出已更新的內容片段以及無法更新的內容片段

重要的是，從擴充功能與AEM的任何互動都應委派給[AppBuilder Adobe I/O Runtime動作](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/)，這是在[Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/)中執行的個別無伺服器程式。
使用Adobe I/O Runtime動作與AEM通訊是為了避免跨原始資源共用(CORS)連線問題。

提交大量屬性更新表單時，自訂`onSubmitHandler()`會叫用Adobe I/O Runtime動作，傳遞目前的AEM主機（網域）和使用者的AEM存取權杖，接著呼叫[AEM內容片段API](https://experienceleague.adobe.com/docs/experience-manager-65/assets/extending/assets-api-content-fragments.html)以更新內容片段。

收到Adobe I/O Runtime動作的回應時，強制回應會更新，以顯示大量屬性更新作業的結果。

+ `src/aem-cf-console-admin-1/web-src/src/components/BulkPropertyUpdateModal.js`

```javascript
import React, { useState, useEffect } from 'react'
import { attach } from "@adobe/uix-guest"
import {
  Flex,
  Form,
  Provider,
  Content,
  defaultTheme,
  ContextualHelp,
  Text,
  TextField,
  ButtonGroup,
  Button,
  Heading,
  ListView,
  Item,
} from '@adobe/react-spectrum'

import Spinner from "./Spinner"

import { useParams } from "react-router-dom"

import allActions from '../config.json'
import actionWebInvoke from '../utils'

import { extensionId } from "./Constants"

export default function BulkPropertyUpdateModal() {
  // Set up state used by the React component
  const [guestConnection, setGuestConnection] = useState()
  
  const [actionInvokeInProgress, setActionInvokeInProgress] = useState(false);
  const [actionResponse, setActionResponse] = useState();

  const [propertyName, setPropertyName] = useState(null);
  const [propertyValue, setPropertyValue] = useState(null);
  const [validationState, setValidationState] = useState({});

  // Get the selected content fragment paths from the route parameter `:selection`
  let { selection } = useParams();
  let fragmentIds = selection?.split('|') || [];
  
  console.log('Content Fragment Ids', fragmentIds);

  if (!fragmentIds || fragmentIds.length === 0) {
    console.error("Unable to locate a list of Content Fragments to update.")
    return;
  }

  // Asynchronously attach the extension to AEM, we must wait or the guestConnection to be set before doing anything in the modal
  useEffect(() => {
    (async () => {
       // extensionId is the unique id of this extension (you can make this up as long as its unique) .. in this case its `bulk-property-update` pulled out into Constants.js as it is also referenced in ExtensionRegistration.js
      const guestConnection = await attach({ id: extensionId })
      setGuestConnection(guestConnection);
    })()
  }, [])


  // Determine view to display in the modal
  if (!guestConnection) {
    // If the guestConnection is not initialized, display a loading spinner
    return <Spinner />
  } else if (actionInvokeInProgress) {
    // If the bulk property action has been invoked but not completed, display a loading spinner
    return <Spinner />
  } else if (actionResponse) {
    // If the bulk property action has completed, display the response
    return renderResponse();
  } else {
    // Else display the bulk property update form
    return renderForm();
  }

  /**
   * Renders the Bulk Property Update form. 
   * This form has two fields:
   * - Property Name: The name of the Content Fragment property name to update
   * - Property Value: the value the Content Fragment property, specified by the Property Name, will be updated to
   * 
   * @returns the Bulk Property Update form
   */
  function renderForm() {
    return (
      // Use React Spectrum components to render the form
      <Provider theme={defaultTheme} colorScheme='light'>
        <Content width="100%">
          <Flex width="100%">
            <Form 
              width="100%">
              <TextField label="Property name"
                          isRequired={true}
                          validationState={validationState?.propertyName}
                onChange={setPropertyName}
                contextualHelp={
                  <ContextualHelp>
                    <Heading>Need help?</Heading>
                    <Content>
                      <Text>The <strong>Property name</strong> must be a valid for the Content Fragment model(s) the selected Content Fragments implement.</Text>
                    </Content>
                  </ContextualHelp>
                } />

              <TextField
                label="Property value"
                validationState={validationState?.propertyValue}
                onChange={setPropertyValue} />

              <ButtonGroup align="start" marginTop="size-200">
                <Button variant="cta" onPress={onSubmitHandler}>Update {fragmentIds.length} Content Fragments</Button>
              </ButtonGroup>
            </Form>
          </Flex>

          {/* Render the close button so the user can close the modal */}
          {renderCloseButton()}
        </Content>
      </Provider>
    )
  }
  /**
   * Display the response from the Adobe I/O Runtime action in the modal.
   * This includes:
   * - A list of content fragments that were updated successfully
   * - A list a content fragments that failed to update
   * 
   * @returns the response view
   */
  function renderResponse() {
    // Separate the successful and failed content fragments updates
    const successes = actionResponse.filter(item => item.status === 200);
    const failures = actionResponse.filter(item => item.status !== 200);

    return (
      <Provider theme={defaultTheme} colorScheme='light'>
        <Content width="100%">

          <Text>Bulk updated property <strong>{propertyName}</strong> with value <strong>{propertyValue}</strong></Text>

          {/* Render the list of content fragments that were updated successfully */}
          {successes.length > 0 &&
            <><Heading level="4">{successes.length} Content Fragments successfully updated</Heading>
              <ListView
                items={successes}
                selectionMode="none"
                aria-label="Successful updates"
              >
                {(item) => (
                  <Item key={item.fragmentId} textValue={item.fragmentId.split('/').pop()}>
                    {item.fragmentId.split('/').pop()}
                  </Item>
                )}
              </ListView></>}

          {/* Render the list of content fragments that failed to update */}
          {failures.length > 0 &&
            <><Heading level="4">{failures.length} Content Fragments failed to update</Heading><ListView
              items={failures}
              selectionMode="none"
              aria-label="Failed updates"
            >
              {(item) => (
                <Item key={item.fragmentId} textValue={item.fragmentId.split('/').pop()}>
                  {item.fragmentId.split('/').pop()}
                </Item>
              )}
            </ListView></>}

          {/* Render the close button so the user can close the modal */}
          {renderCloseButton()}
        </Content>
      </Provider>);
  }

  /**
   * Provide a close button for the modal, else it cannot be closed (without refreshing the browser window)
   * 
   * @returns a button that closes the modal.
   */
   function renderCloseButton() {
    return (
      <Flex width="100%" justifyContent="end" alignItems="center" marginTop="size-400">
        <ButtonGroup align="end">
          <Button variant="primary" onPress={() => guestConnection.host.modal.close()}>Close</Button>
        </ButtonGroup>
      </Flex>
    );
  }

  /**
   * Handle the Bulk Property Update form submission.
   * This function calls the supporting Adobe I/O Runtime action to update the selected Content Fragments, and then returns the response for display in the modal
   * When invoking the Adobe I/O Runtime action, the following parameters are passed as they're used by the action to connect to AEM:
   * - AEM Host to connect to
   * - AEM access token to connect to AEM with
   * - The list of Content Fragment paths to update
   * - The Content Fragment property name to update
   * - The value to update the Content Fragment property to
   * 
   * @returns a list of content fragment update successes and failures
   */
  async function onSubmitHandler() {
    // Validate the form input fields
    if (propertyName?.length > 1) {
      setValidationState({propertyName: 'valid', propertyValue: 'valid'});
    } else {
      setValidationState({propertyName: 'invalid', propertyValue: 'valid'});
      return;
    }

    // Mark the extension as invoking the action, so the loading spinner is displayed
    setActionInvokeInProgress(true);

    // Set the HTTP headers to access the Adobe I/O runtime action
    const headers = {
      'Authorization': 'Bearer ' + guestConnection.sharedContext.get('auth').imsToken,
      'x-gw-ims-org-id': guestConnection.sharedContext.get('auth').imsOrg
    };

    console.log('headers', headers);

    // Set the parameters to pass to the Adobe I/O Runtime action
    const params = {
      aemHost: `https://${guestConnection.sharedContext.get('aemHost')}`,

      fragmentIds: fragmentIds,
      propertyName: propertyName,
      propertyValue: propertyValue
    };

    // Invoke the Adobe I/O Runtime action named `generic`. This name defined in the `ext.config.yaml` file.
    const action = 'generic';

    try {
      // Invoke Adobe I/O Runtime action with the configured headers and parameters
      const actionResponse = await actionWebInvoke(allActions[action], headers, params);

      // Set the response from the Adobe I/O Runtime action
      setActionResponse(actionResponse);

      console.log(`Response from ${action}:`, actionResponse)
    } catch (e) {
      // Log and store any errors
      console.error(e)
    }

    // Set the action as no longer being invoked, so the loading spinner is hidden
    setActionInvokeInProgress(false);
  }
}
```


### Adobe I/O Runtime動作

AEM擴充功能App Builder應用程式可定義或使用0個或多個Adobe I/O Runtime動作。
Adobe Runtime動作應負責需要與AEM或其他Adobe網路服務互動的工作。

在此範例應用程式中，使用預設名稱`generic`的Adobe I/O Runtime動作負責：

1. 向AEM內容片段API提出一系列HTTP請求以更新內容片段。
1. 收集這些HTTP要求的回應，將其歸類為成功和失敗
1. 傳回成功和失敗清單以供強制回應(`BulkPropertyUpdateModal.js`)顯示

+ `src/aem-cf-console-admin-1/actions/generic/index.js`


```javascript
const fetch = require('node-fetch')
const { Core } = require('@adobe/aio-sdk')
const { errorResponse, getBearerToken, stringParameters, checkMissingRequestInputs } = require('../utils')

// main function that will be executed by Adobe I/O Runtime
async function main (params) {
  // create a Logger
  const logger = Core.Logger('main', { level: params.LOG_LEVEL || 'info' })

  try {
    // 'info' is the default level if not set
    logger.info('Calling the main action')

    // log parameters, only if params.LOG_LEVEL === 'debug'
    logger.debug(stringParameters(params))

    // check for missing request input parameters and headers
    const requiredParams = [ 'aemHost', 'fragmentIds', 'propertyName', 'propertyValue' ]
    const requiredHeaders = ['Authorization']
    const errorMessage = checkMissingRequestInputs(params, requiredParams, requiredHeaders)
    if (errorMessage) {
      // return and log client errors
      return errorResponse(400, errorMessage, logger)
    }
      
    const body = {
      "properties": {
        "elements": {
          [params.propertyName]: {
            "value": params.propertyValue
          }
        }
      }
    };

    // Extract the user Bearer token from the Authorization header used to authenticate the request to AEM
    const accessToken = getBearerToken(params);

    let results = await Promise.all(params.fragmentIds.map(async (fragmentId) => {

      logger.info(`Updating fragment ${fragmentId} with property ${params.propertyName} and value ${params.propertyValue}`);

      const res = await fetch(`${params.aemHost}${fragmentId.replace('/content/dam/', '/api/assets/')}.json`, { 
        method: 'put',
        body: JSON.stringify(body),
        headers: {
          'Authorization': `Bearer ${accessToken}`,
          'Content-Type': 'application/json'
        }
      });

      if (res.ok) {
        logger.info(`Successfully updated ${fragmentId}`);
        return { fragmentId, status: res.status, statusText: res.statusText, body: await res.json() };
      } else {
        logger.info(`Failed to update ${fragmentId}`);
        return { fragmentId, status: res.status, statusText: res.statusText, body: await res.text() };
      }
    }));

    const response = {
      statusCode: 200,
      body: results
    };

    logger.info('Adobe I/O Runtime action response', response);

    // Return the response to the A
     return response;

  } catch (error) {
    // log any server errors
    logger.error(error)
    // return with 500
    return errorResponse(500, 'server error', logger)
  }
}

exports.main = main
```
