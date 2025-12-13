---
title: 內容片段主控台 — 自訂欄位
description: 瞭解如何在AEM內容片段編輯器中建立自訂欄位。
feature: Developer Tools, Content Fragments
version: Experience Manager as a Cloud Service
role: Developer
level: Intermediate
doc-type: Tutorial
duration: 315
last-substantial-update: 2024-02-27T00:00:00Z
jira: KT-14903
thumbnail: KT-14903.jpeg
exl-id: 563bab0e-21e3-487c-9bf3-de15c3a81aba
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '473'
ht-degree: 1%

---

# 自訂欄位

瞭解如何在AEM內容片段編輯器中建立自訂欄位。

>[!VIDEO](https://video.tv.adobe.com/v/3427585?learn=on)

AEM UI擴充功能應使用[Adobe React Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html)架構進行開發，因為這樣可與AEM其他部分維持一致的外觀與風格，並且擁有預先建立的廣泛功能庫，可減少開發時間。

## 擴充點

此範例以自訂實施取代內容片段編輯器中的現有欄位。

| AEM UI已擴充 | 擴充點 |
| ------------------------ | --------------------- |
| [內容片段編輯器](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [自訂表單元素轉譯](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/custom-fields/) |

## 範例擴充功能

此範例示範如何透過將標準欄位取代為預先定義SKU的自訂下拉式清單，將內容片段編輯器中的欄位值限製為預先定義的設定。 作者可從這個特定的SKU清單中選取。 雖然SKU通常來自產品資訊管理(PIM)系統，但此範例會透過靜態列出SKU來簡化。

此範例的原始程式碼為[可供下載](./assets/editor-custom-field/content-fragment-editor-custom-field-src.zip)。

### 內容片段模型定義

此範例繫結至任何名稱為`sku`的內容片段欄位（透過[的](#extension-registration)規則運算式符合`^sku$`），並以自訂欄位取代。 此範例使用已更新的WKND Adventure內容片段模式，定義如下：

![內容片段模型定義](./assets/editor-custom-field/content-fragment-editor.png)

儘管自訂SKU欄位顯示為下拉式清單，但其基礎模型仍設定為文字欄位。 自訂欄位實作只需要符合適當的屬性名稱和型別，協助以自訂下拉式版本取代標準欄位。


### 應用程式路由

在主要React元件`App.js`中，包含轉譯`/sku-field` React元件的`SkuField`路由。

`src/aem-cf-editor-1/web-src/src/components/App.js`

```javascript
import React from "react";
import ErrorBoundary from "react-error-boundary";
import { HashRouter as Router, Routes, Route } from "react-router-dom";
import ExtensionRegistration from "./ExtensionRegistration";
import SkuField from "./SkuField"; // Custom component implemented below

function App() {
  return (
    <Router>
      <ErrorBoundary onError={onError} FallbackComponent={fallbackComponent}>
        <Routes>
          <Route index element={<ExtensionRegistration />} />
          <Route
            exact path="index.html"
            element={<ExtensionRegistration />}
          />
          {/* This is the React route that maps to the custom field */}
          <Route
            exact path="/sku-field"
            element={<SkuField />}/>
        </Routes>
      </ErrorBoundary>
    </Router>
  )
  ...
}
...
```

此`/sku-field`對應至`SkuField`元件的自訂路由已在[延伸註冊](#extension-registration)下使用。

### 擴充功能註冊

`ExtensionRegistration.js`對應至index.html路由，是AEM擴充功能的進入點，並定義：

+ `getDefinitions()`中的Widget定義具有`fieldNameExp`和`url`屬性。 [自訂表單元素轉譯API參考](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/custom-fields/#api-reference)中有完整的可用屬性清單。
+ `url`屬性值，相對URL路徑(`/index.html#/skuField`)以載入欄位UI。

`src/aem-cf-editor-1/web-src/src/components/ExtensionRegistration.js`

```javascript
import { Text } from "@adobe/react-spectrum";
import { register } from "@adobe/uix-guest";
import { extensionId } from "./Constants";

function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId,
      methods: {
        field: {
          getDefinitions() {
            return [
              // Multiple fields can be registered here.
              {
                fieldNameExp: '^sku$',  // This is a regular expression that matches the field name in the Content Fragment Model to replace with React component specified in the `url` key.
                url: '/#/sku-field',    // The URL, which is mapped vai the Route in App.js, to the React component that will be used to render the field.
              },
              // Other bindings besides fieldNameExp, other bindings can be used as well as defined here:
              // https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/custom-fields/#api-reference
            ];
          },
        },
      }
    });
  };
  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}

export default ExtensionRegistration;
```

### 自訂欄位

`SkuField` React元件會使用Adobe React Spectrum做為選擇器表單，以自訂UI更新內容片段編輯器。 重點包括：

+ 利用`useEffect`進行初始化並連線至AEM的內容片段編輯器，並在安裝完成前顯示載入狀態。
+ 在iFrame內呈現時，它會透過`onOpenChange`函式動態調整iFrame的高度，以符合Adobe React頻譜選擇器的下拉式清單。
+ 使用`connection.host.field.onChange(value)`函式中的`onSelectionChange`將欄位選取內容傳回主機，確保選取的值已根據內容片段模型的指南驗證並自動儲存。

自訂欄位會在插入內容片段編輯器的iFrame中轉譯。 自訂欄位程式碼與內容片段編輯器之間的通訊僅透過`connection`物件進行，由`attach`封裝中的`@adobe/uix-guest`函式建立。

`src/aem-cf-editor-1/web-src/src/components/SkuField.js`

```javascript
import React, { useEffect, useState } from "react";
import { extensionId } from "./Constants";
import { attach } from "@adobe/uix-guest";
import { Provider, View, lightTheme } from "@adobe/react-spectrum";
import { Picker, Item } from "@adobe/react-spectrum";

const SkuField = (props) => {
  const [connection, setConnection] = useState(null);
  const [validationState, setValidationState] = useState(null);
  const [value, setValue] = useState(null);
  const [model, setModel] = useState(null);
  const [items, setItems] = useState(null);

  /**
   * Mock function that gets a list of Adventure SKUs to display.
   * The data could come anywhere, AEM's HTTP APIs, a PIM, or other system.
   * @returns a list of items for the picker
   */
  async function getItems() {
    // Data to drive input field validation can come from anywhere.
    // Fo example this commented code shows how it could be fetched from an HTTP API.
    // fetch(MY_API_URL).then((response) => response.json()).then((data) => { return data; }

    // In this example, for simplicity, we generate a list of 25 SKUs.
    return Array.from({ length: 25 }, (_, i) => ({ 
        name: `WKND-${String(i + 1).padStart(3, '0')}`, 
        id: `WKND-${String(i + 1).padStart(3, '0')}` 
    }));
  }

  /**
   * When the fields changes, update the value in the Content Fragment Editor
   * @param {*} value the selected value in the picker
   */
  const onSelectionChange = async (value) => {
    // This sets the value in the React state of the custom field
    setValue(value);
    // This calls the setValue method on the host (AEM's Content Fragment Editor)
    connection.host.field.onChange(value);
  };

  /**
   * Some widgets, like the Picker, have a variable height.
   * In these cases adjust the Content Fragment Editor's iframe's height so the field doesn't get cut off.     *
   * @param {*} isOpen true if the picker is open, false if it's closed
   */
  const onOpenChange = (isOpen) => {
    if (isOpen) {
      // Calculate the height of the picker box and its label, and surrounding padding.
      const pickerHeight = Number(document.body.clientHeight.toFixed(0));
      // Calculate the height of the picker options dropdown, and surrounding padding.
      // We do this  by multiplying the number of items by the height of each item (32px) and adding 12px for padding.
      const optionsHeight = items.length * 32 + 12;

      // Set the height of the iframe to the height of the picker + the height of the options, or 400px, whichever is smaller.
      // The options will scroll if they they cannot fit into 400px
      const height = Math.min(pickerHeight + optionsHeight, 400);

      // Set the height of the iframe in the Content Fragment Editor
      connection.host.field.setStyles({
        current: { height, },
        parent: { height, },
      })
    } else {
      connection.host.field.setStyles({
        current: { height: 74 },
        parent: { height: 74 },
      })
    }
  };

  useEffect(() => {
    const init = async () => {
      // Connect to the host (AEM's Content Fragment Editor)
      const conn = await attach({ id: extensionId });
      setConnection(conn);

      // get the Content Fragment Model
      setModel(await conn.host.field.getModel());

      // Share the validation state back to the client.
      // When conn.host.field.setValue(value) is called, the
      await conn.host.field.onValidationStateChange((state) => {
        // state can be `valid` or `invalid`.
        setValidationState(state);
      });
      // Get default value from the Content Fragment Editor
      // (either the default value set in the model, or a perviously set value)
      setValue(await conn.host.field.getDefaultValue());

      // Get the list of items for the picker; in this case its a list of adventure SKUs 
      // This could come from elsewhere in AEM or from an external system.
      setItems(await getItems());
    };

    init().catch(console.error);
  }, []);

  // If the component is not yet initialized, return a loading state.
  if (!connection || !model || !items) {
    // Put whatever loader you like here...
    return <Provider theme={lightTheme}>Loading custom field...</Provider>;
  }

  // Wrap the Spectrum UI component in a Provider theme, such that it is styled appropriately.
  // Render the picker, and bind to the data and custom event handlers.

  // Set as much of the model as we can, to allow maximum authoring flexibility without developer support.
  return (
    <Provider theme={lightTheme}>
      <View width="100%">
        <Picker
          label={model.fieldLabel}
          isRequired={model.required}
          placeholder={model.emptyText}
          errorMessage={model.customErrorMsg}
          selectedKey={value}
          necessityIndicator="icon"
          shouldFlip={false}
          width={"90%"}
          items={items}
          isInvalid={validationState === "invalid"}
          onSelectionChange={onSelectionChange}
          onOpenChange={onOpenChange}
        >
          {(item) => <Item key={item.value}>{item.name}</Item>}
        </Picker>
      </View>
    </Provider>
  );
};

export default SkuField;
```
