---
title: Bulk屬性更新示例AEMContent Fragment Console擴展
description: 批量更AEM新內容片段屬性的示例內容片段控制台擴展。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
kt: 11604
thumbnail: KT-11604.png
doc-type: article
last-substantial-update: 2022-12-09T00:00:00Z
exl-id: fbfb5c10-95f8-4875-88dd-9a941d7a16fd
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '782'
ht-degree: 0%

---

# 批量屬性更新示例擴展

![批量屬性更新](./assets/bulk-property-update/screenshot.png){align="center"}

此示例AEM內容片段控制台擴展是 [操作欄](../action-bar.md) 批量將內容片段屬性更新為公共值的擴展。

示例擴展的功能流如下：

![Adobe I/O Runtime動作流](./assets/bulk-property-update/flow.png){align="center"}

1. 選擇「內容片段」，然後按一下 [操作欄](#extension-registration) 開啟 [模態](#modal)。
1. 的 [模態](#modal) 顯示使用 [反應光譜](https://react-spectrum.adobe.com/react-spectrum/)。
1. 提交表單將選定內容片段的清單和主AEM機發送到 [自定義Adobe I/O Runtime操作](#adobe-io-runtime-action)。
1. 的 [Adobe I/O Runtime行動](#adobe-io-runtime-action) 驗證輸入並發出HTTPPUT請求AEM以更新所選內容片段。
1. 每個內容片段的一系列HTTPPUT以更新指定的屬性。
1. AEMas a Cloud Service將屬性更新保留到內容片段，並返回對Adobe I/O Runtime操作的成功或失敗響應。
1. 該模式從Adobe I/O Runtime操作接收到響應，並顯示成功批量更新的清單。

此視頻將回顧示例批量屬性更新擴展、其工作方式和開發方式。

>[!VIDEO](https://video.tv.adobe.com/v/3412296?quality=12&learn=on)

## 應用程式生成器擴展應用

此示例使用現有的Adobe Developer控制台項目，並在初始化App Builder應用時使用以下選項，通過 `aio app init`。

+ 您要搜索哪些模板？: `All Extension Points`
+ 選擇要安裝的模板：` @adobe/aem-cf-admin-ui-ext-tpl`
+ 您想為您的分機命名什麼？: `Bulk property update`
+ 請提供您的分機的簡短說明： `An example action bar extension that bulk updates a single property one or more content fragments.`
+ 您想從哪個版本開始： `0.0.1`
+ 您接下來想做什麼？
   + `Add a custom button to Action Bar`
      + 請提供按鈕的標籤名稱： `Bulk property update`
      + 是否需要為按鈕顯示模式？ `y`
   + `Add server-side handler`
      + Adobe I/O Runtime允許您按需調用無伺服器代碼。 您要如何命名此操作？: `generic`

生成的App Builder擴展應用將按如下所述更新。

## 應用路由{#app-routes}

的 `src/aem-cf-console-admin-1/web-src/src/components/App.js` 包含 [反應路由器](https://reactrouter.com/en/main)。

路由有兩組邏輯：

1. 第一條路由將請求映射到 `index.html`，調用負責 [擴展註冊](#extension-registration)。

   ```javascript
   <Route index element={<ExtensionRegistration />} />
   ```

1. 第二組路由將URL映射為表示擴展模式內容的「反應」元件。 的 `:selection` param表示分隔的清單內容片段路徑。

   如果擴展具有多個按鈕以調用離散操作，則每個按鈕 [擴展註冊](#extension-registration) 映射到此處定義的路由。

   ```javascript
   <Route
       exact path="content-fragment/:selection/bulk-property-update"
       element={<BulkPropertyUpdateModal />}
       />
   ```

## 分機註冊

`ExtensionRegistration.js`，映射到 `index.html` 路由，是擴展的入AEM口點並定義：

1. 擴展按鈕的位置顯示在創作AEM體驗中(`actionBar` 或 `headerMenu`)
1. 擴展按鈕的定義 `getButton()` 函式
1. 在 `onClick()` 函式

+ `src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId,
      methods: {
        // Configure your Action Bar button here
        actionBar: {
          getButton() {
            return {
              'id': 'bulk-property-update',     // Unique ID for the button
              'label': 'Bulk property update',  // Button label 
              'icon': 'Edit'                    // Button icon; get name from: https://spectrum.adobe.com/page/icons/ (Remove spaces, keep uppercase)
            }
          },

          // Click handler for the extension button
          onClick(selections) {
            // Collect the selected content fragment paths 
            const selectionIds = selections.map(selection => selection.id);

            // Create a URL that maps to the 
            const modalURL = "/index.html#" + generatePath(
              "/content-fragment/:selection/bulk-property-update",
              {
                // Set the :selection React route parameter to an encoded, delimited list of paths of the selected content fragments
                selection: encodeURIComponent(selectionIds.join('|'))
              }
            );

            // Open the route in the extension modal using the constructed URL
            guestConnection.host.modal.showUrl({
              title: "Bulk property update",
              url: modalURL
            })
          }
        },

      }
    })
  }
  init().catch(console.error)
```

## 模型

擴展的每條路由，如中定義 [`App.js`](#app-routes)，映射到在擴展模式中呈現的「反應」(React)元件。

在此示例應用中，有一個模式React元件(`BulkPropertyUpdateModal.js`)有三個狀態：

1. 正在載入，指示用戶必須等待
1. 批量屬性更新表單，允許用戶指定要更新的屬性名稱和值
1. 批量屬性更新操作的響應，列出已更新的內容片段和無法更新的內容片段

重要的是，與擴展AEM的任何互動都應委託 [AppBuilderAdobe I/O Runtime操作](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/)，是在中運行的獨立無伺服器進程 [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/)。
使用Adobe I/O Runtime的操作與AEM其進行通信是為了避免跨源資源共用(CORS)連接問題。

提交「批量屬性更新」表單時，將自定義 `onSubmitHandler()` 調用Adobe I/O Runtime操作，傳AEM遞當前主機（域）和用AEM戶的訪問令牌，然後調用 [內AEM容片段API](https://experienceleague.adobe.com/docs/experience-manager-65/assets/extending/assets-api-content-fragments.html) 更新內容片段。

當接收到來自Adobe I/O Runtime操作的響應時，更新模式以顯示批量屬性更新操作的結果。

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


## Adobe I/O Runtime行動

擴展應AEM用生成器應用可以定義或使用0或許多Adobe I/O Runtime操作。
Adobe運行時操作應是需要與或其他AdobeWeb服AEM務交互的負責工作。

在此示例應用中，Adobe I/O Runtime操作 — 使用預設名稱 `generic`  — 負責：

1. 向內容片段API發出一AEM系列HTTP請求以更新內容片段。
1. 收集這些HTTP請求的響應，將它們歸類為成功和失敗
1. 返回成功和失敗清單以供模式顯示(`BulkPropertyUpdateModal.js`)

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
