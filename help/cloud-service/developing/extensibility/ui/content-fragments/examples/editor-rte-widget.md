---
title: 將Widget新增至RTF編輯器(RTE)
description: 瞭解如何在AEM內容片段編輯器中將Widget新增至RTF編輯器(RTE)
feature: Developer Tools, Content Fragments
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13465
thumbnail: KT-13465.jpg
doc-type: article
last-substantial-update: 2023-06-12T00:00:00Z
exl-id: 167a4b11-1202-4c7a-b022-f3f996348a4e
duration: 476
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '553'
ht-degree: 0%

---

# 將Widget新增至RTF編輯器(RTE)

瞭解如何在AEM內容片段編輯器中將Widget新增至RTF編輯器(RTE)。

>[!VIDEO](https://video.tv.adobe.com/v/3420822?quality=12&learn=on)

若要在RTF編輯器(RTE)中新增動態內容，可以使用&#x200B;**Widget**&#x200B;功能。 Widget可協助您在RTE中整合簡單或複雜的UI，您也可以使用所選JS架構來建立UI。 它們可視為在RTE中按下`{`特殊鍵所開啟的對話方塊。

通常Widget是用來插入具有外部系統相依性或可能根據目前內容而變更的動態內容。

使用`rte`擴充點將&#x200B;**Widget**&#x200B;新增到內容片段編輯器中的&#x200B;**RTE**。 使用`rte`擴充點的`getWidgets()`方法新增一或多個Widget。 會透過按下`{`特殊鍵以開啟內容功能表選項來觸發，然後選取所需的Widget以載入自訂對話方塊UI。

此範例說明如何新增名為&#x200B;_折扣代碼清單_&#x200B;的Widget，以在RTE內容中尋找、選取及新增WKND冒險特有折扣代碼。 這些折扣代碼可在外部系統中管理，例如Order Management系統(OMS)、產品資訊管理(PIM)、自主開發應用程式或AdobeAppBuilder動作。

為了簡單起見，此範例使用[AdobeReact Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html)架構來開發Widget或對話方塊UI以及硬式編碼的WKND冒險名稱、折扣碼資料。

## 擴充點

此範例延伸至擴充點`rte`，在內容片段編輯器中將Widget新增至RTE。

| AEM UI已擴充 | 擴充點 |
| ------------------------ | --------------------- | 
| [內容片段編輯器](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [RTF編輯器Widget](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/) |

## 範例擴充功能

下列範例會建立&#x200B;_折扣代碼清單_ Widget。 在RTE中按下`{`特殊鍵，會開啟內容功能表，然後從內容功能表選取&#x200B;_折扣代碼清單_&#x200B;選項，會開啟對話方塊UI。

WKND內容作者可以尋找、選取及新增目前的Adventure特有折扣代碼（若有）。

### 擴充功能註冊

`ExtensionRegistration.js`對應至index.html路由，是AEM擴充功能的進入點，並定義：

+ `getWidgets()`函式中具有`id, label and url`屬性的Widget定義。
+ `url`屬性值，相對URL路徑(`/index.html#/discountCodes`)，可載入對話方塊UI。

`src/aem-cf-editor-1/web-src/src/components/ExtensionRegistration.js`

```javascript
import React from "react";
import { Text } from "@adobe/react-spectrum";
import { register } from "@adobe/uix-guest";
import { extensionId } from "./Constants";

// This function is called when the extension is registered with the host and runs in an iframe in the Content Fragment Editor browser window.
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId,
      methods: {
        rte: {
          // RTE Widgets
          getWidgets: () => [
            {
              id: "discountcode-list-widget", // Provide a unique ID for the widget
              label: "Discount Code List", // Provide a label for the widget
              url: "/index.html#/discountCodes", // Provide the "relative" URL to the widget content. It will be resolved as `/index.html#/discountCodes`
            },
          ],
        }, // Add a comma here
      },
    });
  };

  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}

export default ExtensionRegistration;
```

### 在`App.js`中新增`discountCodes`路由{#add-widgets-route}

在主要React元件`App.js`中，新增`discountCodes`路由以呈現上述相對URL路徑的UI。

`src/aem-cf-editor-1/web-src/src/components/App.js`

```javascript
...

<Routes>
  <Route index element={<ExtensionRegistration />} />
  <Route
    exact path="index.html"
    element={<ExtensionRegistration />}
  />

  {/* Content Fragment RTE routes that support the Discount Codes Widget functionality*/}
  <Route path="/discountCodes" element={<DiscountCodes />} />
</Routes>
...
```

### 建立`DiscountCodes` React元件{#create-widget-react-component}

Widget或對話方塊UI是使用[AdobeReact Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html)架構所建立。 `DiscountCodes`元件程式碼如下，以下是重點專案：

+ UI是使用React Spectrum元件來轉譯，例如[ComboBox](https://react-spectrum.adobe.com/react-spectrum/ComboBox.html)、[ButtonGroup](https://react-spectrum.adobe.com/react-spectrum/ButtonGroup.html)、[Button](https://react-spectrum.adobe.com/react-spectrum/Button.html)
+ `adventureDiscountCodes`陣列具有冒險名稱和折扣代碼的硬式編碼對應。 在真實情境中，此資料可以從AdobeAppBuilder動作或外部系統（例如PIM、OMS或自行開發的或雲端提供者型API閘道）擷取。
+ 已使用`useEffect` [React勾點](https://react.dev/reference/react/useEffect)初始化`guestConnection`，並管理為元件狀態。 它可用來與AEM主機通訊。
+ `handleDiscountCodeChange`函式取得所選冒險名稱的折扣碼，並更新狀態變數。
+ 使用`guestConnection`物件的`addDiscountCode`函式提供要執行的RTE指令。 在此情況下，會在RTE中插入實際折扣代碼的`insertContent`指示和HTML代碼片段。

`src/aem-cf-editor-1/web-src/src/components/DiscountCodes.js`

```javascript
import {
  Button,
  ButtonGroup,
  ComboBox,
  Content,
  Divider,
  Flex, Form,
  Item,
  Provider,
  Text,
  defaultTheme
} from '@adobe/react-spectrum';
import { attach } from '@adobe/uix-guest';
import React, { useEffect, useState } from 'react';
import { extensionId } from './Constants';

const DiscountCodes = () => {

  // The Adventure Discount Code list
  // In this example its hard coded, however you can call an Adobe AppBuilder Action or even make an AJAX call to load it from 3rd party system
  const adventureDiscountCodes = [
    { id: 1, adventureName: 'BALI SURF CAMP', discountCode: 'BALI2023' },
    { id: 2, adventureName: 'BEERVANA IN PORTLAND', discountCode: 'PORTFEST' },
    { id: 3, adventureName: 'NAPA WINE TASTING', discountCode: 'WINEINSPRING' },
    { id: 4, adventureName: 'RIVERSIDE CAMPING', discountCode: 'SUMMERSCAPE' },
    { id: 5, adventureName: 'TAHOE SKIING', discountCode: 'EPICPASS' },
  ];

  // Set up state used by the React component
  const [guestConnection, setGuestConnection] = useState();

  // State hooks to manage the component state
  const [discountCode, setDiscountCode] = useState(null);

  // Asynchronously attach the extension to AEM, we must wait or the guestConnection to be set before doing anything in the modal
  useEffect(() => {
    (async () => {
      const myGuestConnection = await attach({ id: extensionId });

      setGuestConnection(myGuestConnection);
    })();
  }, []);

  // Handle the `discountCodeList` Dropdown change
  const handleDiscountCodeChange = (key) => {

    if (key) {
      //console.log(`DiscountCode Key: ${key}`);
      //console.log(`DiscountCode Value: ${adventureDiscountCodes[key-1].discountCode}`);

      //Get discount code value using selected key/index
      let discountCodeValue = adventureDiscountCodes[key - 1].discountCode;

      //update the `discountCode` state
      setDiscountCode(discountCodeValue);
    }
  };

  // Add the selected Adventure's Discount Code into the RTE
  const addDiscountCode = () => {

    if (discountCode) {
      // Use `guestConnection.host.rte.applyInstructions` method and provide RTE instruction to execute.
      // The instructions are passed as an array of object, that has `type` and `value` keys
      guestConnection.host.rte.applyInstructions([{ type: "insertContent", value: `<strong>Discount Code: ${discountCode}</strong>` }]);
    }

  };

  // Adobe React Spectrum (HTML code) that renders the Discount Code dropdown list, see https://react-spectrum.adobe.com/react-spectrum/index.html
  return (
    <Provider theme={defaultTheme}>
      <Content width="100%">
        <Flex width="100%">

          <Form width="50%">

            <Text>Selected Discount Code: <strong>{discountCode}</strong></Text>

            <p />

            <Divider size="M" />


            <ComboBox
              name="discountCodeList"
              label="Type or Select an Adventure name"
              defaultItems={adventureDiscountCodes}
              onSelectionChange={handleDiscountCodeChange}>
              {item => <Item>{item.adventureName}</Item>}
            </ComboBox>

            <p />

            <ButtonGroup align="right">
              <Button variant="accent" onPress={addDiscountCode} autoFocus>Add</Button>
              <Button variant="secondary" onPress={() => setDiscountCode(null)}>Clear</Button>
            </ButtonGroup>

          </Form>
        </Flex>
      </Content>
    </Provider>
  );
}

export default DiscountCodes;
```
