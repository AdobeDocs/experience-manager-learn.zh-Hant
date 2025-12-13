---
title: 新增自訂按鈕到RTF編輯器(RTE)工具列
description: 瞭解如何在AEM內容片段編輯器中將自訂按鈕新增到RTF編輯器(RTE)工具列
feature: Developer Tools, Content Fragments
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13464
thumbnail: KT-13464.jpg
doc-type: article
last-substantial-update: 2023-06-12T00:00:00Z
exl-id: 6fd93d3b-6d56-43c5-86e6-2e2685deecc9
duration: 345
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '406'
ht-degree: 0%

---

# 新增自訂按鈕到RTF編輯器(RTE)工具列

瞭解如何在AEM內容片段編輯器中將自訂按鈕新增到RTF編輯器(RTE)工具列。

>[!VIDEO](https://video.tv.adobe.com/v/3420768?quality=12&learn=on)

可以使用&#x200B;**擴充點將自訂按鈕新增到內容片段編輯器中的** RTE工具列`rte`。 這個範例說明如何新增名為&#x200B;_新增提示_&#x200B;的自訂按鈕到RTE工具列並修改RTE中的內容。

使用`rte`擴充點的`getCustomButtons()`方法，可以將一或多個自訂按鈕新增至&#x200B;**RTE工具列**。 也可以分別使用&#x200B;_和_&#x200B;方法新增或移除標準RTE按鈕，例如`getCoreButtons()`複製、貼上、粗體和斜體`removeButtons)`。

此範例顯示如何使用自訂&#x200B;_新增提示_&#x200B;工具列按鈕插入醒目提示的筆記或提示。 反白顯示的附註或提示內容透過HTML元素和相關聯的CSS類別套用特殊格式。 使用`onClick()`的`getCustomButtons()`回呼方法插入預留位置內容和HTML程式碼。

## 擴充點

此範例會延伸至擴充點`rte`，以將自訂按鈕新增至內容片段編輯器中的RTE工具列。

| AEM UI已擴充 | 擴充點 |
| ------------------------ | --------------------- |
| [內容片段編輯器](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [ RTF編輯器工具列](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-toolbar/) |

## 範例擴充功能

下列範例會在RTE工具列中建立&#x200B;_新增提示_&#x200B;自訂按鈕。 按一下動作會將預留位置文字插入RTE中的目前插入號位置。

程式碼會顯示如何使用圖示新增自訂按鈕並註冊點選處理常式函式。

### 擴充功能註冊

`ExtensionRegistration.js`對應至index.html路由，是AEM擴充功能的進入點，並定義：

+ `getCustomButtons()`函式中RTE工具列按鈕的定義具有`id, tooltip and icon`屬性。
+ `onClick()`函式中按鈕的點選處理常式。
+ 點按處理常式函式會接收`state`物件做為引數，以取得HTML或文字格式的RTE內容。 但在此範例中，並未使用它。
+ 點按處理常式函式會傳回指示陣列。 此陣列具有具有`type`和`value`屬性的物件。 若要插入內容，`value`屬性HTML程式碼片段，`type`屬性使用`insertContent`。 如果有取代內容的使用案例，則使用案例`replaceContent`指示型別。

`insertContent`值是HTML字串`<div class=\"cmp-contentfragment__element-tip\"><div>TIP</div><div>Add your tip text here...</div></div>`。 用來顯示值的CSS類別`cmp-contentfragment__element-tip`未在Widget中定義，而是實作於顯示此內容片段欄位的Web體驗。


`src/aem-cf-editor-1/web-src/src/components/ExtensionRegistration.js`

```javascript
import React from "react";
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
          getCustomButtons: () => [
            {
              id: "wknd-cf-tip", // Provide a unique ID for the custom button
              tooltip: "Add Tip", // Provide a label for the custom button
              icon: "Note", // Provide an icon for the button (see https://spectrum.adobe.com/page/icons/ for a list of available icons)
              onClick: (state) => {
                // Provide a click handler function that returns the instructions array with type and value. This example inserts the HTML snippet for TIP content.
                return [
                  {
                    type: "insertContent",
                    value:
                      '<div class="cmp-contentfragment__element-tip"><div>TIP</div><div>Add your tip text here...</div></div>',
                  },
                ];
              },
            },
          ],
        },
      },
    });
  };

  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}

export default ExtensionRegistration;
```
