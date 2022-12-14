---
title: AEM內容片段主控台擴充功能註冊
description: 了解如何註冊內容片段主控台擴充功能。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
source-git-commit: f19cdc7d551f20b35550e7d25bd168a2eaa43b6a
workflow-type: tm+mt
source-wordcount: '569'
ht-degree: 0%

---


# 擴充功能註冊

AEM內容片段主控台擴充功能是根據React而專用的App Builder應用程式，會使用 [React Spectrum](https://react-spectrum.adobe.com/react-spectrum/) UI架構。

若要定義擴充功能的AEM內容片段主控台的顯示位置和方式，在擴充功能的App Builder應用程式中需要兩項特定設定：應用程式路由和擴充功能註冊。

## 應用程式路由{#app-routes}

擴充功能 `App.js` 聲明 [React路由器](https://reactrouter.com/en/main) 包含在AEM內容片段控制台中註冊擴充功能的索引路由。

當AEM內容片段控制台初次載入時，會叫用索引路由，此路由的目標將定義在控制台中如何公開擴充功能。

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

## 擴充功能註冊

`ExtensionRegistration.js` 必須透過擴充功能的索引路徑立即載入，並執行擴充功能的註冊點，定義：

1. 擴充功能類型；a [標題功能表](./header-menu.md) 或 [動作列](./action-bar.md) 按鈕。
   + [標題功能表](./header-menu.md#extension-registration) 擴充功能會以 `headerMenu` 屬性 `methods`.
   + [動作列](./action-bar.md#extension-registration) 擴充功能會以 `actionBar` 屬性 `methods`.
1. 擴充功能按鈕的定義，位於 `getButton()` 函式。 此函式會傳回包含欄位的物件：
   + `id` 是按鈕的唯一ID
   + `label` 是AEM內容片段控制台中擴充功能按鈕的標籤
   + `icon` 是AEM內容片段控制台中擴充功能按鈕的圖示。 圖示是 [React Spectrum](https://spectrum.adobe.com/page/icons/) 表徵圖名稱，並刪除空格。
1. 按鈕的點按處理常式，定義於 `onClick()` 函式。
   + [標題功能表](./header-menu.md#extension-registration) 擴充功能不會將參數傳遞至點按處理常式。
   + [動作列](./action-bar.md#extension-registration) 擴充功能提供 `selections` 參數。

### 標題功能表擴充功能

![標題功能表擴充功能](./assets/extension-registration/header-menu.png)

未選取內容片段時，會顯示「標題功能表」擴充按鈕。 因為標題功能表擴充功能不會對內容片段選取項目產生任何作用，因此不會提供任何內容片段給其 `onClick()` 處理常式。

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
      <p class="has-text-weight-bold is-size-36 is-size-27-touch is-margin-bottom-big has-text-blackest">跳到生成標題菜單擴展</p>
      <p class="has-text-blackest">了解如何在AEM內容片段主控台中註冊和定義標題功能表擴充功能。</p>
      <div class="has-align-start is-margin-top-big">
        <a href="./header-menu.md" target="_blank" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
          <span class="spectrum-Button-label has-no-wrap has-text-weight-bold" title="了解如何建立頁首功能表擴充功能">了解如何建立頁首功能表擴充功能</span>
        </a>
      </div>
    </div>
  </div>
</div>

### 動作列擴充功能

![動作列擴充功能](./assets/extension-registration/action-bar.png)

選取一或多個內容片段時，動作列延伸模組按鈕會隨即顯示。 所選內容片段的路徑可透過 `selections` 參數，在按鈕的 `onClick(..)` 處理常式。

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
      <p class="has-text-weight-bold is-size-36 is-size-27-touch is-margin-bottom-big has-text-blackest">跳至建立動作列延伸模組</p>
      <p class="has-text-blackest">了解如何在AEM內容片段主控台中註冊和定義動作列擴充功能。</p>
      <div class="has-align-start is-margin-top-big">
        <a href="./action-bar.md" target="_blank" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
          <span class="spectrum-Button-label has-no-wrap has-text-weight-bold" title="了解如何建立動作列擴充功能">了解如何建立動作列擴充功能</span>
        </a>
      </div>
    </div>
  </div>
</div>

## 有條件地包含擴充功能

AEM內容片段控制台擴充功能可執行自訂邏輯，以在擴充功能出現在AEM內容片段控制台時限制。 此檢查會在 `register` 呼叫 `ExtensionRegistration` 元件，若不應顯示擴充功能，則會立即傳回。

此檢查的上下文有限：

+ 擴充功能載入所在的AEM主機。
+ 目前使用者的AEM存取權杖。

載入擴充功能最常見的檢查為：

+ 使用AEM主機(`new URLSearchParams(window.location.search).get('repo')`)，以判斷擴充功能是否應載入。
   + 只顯示屬於特定程式一部分之AEM環境上的擴充功能（如以下範例所示）。
   + 僅顯示特定AEM環境(即AEM主機)上的擴充功能。
+ 使用 [Adobe I/O Runtime行動](./runtime-action.md) 向AEM發出HTTP呼叫，以判斷目前的使用者是否應該看到擴充功能。

以下範例說明將擴充功能限制在程式中的所有環境 `p12345`.

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
