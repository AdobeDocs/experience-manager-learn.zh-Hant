---
title: 新增徽章至RTF編輯器(RTE)
description: 瞭解如何在AEM內容片段編輯器中新增徽章至RTF編輯器(RTE)
feature: Developer Tools, Content Fragments
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13466
thumbnail: KT-13466.jpg
doc-type: article
last-substantial-update: 2023-06-12T00:00:00Z
exl-id: 83acbddb-9168-4d8b-84b5-97577d8a1ead
duration: 555
source-git-commit: 6f1245e804f0311c3f833ea8b2324cbc95272f52
workflow-type: tm+mt
source-wordcount: '729'
ht-degree: 0%

---

# 新增徽章至RTF編輯器(RTE)

瞭解如何在AEM內容片段編輯器中新增徽章至RTF編輯器(RTE)。

>[!VIDEO](https://video.tv.adobe.com/v/3420831?quality=12&learn=on)

[RTF編輯器徽章](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/)  是RTF編輯器(RTE)中文字無法編輯的擴充功能。 這表示宣告為此類的徽章只能完全移除且不能部分編輯。 這些徽章也支援RTE內的特殊著色，明確向內容作者指出文字是徽章，因此不可編輯。 此外，這類提示還會提供有關徽章文字含義的視覺提示。

RTE徽章最常見的使用案例是搭配使用 [RTE Widget](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/). 這可讓RTE Widget插入RTE的內容不可編輯。

通常，與Widget相關聯的徽章會用於新增具有外部系統相依性，但 _內容作者無法修改_ 插入的動態內容以保持完整性。 它們只能當作整個專案移除。

此 **徽章** 新增至 **RTE** 在內容片段編輯器中使用 `rte` 延伸點。 使用 `rte` 延伸點的 `getBadges()` 方法新增一或多個徽章。

此範例說明如何新增名為的Widget _大型群組預約客戶服務_ 尋找、選擇並新增WKND探險特有客戶服務詳細資訊，例如 **代表姓名** 和 **電話號碼** 在RTE內容中。 使用徽章功能 **電話號碼** 已進行 **不可編輯** 但WKND內容作者可以編輯「代表名稱」。

此外， **電話號碼** 樣式不同（藍色），這是badges功能的額外使用案例。

為了簡單起見，此範例使用 [AdobeReact Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html) 框架來開發Widget或對話方塊UI以及硬式編碼的WKND客戶服務電話號碼。 若要控制內容的非編輯及不同樣式方面， `#` 字元用於 `prefix` 和 `suffix` 徽章定義的屬性。

## 延伸點

此範例會延伸至擴充點 `rte` 在內容片段編輯器中，將徽章新增到RTE。

| AEM UI已擴充 | 延伸點 |
| ------------------------ | --------------------- | 
| [內容片段編輯器](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [RTF編輯器徽章](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/) 和 [RTF編輯器Widget](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/) |

## 範例擴充功能

下列範例會建立 _大型群組預約客戶服務_ Widget. 按下 `{` 鍵（在RTE中），RTE Widget內容功能表隨即開啟。 藉由選取 _大型群組預約客戶服務_ 自訂強制回應視窗會從快顯選單中開啟。

從強制回應視窗新增所需的客戶服務編號後，徽章會將 _電話號碼不可編輯_ 並以藍色設定樣式。

### 擴充功能註冊

`ExtensionRegistration.js`，對應至 `index.html` route，是AEM延伸模組的進入點，並定義：

+ 此徽章的定義定義定義於 `getBadges()` 使用設定屬性 `id`， `prefix`， `suffix`， `backgroundColor` 和 `textColor`.
+ 在此範例中， `#` 字元可用來定義此徽章的邊界，也就是RTE中由所包住的任何字串 `#` 會視為此徽章的例項。

此外，也請參閱RTE Widget的重要詳細資訊：

+ 中的Widget定義 `getWidgets()` 函式為 `id`， `label` 和 `url` 屬性。
+ 此 `url` 屬性值，相對URL路徑(`/index.html#/largeBookingsCustomerService`)以載入強制回應視窗。


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

### 新增 `largeBookingsCustomerService` 路由 `App.js`{#add-widgets-route}

在主要React元件中 `App.js`，新增 `largeBookingsCustomerService` 呈現上述相對URL路徑UI的路由。

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

### 建立 `LargeBookingsCustomerService` React元件{#create-widget-react-component}

Widget或對話方塊UI是使用 [AdobeReact Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html) 框架。

新增客戶服務詳細資料時，React元件程式碼會在電話號碼變數周圍加上 `#` 註冊的徽章字元，可將其轉換為徽章，例如 `#${phoneNumber}#`，因此使其不可編輯。

以下是的關鍵重點專案 `LargeBookingsCustomerService` 程式碼：

+ UI會使用React Spectrum元件(例如 [下拉式方塊](https://react-spectrum.adobe.com/react-spectrum/ComboBox.html)， [ButtonGroup](https://react-spectrum.adobe.com/react-spectrum/ButtonGroup.html)， [按鈕](https://react-spectrum.adobe.com/react-spectrum/Button.html)
+ 此 `largeGroupCustomerServiceList` 陣列具有代表名稱與電話號碼的硬式編碼對應。 在真實情境中，此資料可以從AdobeAppBuilder動作或外部系統、自行開發或雲端提供者為基礎的API閘道擷取。
+ 此 `guestConnection` 已使用 `useEffect` [React勾點](https://react.dev/reference/react/useEffect) 和managed as component state （管理元件狀態）。 它可用來與AEM主機通訊。
+ 此 `handleCustomerServiceChange` 函式取得代表名稱和電話號碼，並更新元件狀態變數。
+ 此 `addCustomerServiceDetails` 函式使用 `guestConnection` 物件提供要執行的RTE指令。 在此案例中 `insertContent` 指示和HTML程式碼片段。
+ 若要進行 **電話號碼不可編輯** 使用徽章， `#` 特殊字元會新增在之前和之後 `phoneNumber` 變數，例如 `...<div><p>Phone Number: #${phoneNumber}#</strong></p></div>`.

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
