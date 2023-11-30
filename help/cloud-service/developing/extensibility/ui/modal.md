---
title: AEM UI擴充功能模型
description: 瞭解如何建立AEM UI擴充功能模型。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
jira: KT-11603
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: e7376eaf-f7d7-48fe-9387-a0e4089806c2
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '313'
ht-degree: 0%

---

# 擴充功能模型

![AEM UI擴充功能模型](./assets/modal/modal.png){align="center"}

AEM UI擴充功能模式提供一種將自訂UI附加至AEM UI擴充功能的方法。

模型是React應用程式，根據 [React Spectrum](https://react-spectrum.adobe.com/react-spectrum/)，和可以建立擴充功能所需的任何自訂UI，包含但不限於：

+ 確認對話方塊
+ [輸入表單](https://react-spectrum.adobe.com/react-spectrum/#forms)
+ [進度指示器](https://react-spectrum.adobe.com/react-spectrum/#status)
+ [結果摘要](https://react-spectrum.adobe.com/react-spectrum/#collections)
+ 錯誤訊息
+ ...甚至完整的多重檢視React應用程式！

## 強制回應路由

強制回應體驗由以下定義的擴充功能App Builder React應用程式定義： `web-src` 資料夾。 和任何React應用程式一樣，完整體驗也是使用來編排 [React路由](https://reactrouter.com/en/main/components/routes) 該轉譯器 [React元件](https://reactjs.org/docs/components-and-props.html).

產生初始模組檢視至少需要一個路由。 此初始路由會在 [延伸註冊](#extension-registration)的 `onClick(..)` 函式，如下所示。


+ `./src/aem-ui-extension/web-src/src/components/App.js`

```javascript
import MyModal from './MyModal';
import MyModalResults from './MyModalResults';
...
function App(props) {
  return (
    <Router>
      <ErrorBoundary onError={onError} FallbackComponent={fallbackComponent}>
        <Routes>
          ...         
          {/* 
            Define the entry route to the modal.

            For modals opened from Action Bar extensions.
            Depending on the extension point, different parameters are passed to the modal.
            This example illustrates a modal for the AEM Content Fragment Console (list view), where typically a :selection parameter is used to pass in the list of selected Content Fragments.
            Where as Header Menu extensions do not use a selection parameter.
           */}
          <Route
            exact path="aem-ui-extension/:selection/my-modal"
            element={<MyModal />}
          />                    

          {/* Define any other routes the modal may need */}
          <Route
            exact path="aem-ui-extension/my-modal"
            element={<MyOtherModalView />}
          />                    

        </Routes>
      </ErrorBoundary>
    </Router>
  )
  ...
}
```

## 擴充功能註冊

若要開啟強制回應，請呼叫 `guestConnection.host.modal.showUrl(..)` 由擴充功能的 `onClick(..)` 函式。 `showUrl(..)` 傳遞的JavaScript物件具有索引鍵/值：

+ `title` 提供向使用者顯示的強制回應視窗標題名稱
+ `url` 是叫用 [React路由](#modal-routes) 負責強制回應視窗的初始檢視。

當務之急是 `url` 傳遞至 `guestConnection.host.modal.showUrl(..)` 會解析成擴充功能中的路由，否則強制回應視窗中不會顯示任何內容。

+ `./src/aem-ui-extension/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  ...
  onClick() {
    // Create a URL that maps to the React route to be rendered in the modal
    const modalURL = "/index.html#/aem-ui-extension/my-modal";

    // Open the modal and display the React route created above
    guestConnection.host.modal.showUrl({
      title: "My modal title",
      url: modalURL
    })     
  }
  ...     
}...
```

## 模組元件

每個延伸路線， [這不是 `index` 路由](./extension-registration.md#app-routes)，對應至可在擴充功能強制回應視窗中呈現的React元件。

強制回應可由任意數量的React路徑組成，從簡單的單路徑強制回應到複雜的多路徑強制回應。

以下說明一個簡單的單路由模型，但此模型檢視可能包含叫用其他路由或行為的React連結。

+ `./src/aem-ui-extension/web-src/src/components/MyModal.js`

```javascript
import React, { useState, useEffect } from 'react'
import { attach } from "@adobe/uix-guest"
import {
  Flex,
  Provider,
  Content,
  defaultTheme,
  Text,
  ButtonGroup,
  Button
} from '@adobe/react-spectrum'
import Spinner from "./Spinner"
import { useParams } from "react-router-dom"
import { extensionId } from "./Constants"

export default function MyModal() {
  // Initial modal views for Action Bar extensions typically pass in the list of selected Content Fragment Paths from ExtensionRegistration.js
  // Get the paths from useParams() and split on delimiter used
  let { selection } = useParams();
  let contentFragmentPaths = selection?.split('|') || [];
  
  // Asynchronously attach the extension to AEM. 
  // Wait or the guestConnection to be set before doing anything in the modal.
  const [guestConnection, setGuestConnection] = useState()

  useEffect(() => {
    (async () => {
      const guestConnection = await attach({ id: extensionId })
      setGuestConnection(guestConnection);
    })()
  }, [])

  if (!guestConnection) {
    // If the guestConnection is not initialized, display a loading spinner
    return <Spinner />
  } else {
    // Else the modal is ready to render!
    return (
        <Provider theme={defaultTheme} colorScheme='light'>
        {/* 
            Use the React Spectrum components to render the modal UI.
            Using React Spectrum ensures a consistent, accessible, future-proof look-and-feel and speeds up development.
        */}
            <Content width="100%">
                <Flex width="100%">
                    <Text>
                        This is the contents in the modal! 
                        Anything can be created in this return statement!

                        The selected Content Fragments are: { contentFragmentPaths.join(', ') }
                    </Text>                    
                    {/*
                        Modals must provide their own Close button, by calling: guestConnection.host.modal.close()
                    */}
                    <ButtonGroup align="end">
                        <Button variant="primary" onPress={() => guestConnection.host.modal.close()}>Close</Button>
                    </ButtonGroup>
                </Flex>
            </Content>
        </Provider>
    )
  }
}
```

## 關閉強制回應視窗

![AEM UI擴充功能強制關閉按鈕](./assets/modal/close.png){align="center"}

模組必須提供自己的緊密控制。 這可透過叫用來完成 `guestConnection.host.modal.close()`.

```javascript
<ButtonGroup align="end">
    <Button variant="primary" onPress={() => guestConnection.host.modal.close()}>Close</Button>
</ButtonGroup>
```
