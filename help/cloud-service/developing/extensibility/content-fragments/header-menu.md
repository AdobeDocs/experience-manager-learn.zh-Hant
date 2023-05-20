---
title: 內AEM容片段控制台標題菜單擴展
description: 瞭解如何建立內AEM容片段控制台標題菜單擴展。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
exl-id: 92d6e98e-24d0-4229-9d30-850f6b72ab43
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '366'
ht-degree: 0%

---

# 標題菜單擴展

![標題菜單擴展](./assets/header-menu/header-menu.png){align="center"}

包含標題菜單的擴展，將按鈕引入內容片段控制台AEM的標題，在 __不__ 已選擇內容片段。 因為標題菜單擴展按鈕僅在未選擇內容片段時才顯示，所以它們通常不會對現有內容片段執行任何操作。 通常，標題菜單擴展：

+ 使用自定義邏輯（如建立一組通過內容引用連結的內容片段）建立新的內容片段。
+ 對以寫程式方式選擇的一組內容片段執行操作，例如導出上週建立的所有內容片段。

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
      id: extensionId, // Must be unique
      methods: {
        // Configure your Header Menu button here
        headerMenu: {
          getButton() {
            return {
              'id': 'example.my-header-menu-button',    // Unique ID for the button
              'label': 'My header menu button',         // Button label 
              'icon': 'Bookmark'                        // Button icon; get name from: https://spectrum.adobe.com/page/icons/ (Remove spaces, keep uppercase)
            }
          },

          // Click handler for the Header Menu extension button
          onClick() {
            // Header Menu buttons are not associated with selected Content Fragment, and thus are not provided a selection parameter.        
            // Do work like importing data from a well known location, or exporting a welll known set of data
            doWork();            
          },
        }
      }
    }
  }
  init().catch(console.error);
}
```

## 模型

![模型](./assets/modal/modal.png)

內AEM容片段控制台標題菜單擴展可能需要：

+ 用戶執行所需動作的附加輸入。
+ 提供有關操作結果的用戶詳細資訊的能力。

為支援這些要求，內AEM容片段控制台擴展允許以React應用程式形式呈現的自定義模式。

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  ...
  onClick() {
    // Create a URL that maps to the React route to be rendered in the modal
    const modalURL = "/index.html#/content-fragment/my-extension";

    // Open the modal and display the React route created above
    guestConnection.host.modal.showUrl({
      title: "My modal title",
      url: modalURL
    })     
  }
  ...     
}...
```

<div class="column is-8-desktop is-full-mobile is-half-tablet" style="
    border: solid 1px #ccc;
    border-radius: 10px;
    margin: 4rem auto;
">
  <div class="is-flex is-padded-small is-padded-big-mobile">
    <div>
      <p class="has-text-weight-bold is-size-36 is-size-27-touch is-margin-bottom-big has-text-blackest">跳至建立模式</p>
      <p class="has-text-blackest">瞭解如何建立按一下標題菜單擴展按鈕時顯示的模式。</p>
      <div class="has-align-start is-margin-top-big">
        <a href="./modal.md" target="_blank" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
          <span class="spectrum-Button-label has-no-wrap has-text-weight-bold" title="學習構建模式">學習構建模式</span>
        </a>
      </div>
    </div>
  </div>
</div>

## 無模式

有時，AEM Content Fragment控制台標題菜單擴展不需要與用戶進行進一步交互，例如：

+ 調用不需要用戶輸入的後端進程，如導入或導出。
+ 開啟新網頁，如內容指南的內部文檔。

在這些情況下，AEM內容片段控制台擴展不需要 [模態](#modal)，並可以直接在標題菜單按鈕的 `onClick` 處理程式。

內AEM容片段控制台擴展允許進度指示器在執行工作時覆蓋AEM內容片段控制台，從而阻止用戶執行進一步的操作。 進度指示器的使用是可選的，但對於將同步工作的進度告知用戶非常有用。

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  guestConnection: { ...
    methods: { ...
      headerMenu: { ...
        onClick() {
          // Optionally, show the progress indicator overlay on the AEM Content Fragment console
          guestConnection.host.progressCircle.start();
          // Perform work on the selected Content Fragments
          doWork();
          // Hide the progress indicator overlay on the AEM Content Fragment console when the work is done
          guestConnection.host.progressCircle.stop();
        }
      }
    }
  }
}
```
