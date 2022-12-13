---
title: AEM內容片段主控台動作列擴充功能
description: 了解如何建立AEM內容片段主控台動作列擴充功能。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
source-git-commit: a7b32982b547eb292384d2ebde80ba745091702a
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 0%

---


# 動作列延伸模組

![動作列延伸模組](./assets/action-bar/action-bar.png){align="center"}

包含動作列的擴充功能，會為AEM內容片段控制台的動作導入按鈕，此按鈕會在 __1或更多__ 已選取內容片段。 因為動作列延伸按鈕只有在選取至少一個內容片段時才會顯示，所以它們通常會對選取的內容片段採取行動。 範例包括：

+ 在選取的內容片段上叫用業務流程或工作流程。
+ 更新或變更所選內容片段的資料。

## 擴充功能註冊

`ExtensionRegistration.js` 是AEM擴充功能的進入點，並定義：

1. 擴充功能類型；在中，會顯示動作列按鈕。
1. 擴充功能按鈕的定義，位於 `getButton()` 函式。
1. 按鈕的點擊處理常式，位於 `onClick()` 函式。

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

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
              'id': 'example.my-action-bar-button',     // Unique ID for the button
              'label': 'My action bar button',          // Button label 
              'icon': 'Edit'                            // Button icon; get name from: https://spectrum.adobe.com/page/icons/ (Remove spaces, keep uppercase)
            }
          },
          // Click handler for the Action Bar extension button
          onClick(selections) {
            // Action Bar buttons require the selection of at least 1 Content Fragment, 
            // so we can assume the extension will do something with these selections

            // Collect the selected content fragment paths from the selections parameter
            const selectionIds = selections.map(selection => selection.id);
            
            // Do some work with the selected content fragments
            doWork(selectionIds);          
        }
      }
    })
  }
  init().catch(console.error)
```

## 模型

![模型](./assets/modal/modal.png)

AEM內容片段控制台動作列擴充功能可能需要：

+ 來自用戶的額外輸入，以執行所需的操作。
+ 提供使用者動作結果的詳細資訊的功能。

為支援這些需求，AEM內容片段主控台擴充功能允許自訂強制回應視窗，以React應用程式的形式呈現。

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  ...
  onClick(selections) {
    // Collect the selected content fragment paths 
    const contentFragmentPaths = selections.map(selection => selection.id);

    // Create a URL that maps to the React route to be rendered in the modal 
    const modalURL = "/index.html#" + generatePath(
      "/content-fragment/:selection/my-extension",
      {
        // Set the :selection React route parameter to an encoded, delimited list of paths of the selected content fragments
        selection: encodeURIComponent(contentFragmentPaths.join('|'))
      }
    );

    // Open the modal and display the React route created above
    guestConnection.host.modal.showUrl({
      title: "My modal title",
      url: modalURL
    })     
  } ...     
} ...
```

<div class="column is-8-desktop is-full-mobile is-half-tablet" style="
    border: solid 1px #ccc;
    border-radius: 10px;
    margin: 4rem auto;
">
  <div class="is-flex is-padded-small is-padded-big-mobile">
    <div>
      <p class="has-text-weight-bold is-size-36 is-size-27-touch is-margin-bottom-big has-text-blackest">跳到建立強制回應視窗</p>
      <p class="has-text-blackest">了解如何建立強制回應視窗，以便按一下動作列擴充功能按鈕時顯示。</p>
      <div class="has-align-start is-margin-top-big">
        <a href="./modal.md" target="_blank" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
          <span class="spectrum-Button-label has-no-wrap has-text-weight-bold" title="了解如何建立強制回應視窗">了解如何建立強制回應視窗</span>
        </a>
      </div>
    </div>
  </div>
</div>

## 無強制回應

AEM內容片段控制台動作列擴充功能有時不需要與使用者進一步互動，例如：

+ 叫用不需要使用者輸入的後端程式，例如匯入或匯出。

在這些情況下，AEM內容片段主控台擴充功能不需要 [模態](#modal)，並直接在動作列按鈕的 `onClick` 處理常式。

AEM內容片段控制台擴充功能允許進度指標在執行工作時覆蓋AEM內容片段控制台，使使用者無法執行進一步動作。 進度指示器的使用是可選的，但對於將同步工作的進度傳達給用戶非常有用。

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  guestConnection: { ...
    methods: { ...
      actionBar: { ...
        onClick(selections) {
          // Collect the selected content fragment paths 
          const contentFragmentPaths = selections.map(selection => selection.id);

          // Optionally, show the progress indicator overlay on the AEM Content Fragment console
          guestConnection.host.progressCircle.start();
          // Perform work on the selected Content Fragments
          doWork(contentFragmentPaths);
          // Hide the progress indicator overlay on the AEM Content Fragment console when the work is done
          guestConnection.host.progressCircle.stop();
        }
      }
    }
  }
}
```
