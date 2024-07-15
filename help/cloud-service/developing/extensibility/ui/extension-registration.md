---
title: AEM UI擴充功能註冊
description: 瞭解如何註冊AEM UI擴充功能。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
jira: KT-11603
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: ef2290d9-ba40-429b-b10d-e82d6c1c20f6
duration: 85
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '299'
ht-degree: 0%

---

# 擴充功能註冊

AEM UI擴充功能是專門的App Builder應用程式，以React為基礎，並使用[React Spectrum](https://react-spectrum.adobe.com/react-spectrum/) UI架構。

若要定義AEM UI擴充功能的顯示位置及方式，擴充功能的App Builder應用程式中需要兩項設定：應用程式路由和擴充功能註冊。

## 應用程式路由{#app-routes}

擴充功能的`App.js`會宣告[React路由器](https://reactrouter.com/en/main)，其中包含在AEM UI中註冊擴充功能的索引路由。

索引路由會在AEM UI初次載入時叫用，而此路由的目標會定義擴充功能在主控台中的公開方式。

+ `./src/aem-ui-extension/web-src/src/components/App.js`

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

`ExtensionRegistration.js`必須透過擴充功能的索引路徑立即載入，並做為擴充功能的註冊點。

根據在[初始化App Builder應用程式擴充功能](./app-initialization.md)時選取的AEM UI擴充功能範本，支援不同的擴充功能點。

+ [內容片段UI擴充功能點](./content-fragments/overview.md#extension-points)

## 有條件地包含擴充功能

AEM UI擴充功能可執行自訂邏輯，以限制擴充功能出現的AEM環境。 這項檢查是在`ExtensionRegistration`元件中的`register`呼叫之前執行，如果不應該顯示副檔名，則會立即傳回。

此檢查可用的內容有限：

+ 正在載入擴充功能的AEM主機。
+ 目前使用者的AEM存取權杖。

載入擴充功能的最常見檢查為：

+ 使用AEM主機(`new URLSearchParams(window.location.search).get('repo')`)來決定是否應載入擴充功能。
   + 只顯示屬於特定計劃一部分的AEM環境上的擴充功能（如以下範例所示）。
   + 僅顯示特定AEM環境(AEM主機)上的擴充功能。
+ 使用[Adobe I/O Runtime動作](./runtime-action.md)對AEM發出HTTP呼叫，以判斷目前的使用者是否應該看到擴充功能。

以下範例說明將擴充功能限制在程式`p12345`中的所有環境。

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
