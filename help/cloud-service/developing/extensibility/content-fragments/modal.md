---
title: 內AEM容片段控制台擴展模式
description: 瞭解如何建立內容AEM片段控制台擴展模式。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
exl-id: e7376eaf-f7d7-48fe-9387-a0e4089806c2
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '344'
ht-degree: 0%

---

# 擴展模式

![內AEM容片段擴展模式](./assets/modal/modal.png){align="center"}

內AEM容片段擴展模式提供了將自定義UI附AEM加到內容片段擴展的方法 [操作欄](./action-bar.md) 或 [標題菜單](./header-menu.md) 按鈕。

模組是React應用程式，基於 [反應光譜](https://react-spectrum.adobe.com/react-spectrum/)，並且可以建立擴展所需的任何自定義UI，包括但不限於：

+ 確認對話框
+ [輸入表單](https://react-spectrum.adobe.com/react-spectrum/#forms)
+ [進展指標](https://react-spectrum.adobe.com/react-spectrum/#status)
+ [結果摘要](https://react-spectrum.adobe.com/react-spectrum/#collections)
+ 錯誤消息
+ ...甚至是全面、多視圖的React應用程式！

## 模式路由

模式體驗由在以下位置定義的擴展App Builder React應用定義 `web-src` 的子菜單。 與任何React應用一樣，完整的體驗是使用 [反應路由](https://reactrouter.com/en/main/components/routes) 呈現 [反應元件](https://reactjs.org/docs/components-and-props.html)。

至少需要一條路由才能生成初始模態視圖。 在 [擴展註冊](#extension-registration)`s `onClick(..)` 函式，如下所示。


+ `./src/aem-cf-console-admin-1/web-src/src/components/App.js`

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

            For modals opened from Action Bar extensions, typically a :selection parameter is used to pass in the list of selected Content Fragments.
            Header Menu extensions do not use a selection parameter.
           */}
          <Route
            exact path="content-fragment/:selection/my-modal"
            element={<MyModal />}
          />                    

          {/* Define any other routes the modal may need */}
          <Route
            exact path="content-fragment/my-modal"
            element={<MyOtherModalView />}
          />                    

        </Routes>
      </ErrorBoundary>
    </Router>
  )
  ...
}
```

## 分機註冊

要開啟模式，請調用 `guestConnection.host.modal.showUrl(..)` 是由分機 `onClick(..)` 的子菜單。 `showUrl(..)` 傳遞的JavaScript對象具有鍵/值：

+ `title` 提供顯示給用戶的模式標題的名稱
+ `url` 是調用 [反應路由](#modal-routes) 負責模型的初始視圖。

當務之急是 `url` 已傳遞 `guestConnection.host.modal.showUrl(..)` 解析為在擴展中佈線，否則在模式中不顯示任何內容。

查看 [標題菜單](./header-menu.md#modal) 和 [操作欄](./action-bar.md#modal) 文檔，瞭解如何建立模式URL。

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  ...
  onClick() {
    // Create a URL that maps to the React route to be rendered in the modal
    const modalURL = "/index.html#/content-fragment/my-modal";

    // Open the modal and display the React route created above
    guestConnection.host.modal.showUrl({
      title: "My modal title",
      url: modalURL
    })     
  }
  ...     
}...
```

## 模態分量

每條分機路線， [那不是 `index` 路線](./extension-registration.md#app-routes)，映射到可在擴展模式中呈現的「反應」(React)元件。

模式可以由任意數量的「反應」路由組成，從簡單的單路由模式到複雜的多路由模式。

下面說明了簡單的單路由模式，但此模式視圖可能包含調用其他路由或行為的「反應」連結。

+ `./src/aem-cf-console-admin-1/web-src/src/components/MyModal.js`

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

## 關閉模式

![「內AEM容片段擴展模式關閉」按鈕](./assets/modal/close.png){align="center"}

模型必須提供自己的封閉控制。 通過調用 `guestConnection.host.modal.close()`。

```javascript
<ButtonGroup align="end">
    <Button variant="primary" onPress={() => guestConnection.host.modal.close()}>Close</Button>
</ButtonGroup>
```
