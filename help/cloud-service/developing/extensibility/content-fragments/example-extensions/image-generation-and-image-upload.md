---
title: 通過自定義內容片段控制台擴展生成OpenAI映像
description: 瞭解如何使用OpenAI或DALL-E 2從自然語言描述生成數字影像，以及如何使用自定義內容片段控制台擴展AEM將生成的影像上載到。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
kt: 11649
thumbnail: KT-11649.png
doc-type: article
last-substantial-update: 2023-01-04T00:00:00Z
exl-id: f3047f1d-1c46-4aee-9262-7aab35e9c4cb
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '1399'
ht-degree: 1%

---

# 使AEM用OpenAI生成影像資產

瞭解如何使用OpenAI或DALL.E 2生成影像，並將其上傳到AEMDAM以獲得內容速度。

![數字影像生成](./assets/digital-image-generation/screenshot.png){width="500" zoomable="yes"}

此示例AEM內容片段控制台擴展是 [操作欄](../action-bar.md) 使用自然語言輸入生成數字影像的擴展 [OpenAI API](https://openai.com/api/) 或 [DALL.E 2](https://openai.com/dall-e-2/)。 生成的影像被上載到AEMDAM，並且所選內容片段的影像屬性被更新以引用此從DAM新生成的上載影像。

在本示例中，您將學習：

1. 使用 [OpenAI API](https://beta.openai.com/docs/guides/images/image-generation-beta) 或 [DALL.E 2](https://openai.com/dall-e-2/)
1. 正在將影像上載到AEM
1. 內容片段屬性更新

示例擴展的功能流如下：

![Adobe I/O Runtime動作流在數字影像生成中的應用](./assets/digital-image-generation/flow.png){align="center"}

1. 選擇「內容片段」並按一下副檔名 `Generate Image` 按鈕 [操作欄](#extension-registration) 開啟 [模態](#modal)。
1. 的 [模態](#modal) 顯示使用 [反應光譜](https://react-spectrum.adobe.com/react-spectrum/)。
1. 提交表單會發送用戶提供的 `Image Description` 文本、所選內容片段AEM和主機 [自定義Adobe I/O Runtime操作](#adobe-io-runtime-action)。
1. 的 [Adobe I/O Runtime行動](#adobe-io-runtime-action) 驗證輸入。
1. 接下來，它稱為OpenAI [影像生成](https://beta.openai.com/docs/guides/images/image-generation-beta) API及其使用 `Image Description` 文本，指定應生成的影像。
1. 的 [影像生成](https://beta.openai.com/docs/guides/images/image-generation-beta) 端點建立大小的原始影像 _1024x1024_ 使用prompt請求參數值的像素並返回生成的影像URL作為響應。
1. 的 [Adobe I/O Runtime行動](#adobe-io-runtime-action) 將生成的映像下載到App Builder運行時。
1. 然後，它將啟動從App Builder運行時到預定義路AEM徑下的DAM的映像上載。
1. 該AEMas a Cloud Service將映像保存到DAM並返回對Adobe I/O Runtime操作的成功或失敗響應。 成功上載響應使用從Adobe I/O Runtime操作到的另一HTTP請求更新所選內容片段的AEM映像屬性值。
1. 該模式從Adobe I/O Runtime操作接收響應，並提供AEM新生成的上載影像的資產詳細資訊連結。

此視頻將回顧使用OpenAI或DALL.E 2擴展的示例影像生成、其工作方式和開發方式。 視頻有章節標籤，如 __功能演示、設定和技術代碼__ 快點看相關片段。

>[!VIDEO](https://video.tv.adobe.com/v/3413093?quality=12&learn=on)


## 應用程式生成器擴展應用

此示例在通過初始化App Builder應用程式時使用現有的Adobe Developer控制台項目和以下選項 `aio app init`。

+ 您要搜索哪些模板？: `All Extension Points`
+ 選擇要安裝的模板：` @adobe/aem-cf-admin-ui-ext-tpl`
+ 您想為您的分機命名什麼？: `Image generation`
+ 請提供您的分機的簡短說明： `An example action bar extension that generates an image using OpenAI and uploads it to AEM DAM.`
+ 您想從哪個版本開始： `0.0.1`
+ 您接下來想做什麼？
   + `Add a custom button to Action Bar`
      + 請提供按鈕的標籤名稱： `Generate Image`
      + 是否需要為按鈕顯示模式？ `y`
   + `Add server-side handler`
      + Adobe I/O Runtime允許您按需調用無伺服器代碼。 您要如何命名此操作？: `generate-image`

生成的App Builder擴展應用將按如下所述更新。

## 其他設定

1. 註冊免費 [OpenAI API](https://openai.com/api/) 帳戶並建立 [API密鑰](https://beta.openai.com/account/api-keys)
1. 將此密鑰添加到App Builder項目 `.env` 檔案

   ```
       # Specify your secrets here
       # This file must not be committed to source control
       ## Adobe I/O Runtime credentials
       ...
       AIO_runtime_apihost=https://adobeioruntime.net
       ...
       # OpenAI secret API key
       OPENAI_API_KEY=my-openai-secrete-key-to-generate-images
       ...
   ```

1. 通過 `OPENAI_API_KEY` 作為Adobe I/O Runtime操作的參數，更新 `src/aem-cf-console-admin-1/ext.config.yaml`

   ```yaml
       ...
   
       runtimeManifest:
         packages:
           aem-cf-console-admin-1:
             license: Apache-2.0
             actions:
               generate-image:
                 function: actions/generate-image/index.js
                 web: 'yes'
                 runtime: nodejs:16
                 inputs:
                   LOG_LEVEL: debug
                   OPENAI_API_KEY: $OPENAI_API_KEY
       ...
   ```

1. 在Node.js庫下安裝
   1. [OpenAI Node.js庫](https://github.com/openai/openai-node#installation)  — 輕鬆調用OpenAI API
   1. [上AEM載](https://github.com/adobe/aem-upload#install)  — 將影像上載到AEM-CS實例。


>[!TIP]
>
>在以下各節中，您將瞭解React和Adobe I/O Runtime操作JavaScript檔案的關鍵資訊。 供您參考的關鍵檔案 `web-src` 和  `actions` 提供了AppBuilder項目的資料夾，請參見 [adobe-appbuilder-cfc-ext-image-generation-code.zip](./assets/digital-image-generation/adobe-appbuilder-cfc-ext-image-generation-code.zip)。


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
       exact path="content-fragment/:selection/generate-image-modal"
       element={<GenerateImageModal />}
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
              'id': 'generate-image',     // Unique ID for the button
              'label': 'Generate Image',  // Button label 
              'icon': 'PublishCheck'      // Button icon; get name from: https://spectrum.adobe.com/page/icons/ (Remove spaces, keep uppercase)
            }
          },

          // Click handler for the extension button
          onClick(selections) {
            // Collect the selected content fragment paths 
            const selectionIds = selections.map(selection => selection.id);

            // Create a URL that maps to the 
            const modalURL = "/index.html#" + generatePath(
              "/content-fragment/:selection/generate-image-modal",
              {
                // Set the :selection React route parameter to an encoded, delimited list of paths of the selected content fragments
                selection: encodeURIComponent(selectionIds.join('|')),
              }
            );

            // Open the route in the extension modal using the constructed URL
            guestConnection.host.modal.showUrl({
              title: "Generate Image",
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

在此示例應用中，有一個模式React元件(`GenerateImageModal.js`)有四個狀態：

1. 正在載入，指示用戶必須等待
1. 建議用戶一次只選擇一個內容片段的警告消息
1. 「生成影像」(Generate Image)表單允許用戶以自然語言提供影像描述。
1. 影像生成操作的響應，提供新生AEM成、上傳的影像的資產詳細資訊連結。

重要的是，與擴展AEM的任何互動都應委託 [AppBuilderAdobe I/O Runtime操作](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/)，是在中運行的獨立無伺服器進程 [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/)。
使用Adobe I/O Runtime操作與AEM通信，並避免跨源資源共用(CORS)連接問題。

當 _生成影像_ 表單，自定義 `onSubmitHandler()` 調用Adobe I/O Runtime操作，傳遞映像描述AEM、當前主機（域）和用戶的訪AEM問令牌。 然後，該操作將調用OpenAI [影像生成](https://beta.openai.com/docs/guides/images/image-generation-beta) API，使用提交的映像描述生成映像。 下一個使用 [上AEM載](https://github.com/adobe/aem-upload) 節點模組 `DirectBinaryUpload` 將生成的影像上載AEM到並最終使用 [內AEM容片段API](https://experienceleague.adobe.com/docs/experience-manager-65/assets/extending/assets-api-content-fragments.html) 更新內容片段。

當接收到來自Adobe I/O Runtime動作的響應時，更新模式以顯示影像生成操作的結果。

+ `src/aem-cf-console-admin-1/web-src/src/components/GenerateImageModal.js`

```javascript
export default function GenerateImageModal() {
  // Set up state used by the React component
  const [guestConnection, setGuestConnection] = useState();

  // State hooks to manage the application state
  const [imageDescription, setImageDescription] = useState(null);
  const [validationState, setValidationState] = useState({});

  const [actionInvokeInProgress, setActionInvokeInProgress] = useState(false);
  const [actionResponse, setActionResponse] = useState();

  // Get the selected content fragment paths from the route parameter `:selection`
  const { selection } = useParams();
  const fragmentIds = selection?.split('|') || [];

  console.log('Selected Fragment Ids', fragmentIds);

  if (!fragmentIds || fragmentIds.length === 0) {
    console.error('The Content Fragments are not selected, can NOT generate images');
    return;
  }

  // Asynchronously attach the extension to AEM, we must wait or the guestConnection to be set before doing anything in the modal
  useEffect(() => {
    (async () => {
      const myGuestConnection = await attach({ id: extensionId });

      setGuestConnection(myGuestConnection);
    })();
  }, []);

  // Determine view to display in the modal
  if (!guestConnection) {
    // If the guestConnection is not initialized, display a loading spinner
    return <Spinner />;
  } if (actionInvokeInProgress) {
    // If the 'Generate Image' action has been invoked but not completed, display a loading spinner
    return <Spinner />;
  } if (fragmentIds.length > 1) {
    // If more than one CF selected show warning and suggest to select only one CF
    return renderMoreThanOneCFSelectionError();
  } if (fragmentIds.length === 1 && !actionResponse) {
    // Display the 'Generate Image' modal and ask for image description
    return renderImgGenerationForm();
  } if (actionResponse) {
    // If the 'Generate Image' actio has completed, display the response
    return renderActionResponse();
  }

  /**
   * Renders the message suggesting to select only on CF at a time to not lose credits accidentally
   *
   * @returns the suggestion or error message to select one CF at a time
   */
  function renderMoreThanOneCFSelectionError() {
    return (
      <Provider theme={defaultTheme} colorScheme="light">
        <Content width="100%">
          <Text>
            As this operation
            <strong> uses credits from Generative AI services</strong>
            {' '}
            such as DALL.E 2 (or Stable Dufusion), we allow only one Generate Image at a time.
            <p />
            <strong>So please select only one Content Fragment at this moment.</strong>
          </Text>

          <Flex width="100%" justifyContent="end" alignItems="center" marginTop="size-400">
            <ButtonGroup align="end">
              <Button variant="negative" onPress={() => guestConnection.host.modal.close()}>Close</Button>
            </ButtonGroup>
          </Flex>

        </Content>
      </Provider>
    );
  }

  /**
   * Renders the form asking for image description in the natural language and
   * displays message this action uses credits from Generative AI services.
   *
   *
   * @returns the image description input field and credit usage message
   */
  function renderImgGenerationForm() {
    return (

      <Provider theme={defaultTheme} colorScheme="light">
        <Content width="100%">

          <Flex width="100%">
            <Form
              width="100%"
            >
              <TextField
                label="Image Description"
                description="The image description in natural language, for e.g. Alaskan adventure in wilderness, animals, and flowers."
                isRequired
                validationState={validationState?.propertyName}
                onChange={setImageDescription}
                contextualHelp={(
                  <ContextualHelp>
                    <Heading>Need help?</Heading>
                    <Content>
                      <Text>
                        The
                        <strong>description of an image</strong>
                        {' '}
                        you are looking for in the natural language, for e.g. &quot;Family vacation on the beach with blue ocean, dolphins, boats and drink&quot;
                      </Text>
                    </Content>
                  </ContextualHelp>
                  )}
              />

              <Text>
                <p />
                Please note this will use credits from Generative AI services such as OpenAI/DALL.E 2. The AI-generated images are saved to this AEM as a Cloud Service Author service using logged user access (IMS) token.
              </Text>

              <ButtonGroup align="end">
                <Button variant="accent" onPress={onSubmitHandler}>Use Credits</Button>
                <Button variant="accent" onPress={() => guestConnection.host.modal.close()}>Close</Button>
              </ButtonGroup>
            </Form>
          </Flex>

        </Content>
      </Provider>

    );
  }

  function buildAssetDetailsURL(aemImgURL) {
    const urlParts = aemImgURL.split('.com');
    const aemAssetdetailsURL = `${urlParts[0]}.com/ui#/aem/assetdetails.html${urlParts[1]}`;

    return aemAssetdetailsURL;
  }

  /**
   * Displays the action response received from the App Builder
   *
   * @returns Displays App Builder action and details
   */
  function renderActionResponse() {
    return (
      <Provider theme={defaultTheme} colorScheme="light">
        <Content width="100%">

          {actionResponse.status === 'success'
            && (
              <>
                <Heading level="4">
                  Successfully generated an image, uploaded it to this AEM-CS Author service, and associated it to the selected Content Fragment.
                </Heading>

                <Text>
                  {' '}
                  Please see generated image in AEM-CS
                  {' '}
                  <Link>
                    <a href={buildAssetDetailsURL(actionResponse.aemImgURL)} target="_blank" rel="noreferrer">
                      here.
                    </a>
                  </Link>
                </Text>
              </>
            )}

          {actionResponse.status === 'failure'
            && (
            <Heading level="4">
              Failed to generate, upload image, please check App Builder logs.
            </Heading>
            )}

          <Flex width="100%" justifyContent="end" alignItems="center" marginTop="size-400">
            <ButtonGroup align="end">
              <Button variant="negative" onPress={() => guestConnection.host.modal.close()}>Close</Button>
            </ButtonGroup>
          </Flex>

        </Content>
      </Provider>
    );
  }

  /**
   * Handle the Generate Image form submission.
   * This function calls the supporting Adobe I/O Runtime actions such as
   * - Call the Generative AI service (DALL.E) with 'image description' to generate an image
   * - Download the AI generated image to App Builder runtime
   * - Save the downloaded image to AEM DAM and update Content Fragement's image reference property to use this new image
   *
   * When invoking the Adobe I/O Runtime actions, the following parameters are passed as they're used by the action to connect to AEM:
   * - AEM Host to connect to
   * - AEM access token to connect to AEM with
   * - The Content Fragment path to update
   *
   * @returns In case of success the updated content fragment, otherwise failure message
   */
  async function onSubmitHandler() {
    console.log('Started Image Generation orchestration');

    // Validate the form input fields
    if (imageDescription?.length > 1) {
      setValidationState({ imageDescription: 'valid' });
    } else {
      setValidationState({ imageDescription: 'invalid' });
      return;
    }

    // Mark the extension as invoking the action, so the loading spinner is displayed
    setActionInvokeInProgress(true);

    // Set the HTTP headers to access the Adobe I/O runtime action
    const headers = {
      Authorization: `Bearer ${guestConnection.sharedContext.get('auth').imsToken}`,
      'x-gw-ims-org-id': guestConnection.sharedContext.get('auth').imsOrg,
    };

    // Set the parameters to pass to the Adobe I/O Runtime action
    const params = {

      aemHost: `https://${guestConnection.sharedContext.get('aemHost')}`,

      fragmentId: fragmentIds[0],
      imageDescription,
    };

    const generateImageAction = 'generate-image';

    try {
      const generateImageActionResponse = await actionWebInvoke(allActions[generateImageAction], headers, params);

      // Set the response from the Adobe I/O Runtime action
      setActionResponse(generateImageActionResponse);

      console.log(`Response from ${generateImageAction}:`, actionResponse);
    } catch (e) {
      // Log and store any errors
      console.error(e);
    }

    // Set the action as no longer being invoked, so the loading spinner is hidden
    setActionInvokeInProgress(false);
  }
}
```

>[!NOTE]
>
>在 `buildAssetDetailsURL()` 函式。 `aemAssetdetailsURL` 變數值假定 [統一外殼](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/aem-cloud-service-on-unified-shell.html#overview) 的子菜單。 如果禁用了統一外殼，則需要刪除 `/ui#/aem` 的子菜單。


## Adobe I/O Runtime行動

擴展應AEM用生成器應用可以定義或使用0或許多Adobe I/O Runtime操作。
Adobe運行時操作負責需要與或Adobe或第AEM三方Web服務交互的工作。

在此示例應用中， `generate-image` Adobe I/O Runtime行動負責：

1. 使用 [OpenAI API映像生成](https://beta.openai.com/docs/guides/images/image-generation-beta) 服務
1. 將生成的影像上載AEM到 — CS實例 [上AEM載](https://github.com/adobe/aem-upload) 庫
1. 向內容片段APIAEM發出HTTP請求以更新內容片段的影像屬性。
1. 返回成功和失敗的關鍵資訊以通過模式顯示(`GenerateImageModal.js`)


### Orchestrator - `index.js`

的 `index.js` 通過使用相應的JavaScript模組（即JavaScript模組）協調上述1到3項任務 `generate-image-using-openai, upload-generated-image-to-aem, update-content-fragement`。 下面介紹了這些模組和相關代碼 [子節](#image-generation-module---generate-image-using-openaijs)。

+ `src/aem-cf-console-admin-1/actions/generate-image/index.js`

```javascript
/**
 *
 * This action orchestrates an image generation by calling the OpenAI API (DALL.E 2) and saves generated image to AEM.
 *
 * It leverages following modules
 *  - 'generate-image-using-openai' - To generate an image using OpenAI API
 *  - 'upload-generated-image-to-aem' - To upload the generated image into AEM-CS instance
 *  - 'update-content-fragement' - To update the CF image property with generated image's DAM path
 *
 */

const { Core } = require('@adobe/aio-sdk');
const {
  errorResponse, stringParameters, getBearerToken, checkMissingRequestInputs,
} = require('../utils');

const { generateImageUsingOpenAI } = require('./generate-image-using-openai');

const { uploadGeneratedImageToAEM } = require('./upload-generated-image-to-aem');

const { updateContentFragmentToUseGeneratedImg } = require('./update-content-fragement');

// main function that will be executed by Adobe I/O Runtime
async function main(params) {
  // create a Logger
  const logger = Core.Logger('main', { level: params.LOG_LEVEL || 'info' });

  try {
    // 'info' is the default level if not set
    logger.info('Calling the main action');

    // log parameters, only if params.LOG_LEVEL === 'debug'
    logger.debug(stringParameters(params));

    // check for missing request input parameters and headers
    const requiredParams = ['aemHost', 'fragmentId', 'imageDescription'];
    const requiredHeaders = ['Authorization'];
    const errorMessage = checkMissingRequestInputs(params, requiredParams, requiredHeaders);

    if (errorMessage) {
      // return and log client errors
      return errorResponse(400, errorMessage, logger);
    }

    // extract the user Bearer token from the Authorization header
    const token = getBearerToken(params);

    // Call OpenAI (DALL.E 2) API to generate an image using image description
    const generatedImageURL = await generateImageUsingOpenAI(params);
    logger.info(`Generated image using OpenAI API and url is : ${generatedImageURL}`);

    // Upload the generated image to AEM-CS
    const uploadedImagePath = await uploadGeneratedImageToAEM(params, generatedImageURL, token);
    logger.info(`Uploaded image to AEM, path is: ${uploadedImagePath}`);

    // Update Content Fragment with the newly generated image reference
    const updateContentFragmentPath = await updateContentFragmentToUseGeneratedImg(params, uploadedImagePath, token);
    logger.info(`Updated Content Fragment path is: ${updateContentFragmentPath}`);

    let result;
    if (updateContentFragmentPath) {
      result = {
        status: 'success', message: 'Successfully generated and uploaded image to AEM', genTechServiceImageURL: generatedImageURL, aemImgURL: uploadedImagePath, fragmentPath: updateContentFragmentPath,
      };
    } else {
      result = { status: 'failure', message: 'Failed to generated and uploaded image, please check App Builder logs' };
    }

    const response = {
      statusCode: 200,
      body: result,
    };

    logger.info('Adobe I/O Runtime action response', response);

    // Return the response to the caller
    return response;
  } catch (error) {
    // log any server errors
    logger.error(error);
    // return with 500
    return errorResponse(500, 'server error', logger);
  }
}

exports.main = main;
```


### 映像生成模組 —  `generate-image-using-openai.js`

此模組負責調用OpenAI [影像生成](https://beta.openai.com/docs/guides/images/image-generation-beta) 端點使用 [開放](https://github.com/openai/openai-node) 的下界。 獲取在中定義的OpenAI API密鑰 `.env` 檔案，它使用 `params.OPENAI_API_KEY`。

+ `src/aem-cf-console-admin-1/actions/generate-image/generate-image-using-openai.js`

```javascript
/**
 * This module calls OpenAI API to generate an image based on image description provided to Action
 *
 */

const { Configuration, OpenAIApi } = require('openai');

const { Core } = require('@adobe/aio-sdk');

// Placeholder than actual OpenAI Image
const PLACEHOLDER_IMG_URL = 'https://www.gstatic.com/webp/gallery/2.png';

async function generateImageUsingOpenAI(params) {
  // create a Logger
  const logger = Core.Logger('generateImageUsingOpenAI', { level: params.LOG_LEVEL || 'info' });

  let generatedImageURL = PLACEHOLDER_IMG_URL;

  // create configuration object with the API Key
  const configuration = new Configuration({
    apiKey: params.OPENAI_API_KEY,
  });

  // create OpenAIApi object
  const openai = new OpenAIApi(configuration);

  logger.info(`Generating image for input: ${params.imageDescription}`);

  try {
    // invoke createImage method with details
    const response = await openai.createImage({
      prompt: params.imageDescription,
      n: 1,
      size: '1024x1024',
    });

    generatedImageURL = response.data.data[0].url;

    logger.info(`The OpenAI generate image url is: ${generatedImageURL}`);
  } catch (error) {
    logger.error(`Error while generating image, details are: ${error}`);
  }

  return generatedImageURL;
}

module.exports = {
  generateImageUsingOpenAI,
};
```

### 將映像上載AEM到模組 —  `upload-generated-image-to-aem.js`

此模組負責將OpenAI生成的映像上載AEM到 [上AEM載](https://github.com/adobe/aem-upload) 的下界。 生成的映像首先使用Node.js下載到App Builder運行時 [檔案系統](https://nodejs.org/api/fs.html) 庫，上載完AEM成後即被刪除。

在以下代碼中 `uploadGeneratedImageToAEM` 函式將生成的映像下載協調到運行時，將其上載AEM到運行時並從運行時刪除。 映像已上載到 `/content/dam/wknd-shared/en/generated` 路徑，確保DAM中存在所有資料夾，這是使用DAM的先決條件 [上AEM載](https://github.com/adobe/aem-upload) 的下界。

+ `src/aem-cf-console-admin-1/actions/generate-image/upload-generated-image-to-aem.js`

```javascript
/**
 * This module uploads the generated image to AEM-CS instance using current user's IMS token
 *
 */

const { Core } = require('@adobe/aio-sdk');
const fs = require('fs');

const {
  DirectBinaryUploadErrorCodes,
  DirectBinaryUpload,
  DirectBinaryUploadOptions,
} = require('@adobe/aem-upload');

const codes = DirectBinaryUploadErrorCodes;
const IMG_EXTENSION = '.png';

const GENERATED_IMAGES_DAM_PATH = '/content/dam/wknd-shared/en/generated';

async function downloadImageToRuntime(logger, generatedImageURL) {
  logger.log('Downloading generated image to the runtime');

  // placeholder image name
  let generatedImageName = 'generated.png';

  try {
    // Get the generated image name from the image URL
    const justImgURL = generatedImageURL.substring(0, generatedImageURL.indexOf(IMG_EXTENSION) + 4);
    generatedImageName = justImgURL.substring(justImgURL.lastIndexOf('/') + 1);

    // Read image from URL as the buffer
    const response = await fetch(generatedImageURL);
    const buffer = await response.buffer();

    // Write/download image to the runtime
    fs.writeFileSync(generatedImageName, buffer, (err) => {
      if (err) throw err;
      logger.log('Saved the generated image!');
    });
  } catch (error) {
    logger.error(`Error while downloading image on the runtime, details are: ${error}`);
  }

  return generatedImageName;
}

function setupEventHandlers(binaryUpload, logger) {
  binaryUpload.on('filestart', (data) => {
    const { fileName } = data;

    logger.log(`Started file upload ${fileName}`);
  });

  binaryUpload.on('fileprogress', (data) => {
    const { fileName, transferred } = data;

    logger.log(`Fileupload is in progress ${fileName} & ${transferred}`);
  });

  binaryUpload.on('fileend', (data) => {
    const { fileName } = data;

    logger.log(`Finished file upload ${fileName}`);
  });

  binaryUpload.on('fileerror', (data) => {
    const { fileName, errors } = data;

    logger.log(`Error in file upload ${fileName} and ${errors}`);
  });
}

async function getImageSize(downloadedImgName) {
  const stats = fs.statSync(downloadedImgName);
  return stats.size;
}

async function uploadImageToAEMFromRuntime(logger, aemURL, downloadedImgName, accessToken) {
  let aemImageURL;
  try {
    logger.log('Uploading generated image to AEM from the runtime');

    const binaryUpload = new DirectBinaryUpload();

    // setup event handlers to track the progress, success or error
    setupEventHandlers(binaryUpload, logger);

    // get downloaded image size
    const imageSize = await getImageSize(downloadedImgName);
    logger.info(`The image upload size is: ${imageSize}`);

    // The deatils of the file to be uploaded
    const uploadFiles = [
      {
        fileName: downloadedImgName, // name of the file as it will appear in AEM
        fileSize: imageSize, // total size, in bytes, of the file
        filePath: downloadedImgName, // Full path to the local file
      },
    ];

    // Provide AEM URL and DAM Path where images will be uploaded
    const options = new DirectBinaryUploadOptions()
      .withUrl(`${aemURL}${GENERATED_IMAGES_DAM_PATH}`)
      .withUploadFiles(uploadFiles);

    // Add headers like content type and authorization
    options.withHeaders({
      'content-type': 'image/png',
      Authorization: `Bearer ${accessToken}`,
    });

    // Start the upload to AEM
    await binaryUpload.uploadFiles(options)
      .then((result) => {
        // Handle Error
        result.getErrors().forEach((error) => {
          if (error.getCode() === codes.ALREADY_EXISTS) {
            logger.error('The generated image already exists');
          }
        });

        // Handle Upload result and check for errors
        result.getFileUploadResults().forEach((fileResult) => {
          // log file upload result
          logger.info(`File upload result ${JSON.stringify(fileResult)}`);

          fileResult.getErrors().forEach((fileErr) => {
            if (fileErr.getCode() === codes.ALREADY_EXISTS) {
              const fileName = fileResult.getFileName();
              logger.error(`The generated image already exists ${fileName}`);
            }
          });
        });
      })
      .catch((err) => {
        logger.info(`Failed to uploaded generated image to AEM${err}`);
      });

    logger.info('Successfully uploaded generated image to AEM');

    aemImageURL = `${aemURL + GENERATED_IMAGES_DAM_PATH}/${downloadedImgName}`;
  } catch (error) {
    logger.info(`Error while uploading generated image to AEM, see ${error}`);
  }

  return aemImageURL;
}

async function deleteFileFromRuntime(logger, downloadedImgName) {
  try {
    logger.log('Deleting the generated image from the runtime');

    fs.unlinkSync(downloadedImgName);

    logger.log('Successfully deleted the generated image from the runtime');
  } catch (error) {
    logger.error(`Error while deleting generated image from the runtime, details are: ${error}`);
  }
}

async function uploadGeneratedImageToAEM(params, generatedImageURL, accessToken) {
  // create a Logger
  const logger = Core.Logger('uploadGeneratedImageToAEM', { level: params.LOG_LEVEL || 'info' });

  const aemURL = params.aemHost;

  logger.info(`Uploading generated image from ${generatedImageURL} to AEM ${aemURL} by streaming the bytes.`);

  // download image to the App Builder runtime
  const downloadedImgName = await downloadImageToRuntime(logger, generatedImageURL);

  // Upload image to AEM from the App Builder runtime
  const aemImageURL = await uploadImageToAEMFromRuntime(logger, aemURL, downloadedImgName, accessToken);

  // Delete the downloaded image from the App Builder runtime
  await deleteFileFromRuntime(logger, downloadedImgName);

  return aemImageURL;
}

module.exports = {
  uploadGeneratedImageToAEM,
};
```

### 更新內容片段模組 —  `update-content-fragement.js`

此模組負責使用內容片段API使用新上載的映像的DAM路徑更新給定內容片段的映AEM像屬性。

+ `src/aem-cf-console-admin-1/actions/generate-image/update-content-fragement.js`

```javascript
/**
 * This module updates the CF image property with generated image's DAM path
 *
 */
const { Core } = require('@adobe/aio-sdk');

const ADVENTURE_MODEL_IMG_PROPERTY_NAME = 'primaryImage';

const ARTICLE_MODEL_IMG_PROPERTY_NAME = 'featuredImage';

const AUTHOR_MODEL_IMG_PROPERTY_NAME = 'profilePicture';

function findImgPropertyName(fragmenPath) {
  if (fragmenPath && fragmenPath.includes('/adventures')) {
    return ADVENTURE_MODEL_IMG_PROPERTY_NAME;
  } if (fragmenPath && fragmenPath.includes('/magazine')) {
    return ARTICLE_MODEL_IMG_PROPERTY_NAME;
  }
  return AUTHOR_MODEL_IMG_PROPERTY_NAME;
}

async function updateContentFragmentToUseGeneratedImg(params, uploadedImagePath, accessToken) {
  // create a Logger
  const logger = Core.Logger('updateContentFragment', { level: params.LOG_LEVEL || 'info' });

  const fragmenPath = params.fragmentId;
  const imgPropName = findImgPropertyName(fragmenPath);
  const relativeImgPath = uploadedImagePath.substring(uploadedImagePath.indexOf('/content/dam'));

  logger.info(`Update CF ${fragmenPath} to use ${relativeImgPath} image path`);

  const body = {
    properties: {
      elements: {
        [imgPropName]: {
          value: relativeImgPath,
        },
      },
    },
  };

  const res = await fetch(`${params.aemHost}${fragmenPath.replace('/content/dam/', '/api/assets/')}.json`, {
    method: 'put',
    body: JSON.stringify(body),
    headers: {
      Authorization: `Bearer ${accessToken}`,
      'Content-Type': 'application/json',
    },

  });

  if (res.ok) {
    logger.info(`Successfully updated ${fragmenPath}`);
    return fragmenPath;
  }

  logger.info(`Failed to update ${fragmenPath}`);
  return '';
}

module.exports = {
  updateContentFragmentToUseGeneratedImg,
};
```
