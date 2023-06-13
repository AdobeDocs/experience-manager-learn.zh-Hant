---
title: 新增自訂按鈕到RTF編輯器(RTE)工具列
description: 瞭解如何在AEM內容片段編輯器中將自訂按鈕新增到RTF編輯器(RTE)工具列
feature: Developer Tools, Content Fragments
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13464
thumbnail: KT-13464.jpg
doc-type: article
last-substantial-update: 2023-06-12T00:00:00Z
source-git-commit: c54d078c6282f8ace936dd4a9ee0d5cc39490230
workflow-type: tm+mt
source-wordcount: '405'
ht-degree: 0%

---


# 新增自訂按鈕到RTF編輯器(RTE)工具列

![內容片段編輯器工具列擴充功能範例](./assets/rte-toolbar/hero.png){align="center"}

自訂按鈕可新增至 **rte工具列** 在內容片段編輯器中使用 `rte` 延伸點。 此範例顯示如何新增自訂按鈕，稱為 _新增秘訣_ 到RTE工具列，並修改RTE中的內容。

使用 `rte` 擴充點的 `getCustomButtons()` 方法可將一或多個自訂按鈕新增至 **rte工具列**. 您也可以新增或移除標準RTE按鈕，例如 _複製、貼上、粗體和斜體_ 使用 `getCoreButtons()` 和 `removeButtons)` 方法。

此範例說明如何使用自訂來插入醒目提示或筆尖 _新增秘訣_ 工具列按鈕。 反白顯示的附註或提示內容透過HTML元素和相關聯的CSS類別套用特殊的格式設定。 預留位置內容和HTML程式碼是使用 `onClick()` 的回呼方法 `getCustomButtons()`.

## 擴充點

此範例延伸至擴充點 `rte` 以新增自訂按鈕到內容片段編輯器中的RTE工具列。

| AEM UI延伸 | 擴充點 |
| ------------------------ | --------------------- | 
| [內容片段編輯器](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [RTF編輯器工具列](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-toolbar/) |

## 範例擴充功能

下列範例會建立 _新增秘訣_ RTE工具列中的自訂按鈕。 按一下動作會將預留位置文字插入RTE中的目前插入號位置。

程式碼會顯示如何使用圖示新增自訂按鈕並註冊點選處理常式函式。

### 擴充功能註冊

`ExtensionRegistration.js`，對應至index.html路由，是AEM擴充功能的入口點，並定義：

+ RTE工具列按鈕在中的定義 `getCustomButtons()` 函式為 `id, tooltip and icon` 屬性。
+ 按鈕的點選處理常式，位於 `onClick()` 函式。
+ 點按處理常式函式會接收 `state` 物件作為引數，以取得HTML或文字格式的RTE內容。 但在此範例中，並未使用。
+ 點按處理常式函式會傳回指示陣列。 此陣列有一個物件，具有 `type` 和 `value` 屬性。 若要插入內容，請 `value` 屬性HTML程式碼片段、 `type` 屬性使用 `insertContent`. 如果有使用案例來取代內容，使用案例 `replaceContent` 指示型別。

此 `insertContent` 值是HTML字串， `<div class=\"cmp-contentfragment__element-tip\"><div>TIP</div><div>Add your tip text here...</div></div>`. CSS類別 `cmp-contentfragment__element-tip` 用於顯示值並未在Widget中定義，而是實作於顯示此內容片段欄位的網頁體驗。


`src/aem-cf-editor-1/web-src/src/components/ExtensionRegistration.js`

```javascript
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

          // RTE Toolbar custom button
          getCustomButtons: () => ([
            {
              id: "wknd-cf-tip",       // Provide a unique ID for the custom button
              tooltip: "Add Tip",      // Provide a label for the custom button
              icon: 'Note',            // Provide an icon for the button (see https://spectrum.adobe.com/page/icons/ for a list of available icons)
              onClick: (state) => {    // Provide a click handler function that returns the instructions array with type and value. This example inserts the HTML snippet for TIP content.
                return [{
                  type: "insertContent",
                  value: "<div class=\"cmp-contentfragment__element-tip\"><div>TIP</div><div>Add your tip text here...</div></div>"
                }];
              },
            },
          ]),
      }
    });
  };
  
  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}
```
