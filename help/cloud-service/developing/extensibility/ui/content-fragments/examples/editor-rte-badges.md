---
title: 新增徽章至RTF編輯器(RTE)
description: 瞭解如何在AEM內容片段編輯器中新增徽章至RTF編輯器(RTE)
feature: Developer Tools, Content Fragments
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13466
thumbnail: KT-13466.jpg
doc-type: article
last-substantial-update: 2023-06-12T00:00:00Z
exl-id: 83acbddb-9168-4d8b-84b5-97577d8a1ead
duration: 538
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '729'
ht-degree: 0%

---

# 新增徽章至RTF編輯器(RTE)

瞭解如何在AEM內容片段編輯器中新增徽章至RTF編輯器(RTE)。

>[!VIDEO](https://video.tv.adobe.com/v/3420831?quality=12&learn=on)

[RTF編輯器徽章](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/)是擴充，使得RTF編輯器(RTE)中的文字無法編輯。 這表示宣告為此類的徽章只能完全移除且不能部分編輯。 這些徽章也支援RTE內的特殊著色，明確向內容作者指出文字是徽章，因此不可編輯。 此外，這類提示還會提供有關徽章文字含義的視覺提示。

RTE徽章最常見的使用案例是與[RTE Widget](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/)搭配使用。 這可讓RTE Widget插入RTE的內容不可編輯。

通常，與Widget關聯的徽章是用來新增具有外部系統相依性的動態內容，但&#x200B;_內容作者無法修改_&#x200B;插入的動態內容以維持完整性。 它們只能當作整個專案移除。

使用`rte`擴充點將&#x200B;**徽章**&#x200B;新增到內容片段編輯器中的&#x200B;**RTE**。 使用`rte`擴充點的`getBadges()`方法新增一或多個徽章。

此範例說明如何新增名為&#x200B;_Large Group Bookings Customer Service_&#x200B;的Widget，以在RTE內容中尋找、選取和新增特定於WKND冒險的客戶服務詳細資料，例如&#x200B;**代表姓名**&#x200B;和&#x200B;**電話號碼**。 使用徽章功能，**電話號碼**&#x200B;已變成&#x200B;**不可編輯**，但WKND內容作者可以編輯代表名稱。

此外，**電話號碼**&#x200B;的樣式不同（藍色），這是徽章功能的額外使用案例。

為了簡單起見，此範例使用[Adobe React Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html)架構來開發Widget或對話方塊UI以及硬式編碼的WKND客戶服務電話號碼。 為了控制內容的非編輯和不同的樣式方面，在徽章定義的`prefix`和`suffix`屬性中使用`#`字元。

## 擴充功能點

此範例延伸至擴充點`rte`，在內容片段編輯器中將徽章新增至RTE。

| AEM UI已擴充 | 擴充功能點 |
| ------------------------ | --------------------- | 
| [內容片段編輯器](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [RTF編輯器徽章](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/)和[RTF編輯器Widget](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/) |

## 範例擴充功能

下列範例會建立&#x200B;_大型群組預約客戶服務_ Widget。 按下RTE內的`{`鍵，會開啟RTE Widget內容功能表。 從內容功能表選取&#x200B;_Large Group Bookings Customer Service_&#x200B;選項，即可開啟自訂強制回應視窗。

從強制回應視窗新增所需的客戶服務號碼後，徽章會使&#x200B;_電話號碼不可編輯_，並將其樣式設定為藍色。

### 擴充功能註冊

對應至`index.html`路由的`ExtensionRegistration.js`是AEM擴充功能的進入點，並定義：

+ 在`getBadges()`中使用組態屬性`id`、`prefix`、`suffix`、`backgroundColor`和`textColor`定義徽章的定義。
+ 在此範例中，`#`字元用於定義此徽章的邊界，表示RTE中由`#`包圍的任何字串都會被視為此徽章的執行個體。

此外，也請參閱RTE Widget的重要詳細資訊：

+ `getWidgets()`中的Widget定義具有`id`、`label`和`url`屬性。
+ `url`屬性值，載入強制回應視窗的相對URL路徑(`/index.html#/largeBookingsCustomerService`)。


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
          // RTE Badges
          getBadges: () => [
            {
              id: "phoneNumber",
              prefix: "#",
              suffix: "#",
              backgroundColor: "",
              textColor: "#071DF8",
            },
          ],

          // RTE Widgets
          getWidgets: () => [
            {
              id: "largegroup-contact-list-widget",
              label: "Large Group Bookings Customer Service",
              url: "/index.html#/largeBookingsCustomerService",
            },
          ],
        },
      },
    });
  };

  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}

export default ExtensionRegistration;
```

### 在`App.js`中新增`largeBookingsCustomerService`路由{#add-widgets-route}

在主要React元件`App.js`中，新增`largeBookingsCustomerService`路由以呈現上述相對URL路徑的UI。

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
  <Route path="/largeBookingsCustomerService" element={<LargeBookingsCustomerService />} />
</Routes>
...
```

### 建立`LargeBookingsCustomerService` React元件{#create-widget-react-component}

Widget或對話方塊UI是使用[Adobe React Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html)架構所建立。

新增客戶服務詳細資料時，React元件程式碼會在電話號碼變數周圍加上`#`個註冊的徽章字元，以將其轉換為徽章，例如`#${phoneNumber}#`，因此使其不可編輯。

以下是`LargeBookingsCustomerService`程式碼的關鍵重點專案：

+ UI是使用React Spectrum元件來轉譯，例如[ComboBox](https://react-spectrum.adobe.com/react-spectrum/ComboBox.html)、[ButtonGroup](https://react-spectrum.adobe.com/react-spectrum/ButtonGroup.html)、[Button](https://react-spectrum.adobe.com/react-spectrum/Button.html)
+ `largeGroupCustomerServiceList`陣列具有代表名稱與電話號碼的硬式編碼對應。 在真實情境中，此資料可以從Adobe AppBuilder動作或外部系統、自行開發或雲端提供者為基礎的API閘道擷取。
+ 已使用`useEffect` [React勾點](https://react.dev/reference/react/useEffect)初始化`guestConnection`，並管理為元件狀態。 它可用來與AEM主機通訊。
+ `handleCustomerServiceChange`函式取得代表名稱和電話號碼，並更新元件狀態變數。
+ 使用`guestConnection`物件的`addCustomerServiceDetails`函式提供要執行的RTE指令。 在此案例中，`insertContent`指示和HTML程式碼片段。
+ 若要使用徽章讓&#x200B;**電話號碼不可編輯**，請在`phoneNumber`變數（如`...<div><p>Phone Number: #${phoneNumber}#</strong></p></div>`）之前和之後新增`#`特殊字元。

`src/aem-cf-editor-1/web-src/src/components/LargeBookingsCustomerService.js`

```javascript
import {
  Button,
  ButtonGroup,
  Text,
  Divider,
  ComboBox,
  Content, Flex, Form,
  Item,
  Provider, defaultTheme
} from '@adobe/react-spectrum';
import { attach } from '@adobe/uix-guest';
import React, { useEffect, useState } from 'react';
import { extensionId } from './Constants';


const LargeBookingsCustomerService = () => {

  // The Large Group Bookings Customer Service
  // In this example its hard coded, however you can call an Adobe AppBuilder Action or even make an AJAX call to load it from 3rd party system
  const largeGroupCustomerServiceList = [
    { id: 1, repName: 'Max', phoneNumber: '1-800-235-1000' },
    { id: 2, repName: 'John', phoneNumber: '1-700-235-2000' },
    { id: 3, repName: 'Leah', phoneNumber: '1-600-235-3000' },
    { id: 4, repName: 'Leno', phoneNumber: '1-500-235-4000' }
  ];

  // Set up state used by the React component
  const [guestConnection, setGuestConnection] = useState();

  // State hooks to manage the component state
  const [repName, setRepName] = useState(null);
  const [phoneNumber, setPhoneNumber] = useState(null);

  // Asynchronously attach the extension to AEM, we must wait or the guestConnection to be set before doing anything in the modal
  useEffect(() => {
    (async () => {
      const myGuestConnection = await attach({ id: extensionId });

      setGuestConnection(myGuestConnection);
    })();
  }, []);

  // Handle the `customerService` Dropdown change
  const handleCustomerServiceChange = (id) => {

    if (id) {
      //Get Customer Service RepName and Phone Number values using selected id

      const rep = largeGroupCustomerServiceList.filter((r) => r.id === id)[0];

      //update the `repName` state
      setRepName(rep?.repName);

      //update the `phoneNumber` state
      setPhoneNumber(rep?.phoneNumber);
    }
  };

  // Add the selected Customer Service details into the RTE
  const addCustomerServiceDetails = () => {

    if (repName && phoneNumber) {
      // Use `guestConnection.host.rte.applyInstructions` method and provide RTE instruction to execute.
      // The instructions are passed as an array of object, that has `type` and `value` keys
      guestConnection.host.rte.applyInstructions([{ type: "insertContent", value: `<div><p>Representative Name: <strong>${repName}</strong></p></div><div><p>Phone Number: #${phoneNumber}#</strong></p></div>` }]);
    }
  };


  // Adobe React Spectrum (HTML code) that renders the Customer Service dropdown list, see https://react-spectrum.adobe.com/react-spectrum/index.html
  return (
    <Provider theme={defaultTheme}>
      <Content width="100%">
        <Flex width="100%">

          <Form width="50%">
            <Text>Representative Name: <strong>{repName}</strong></Text>
            <Text>Phone Number: <strong>{phoneNumber}</strong></Text>

            <p />

            <Divider size="M" />


            <ComboBox
              name="customerService"
              label="Type or Select Phone Number"
              defaultItems={largeGroupCustomerServiceList}
              onSelectionChange={handleCustomerServiceChange}>
              {item => <Item>{item.phoneNumber}</Item>}
            </ComboBox>

            <p />

            <ButtonGroup align="right">
              <Button variant="accent" onPress={addCustomerServiceDetails}>Add</Button>
              <Button variant="secondary" onPress={() => {setPhoneNumber(null); setRepName(null);}}>Clear</Button>
            </ButtonGroup>

          </Form>
        </Flex>
      </Content>
    </Provider>
  );

};

export default LargeBookingsCustomerService;
```
