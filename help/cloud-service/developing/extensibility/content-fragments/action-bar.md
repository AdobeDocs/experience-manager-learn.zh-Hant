---
title: 內AEM容片段控制台操作欄擴展
description: 瞭解如何建立內AEM容片段控制台操作欄擴展。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
exl-id: 97d26a1f-f9a7-4e57-a5ef-8bb2f3611088
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 0%

---

# 操作欄擴展

![操作欄擴展](./assets/action-bar/action-bar.png){align="center"}

包含操作欄的擴展，將按鈕引入內容片段控制台AEM的操作，該操作在 __1或更多__ 已選擇內容片段。 因為操作欄擴展按鈕僅在至少選擇一個內容片段時才顯示，所以它們通常對所選內容片段執行操作。 示例包括：

+ 在所選內容片段上調用業務流程或工作流。
+ 更新或更改所選內容片段的資料。

## 分機註冊

`ExtensionRegistration.js` 是擴展的入口AEM點，並定義：

1. 擴展類型；中。
1. 擴展按鈕的定義，在 `getButton()` 的子菜單。
1. 在 `onClick()` 的子菜單。

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

內AEM容片段控制台操作欄擴展可能需要：

+ 用戶執行所需動作的附加輸入。
+ 提供有關操作結果的用戶詳細資訊的能力。

為支援這些要求，內AEM容片段控制台擴展允許以React應用程式形式呈現的自定義模式。

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
      <p class="has-text-weight-bold is-size-36 is-size-27-touch is-margin-bottom-big has-text-blackest">跳至建立模式</p>
      <p class="has-text-blackest">瞭解如何建立按一下操作欄擴展按鈕時顯示的模式。</p>
      <div class="has-align-start is-margin-top-big">
        <a href="./modal.md" target="_blank" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
          <span class="spectrum-Button-label has-no-wrap has-text-weight-bold" title="學習構建模式">學習構建模式</span>
        </a>
      </div>
    </div>
  </div>
</div>

## 無模式

有時，AEM Content Fragment控制台操作欄擴展不需要與用戶進行進一步交互，例如：

+ 調用不需要用戶輸入的後端進程，如導入或導出。

在這些情況下，AEM內容片段控制台擴展不需要 [模態](#modal)，並直接在操作欄按鈕的 `onClick` 處理程式。

內AEM容片段控制台擴展允許進度指示符在執行工作AEM時覆蓋內容片段控制台，從而阻止用戶執行進一步的操作。 進度指示器的使用是可選的，但對於將同步工作的進度告知用戶非常有用。

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
