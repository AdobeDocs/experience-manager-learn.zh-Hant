---
title: 內AEM容片段控制台擴展註冊
description: 瞭解如何註冊內容片段控制台擴展。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
exl-id: ef2290d9-ba40-429b-b10d-e82d6c1c20f6
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '569'
ht-degree: 0%

---

# 分機註冊

內AEM容片段控制台擴展是基於React的專用App Builder應用，使用 [反應光譜](https://react-spectrum.adobe.com/react-spectrum/) UI框架。

要定義擴展的內AEM容片段控制台的顯示位置和方式，擴展的App Builder應用中需要兩種特定的配置：應用路由和分機註冊。

## 應用路由{#app-routes}

分機 `App.js` 宣佈 [反應路由器](https://reactrouter.com/en/main) 包含在內容片段控制台中註冊擴展的AEM索引路由。

在開始載入內容片段控AEM制台時，將調用索引路由，此路由的目標定義擴展在控制台中的顯示方式。

+ `./src/aem-cf-console-admin-1/web-src/src/components/App.js`

```javascript
import ExtensionRegistration from "./ExtensionRegistration"
...            
function App(props) {
  return (
    <Router>
      <ErrorBoundary onError={onError} FallbackComponent={fallbackComponent}>
        <Routes>
          {/* The index route maps to the extension registration */}
          <Route index element={<ExtensionRegistration />} />
          ...                                   
        </Routes>
      </ErrorBoundary>
    </Router>
  )
  ...
}
```

## 分機註冊

`ExtensionRegistration.js` 必須通過擴展的索引路由立即載入，並且執行擴展的註冊點，定義：

1. 擴展類型；a [標題菜單](./header-menu.md) 或 [操作欄](./action-bar.md) 按鈕
   + [標題菜單](./header-menu.md#extension-registration) 擴展由 `headerMenu` 財產 `methods`。
   + [操作欄](./action-bar.md#extension-registration) 擴展由 `actionBar` 財產 `methods`。
1. 擴展按鈕的定義，在 `getButton()` 的子菜單。 此函式返回一個包含欄位的對象：
   + `id` 是按鈕的唯一ID
   + `label` 是「內容片段」控制台中擴展按AEM鈕的標籤
   + `icon` 是「內容片段」控制台中擴展按鈕AEM的表徵圖。 表徵圖是 [反應光譜](https://spectrum.adobe.com/page/icons/) 表徵圖名稱，刪除空格。
1. 按鈕的按一下處理程式，在 `onClick()` 的子菜單。
   + [標題菜單](./header-menu.md#extension-registration) 擴展不會將參數傳遞給按一下處理程式。
   + [操作欄](./action-bar.md#extension-registration) 擴展提供選定內容片段路徑的清單 `selections` 的下界。

### 標題菜單擴展

![標題菜單擴展](./assets/extension-registration/header-menu.png)

未選擇「內容片段」時，將顯示「標題菜單」擴展按鈕。 由於標題菜單擴展對內容片段選擇不起作用，因此不向其提供內容片段 `onClick()` 處理程式。

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId, // A unique ID for the extension
      methods: {
        // Configure your Action Bar button here
        headerMenu: {
          getButton() {
            return {
              'id': 'example.my-header-menu-extension', // Unique ID for the button
              'label': 'My header menu extension',      // Button label 
              'icon': 'Edit'                            // Button icon from https://spectrum.adobe.com/page/icons/
            }
          },

          // Click handler for the extension button
          // Header Menu extensions do not pass parameters to the click handler
          onClick() { ... }
        }
      }
    })
  }
  init().catch(console.error)
}
```

<div class="column is-8-desktop is-full-mobile is-half-tablet" style="
    border: solid 1px #ccc;
    border-radius: 10px;
    margin: 4rem auto;
">
  <div class="is-flex is-padded-small is-padded-big-mobile">
    <div>
      <p class="has-text-weight-bold is-size-36 is-size-27-touch is-margin-bottom-big has-text-blackest">跳至生成標題菜單擴展</p>
      <p class="has-text-blackest">瞭解如何在「內容片段控制台」中註冊和定AEM義標題菜單擴展。</p>
      <div class="has-align-start is-margin-top-big">
        <a href="./header-menu.md" target="_blank" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
          <span class="spectrum-Button-label has-no-wrap has-text-weight-bold" title="學習生成標題菜單擴展">學習生成標題菜單擴展</span>
        </a>
      </div>
    </div>
  </div>
</div>

### 操作欄擴展

![操作欄擴展](./assets/extension-registration/action-bar.png)

選擇一個或多個內容片段時，將顯示操作欄擴展按鈕。 所選內容片段的路徑通過 `selections` 參數，在按鈕的 `onClick(..)` 處理程式。

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId, // A unique ID for the extension
      methods: {
        // Configure your Action Bar button here
        actionBar: {
          getButton() {
            return {
              'id': 'example.my-action-bar-extension',  // Unique ID for the button
              'label': 'My action bar extension',       // Button label 
              'icon': 'Edit'                            // Button icon from https://spectrum.adobe.com/page/icons/
            }
          },

          // Click handler for the extension button
          // Only Action Bar buttons populate the selections parameter
          onClick(selections) { ... }
        }
      }
    })
  }
  init().catch(console.error)
}
```

<div class="column is-8-desktop is-full-mobile is-half-tablet" style="
    border: solid 1px #ccc;
    border-radius: 10px;
    margin: 4rem auto;
">
  <div class="is-flex is-padded-small is-padded-big-mobile">
    <div>
      <p class="has-text-weight-bold is-size-36 is-size-27-touch is-margin-bottom-big has-text-blackest">跳至生成操作欄擴展</p>
      <p class="has-text-blackest">瞭解如何在「內容片段控制台」中註冊和定AEM義操作欄擴展。</p>
      <div class="has-align-start is-margin-top-big">
        <a href="./action-bar.md" target="_blank" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
          <span class="spectrum-Button-label has-no-wrap has-text-weight-bold" title="學習生成操作欄擴展">學習生成操作欄擴展</span>
        </a>
      </div>
    </div>
  </div>
</div>

## 有條件地包括擴展

內AEM容片段控制台擴展可以執行自定義邏輯，以在內容片段控制台中顯AEM示該擴展時限制。 此檢查在 `register` 呼叫 `ExtensionRegistration` ，如果不顯示擴展，則立即返回。

此檢查的上下文有限：

+ 正在AEM載入擴展的主機。
+ 當前用戶的訪AEM問令牌。

載入擴展時最常見的檢查是：

+ 使用主AEM機(`new URLSearchParams(window.location.search).get('repo')`)以確定是否應載入擴展。
   + 僅顯示屬於特定AEM程式的環境的擴展（如下例所示）。
   + 僅顯示特定環境(AEM即主機)上AEM的擴展。
+ 使用 [Adobe I/O Runtime行動](./runtime-action.md) 進行HTTP調用以AEM確定當前用戶是否應看到擴展。

以下示例說明將擴展限制到程式中的所有環境 `p12345`。

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const PROGRAM_ID = 'p12345';

  // Get the current AEM Host (author-pXXX-eYYY.adobeaemcloud.com) the extension is loading on
  const aemHost = new URLSearchParams(window.location.search).get('repo');

  // Create a check to determine if the current AEM host matches the AEM program that uses this extension 
  const aemHostRegex = new RegExp(`^author-${PROGRAM_ID}-e[\\d]+\\.adobeaemcloud\\.com$`)

  // Disable the extension if the Cloud Manager Program Id doesn't match the regex.
  if (!aemHostRegex.test(aemHost)) {
    return; // Skip extension registration if the environment is not in program p12345.
  }

  // Else, continue initializing the extension
  const init = async () => { .. };
  
  init().catch(console.error);
}
```
