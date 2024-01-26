---
title: 將內容片段匯出至XML
description: 瞭解如何從AEM內容片段編輯器匯出內容片段
feature: Developer Tools, Content Fragments
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13309
thumbnail: KT-13309.jpg
doc-type: article
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: 70adb2c1-9e08-4a4b-b8f1-16bddd84c23d
duration: 236
source-git-commit: ba9d9fe4b3a387e0fb32bbd2edd7b2debf03f325
workflow-type: tm+mt
source-wordcount: '295'
ht-degree: 1%

---

# 將內容片段匯出至XML

![內容片段編輯器標題功能表擴充功能範例](./assets/export-to-xml/hero.png){align="center"}

自訂按鈕可以使用新增到內容片段編輯器標題選單 `headerMenu` 延伸點。 此範例說明如何新增按鈕到標題選單，以及如何處理點選事件以將作用中的內容片段匯出為XML或CSV。

頁首按鈕可以單一按鈕的形式存在，也可以作為具有子專案的按鈕存在。 此範例說明如何使用子專案實施按鈕，但包含註釋掉的程式碼以實施單一按鈕。

## 擴充點

此範例會延伸至擴充點 `headerBar` 以新增自訂按鈕到內容片段編輯器。

| AEM UI已擴充 | 擴充點 |
| ------------------------ | --------------------- | 
| [內容片段編輯器](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [頁首功能表](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/header-menu/) |

## 範例擴充功能

下列範例會建立含有兩個子專案的頁首功能表按鈕，其中一個是將作用中內容片段匯出為XML （已實作），另一個是將作用中內容片段匯出為CSV （未實作）。

此程式碼說明如何在擴充功能的註冊檔案中取得內容片段內容，以及如何匯出內容片段的JSON內容。

### 擴充功能註冊

`ExtensionRegistration.js`，對應至index.html路由，是AEM擴充功能的進入點，並定義：

+ 擴充功能按鈕的位置隨即顯示(`headerMenu`)在AEM編寫體驗中
+ getButton()函式中的擴充功能按鈕定義
+ 按鈕的點選處理常式(在onClick()函式中)或子專案清單及其點選處理常式。

`src/aem-ui-extension/web-src/src/components/ExtensionRegistration.js`

```javascript
import { Text } from "@adobe/react-spectrum";
import { register } from "@adobe/uix-guest";
import { extensionId } from "./Constants";
import { toXML } from 'jstoxml';

// This function is called when the extension is registered with the host and runs in an iframe in the Content Fragment Editor browser window.
function ExtensionRegistration() {

  const init = async () => {
    const guestConnection = await register({
      id: extensionId, // This is the unique id of this extension (you can make this up as long as its unique) .. in this case its `editor-export-to-xml` pulled out into Constants.js.
      methods: {
        headerMenu: {
          getButtons() {
            return [
              {
                id: 'export',               // Provide a unique ID for the button
                label: 'Export',            // Provide a label for the button
                variant: 'secondary',
                icon: 'OpenIn',             //  Provide an icon for the button (see https://spectrum.adobe.com/page/icons/ for a list of available icons)
                /*
                onClick: async () => {
                  // Use the onClick handler if you do NOT want sub-items and handle  a single-button click.
                  // This example uses the subItems array below to attach multiple actions to the button.
                  console.log("Clicked Export");
                },
                */
                subItems: [
                  // Sub-items are used to create a dropdown menu with multiple items for the header menu button.
                  // Do not include the subItems property if you do not want sub-items.
                  {
                      id: 'xml',                  // Provide a unique ID for the sub-item
                      label: 'XML',               // Provide a label for the sub-item
                      onClick: async () => {      // Provide a click handler for the sub-item  
                          // Get the Content Fragment JSON representation
                          const contentFragment = await guestConnection.host.contentFragment.getContentFragment();
                          console.log(contentFragment);
                          // Use a library like jstoxml to convert the JSON to XML
                          const xml = toXML({ contentFragment: contentFragment.main }, { header: true, indent: '  ' });
                          // Get the download file name from the Content Fragment's node name (last segment of the path)
                          const contentFragmentName = contentFragment.path.split('/').at(-1);
                          // Trigger the download of the XML contents
                          download(contentFragmentName, 'xml', xml);
                      },
                  },
                  {
                      id: 'csv',
                      label: 'CSV',
                      onClick: async () => {
                          const contentFragment = await guestConnection.host.contentFragment.getContentFragment();
                          alert("Implement your CSV export here!");
                          // This can be implemented in the same way as the XML export above, using a similar library such as https://www.npmjs.com/package/json-2-csv                          
                      },
                  },
              ],
              },
            ];
          },
        },
      }
    });
  };
  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}

/**
 * This function triggers a download of the content fragment export.
 * 
 * Not all extensions require App Builder workers or React web apps in extension modals. 
 * Extensions like this can be fully implemented in the extension's registration file.
 * 
 * @param {*} name the filename
 * @param {*} type  the file type/extension
 * @param {*} contents the contents of the file to export
 */
function download(name, type, contents) {
  // Create a link element to trigger the download
  const a = document.createElement('a');
  // Set the filename this file should download as
  a.setAttribute('download', `${name}.${type}`);
  // Set the content fragment value as a data URL - This approach typically works for Content Fragments as their text content is usually under 2MB
  // For larger downloads, you may need to use Blob or Streams
  a.href = `data:attachment/${type},${encodeURIComponent(contents)}`;
  // Trigger the download as an attachment
  a.click();
}

export default ExtensionRegistration;
```

#### 內容片段資料

可使用以下專案擷取使用中的內容片段 `getContentFragment()` 上的方法 `guestConnection.host.contentFragment` 物件。

```javascript
const contentFragment = await guestConnection.host.contentFragment.getContentFragment();
```

此 `contentFragment` 物件包含有關內容片段的所有資訊，包括路徑、模型、中繼資料、主要內容及任何變體。

```json
{
    "path": "/content/dam/wknd-shared/en/adventures/climbing-new-zealand/climbing-new-zealand",
    "etag": "\"2f18b858f4896ec1d7c41ba7a16bed7d\"",
    "model": {
        "path": "/conf/wknd-shared/settings/dam/cfm/models/adventure",
        "title": "Adventure"
    },
    "fragmentId": "eb0175d3-b011-421b-b1be-0ef5b2b64518",
    "metadata": {
        "title": "Climbing New Zealand",
        "description": "",
        "createdBy": "janedoe@example.com",
        "createdDate": "2022-09-09T18:05:35.453Z",
        "modifiedBy": "janedoe@example.com",
        "modifiedDate": "2023-06-01T14:46:56.394Z",
        "publishedBy": "",
        "publishedDate": "",
        "status": "DRAFT"
    },
    "main": {
        "title": "Climbing New Zealand",
        "slug": "climbing-new-zealand",
        "description": {
            "contentType": "text/html",
            "value": "<h2><b>Let us take you on a spectacular climbing experience unique to New Zealand</b></h2>\n<p>Feel the raw adventure and excitement of our guided rock climbing experience. Reach new heights under our professional instruction and feel your body and mind work together in harmony.&nbsp;Come join us for a guided rock climbing adventure in the mountains that trained Sir Edmund Hilary. Whether it is your first time thinking of putting on climbing shoes or you are an old hand looking for some new challenges, our guides can make your climbing adventure a trip you won't soon forget. New Zealand has countless climbing routes to choose from and is known as one of the premiere climbing destinations in the world. With so many different routes and areas to choose from our guides can tailor each trip to your exact specifications. Let us help you make your New Zealand climbing vacation a memory you will cherish forever!</p>\n<p>Related trips:</p>\n<ul>\n<li><a href=\"../../../../content/dam/wknd-shared/en/adventures/colorado-rock-climbing/colorado-rock-climbing\">Colorado Rock Climbing</a></li>\n<li><a href=\"../../../../content/dam/wknd-shared/en/adventures/yosemite-backpacking/yosemite-backpacking\">Yosemite Backpacking</a>&nbsp;</li>\n</ul>"
        },
        "adventureType": "Overnight Trip",
        "tripLength": "2 Days",
        "activity": "Rock Climbing",
        "groupSize": 3,
        "difficulty": "Intermediate",
        "price": 900,
        "primaryImage": {
            "path": "/content/dam/wknd-shared/en/adventures/climbing-new-zealand/adobestock-140634652.jpeg",
            "name": "adobestock-140634652.jpeg",
            "title": "Rock Climbing and Bouldering above the lake and mountains",
            "size": 688150,
            "mimeType": "image/jpeg",
            "width": 1293,
            "height": 862,
            "id": "7f2a2314-7701-4c27-8945-bd5388ac94da",
            "created": {
                "at": "2022-09-09T18:05:37.383Z",
                "by": "janedoe@example.com"
            },
            "status": "MODIFIED"
        },
        "itinerary": {
            "contentType": "text/html",
            "value": "<h2><b>Day 1 - It's climb time</b></h2>\n<p>We depart and end from a central meeting spot in the town of Fauchere.&nbsp; From there we'll be driving 30-50 minutes to the climbing site. On our way there, we'll go over important safety and climbing procedures. After arriving at trailhead, we will confirm all safety and personal climbing equipment (shoes, harness, helmets, belay devises, and more). Our hike to the climbing area will be between 5 minutes to an hour based on where we decide to climb and the desires of the group. On the rock, we will spend about 6 hours climbing.&nbsp; We'll go over different bouldering and climbing techniques first, fundamentals of setting anchors, belay systems and hardware,. Then, we'll make our way to the top.&nbsp; Once we get there, you'll find the view to be spectactular and the sense of accomplishment even better.&nbsp; But as they say, what goes up must come down. We now face another challenge - rappelling back down. But after a day of learning and applying our skills, we will be very comfortable on the mountain and with our abilities.</p>\n<h2><b>Day 2 - Up the world's highest waterfall climb</b></h2>\n<p>Now it's time to satiate your inner-adrenaline junkie.&nbsp; We're going up behind a 60m waterfall to the top of Wanaka. You'll be pushed to reach the highest point, both mentally and physically, with overhangs to get you there. Enjoy the incredible views of surrounding lakes and mountains once you reach summit.&nbsp;</p>"
        },
        "gearList": {
            "contentType": "text/html",
            "value": "<p>We will provide all technical equipment for the course including rock shoes.</p>\n<p>Please bring appropriate clothing for the weather on each of the days, waterproofs, a warm layer, drinks and lunch for the day.&nbsp;</p>"
        }
    },
    "tags": [],
    "variations": {
        "mobile": {
            "title": "Mobile",
            "description": "This is the Content Fragment variation for use on Mobile devices.",
            "elements": {
                "title": "Climbing NZ",
                "slug": "climbing-new-zealand",
                "description": {
                    "contentType": "text/html",
                    "value": "<h2><b>Let us take you on a spectacular climbing experience unique to New Zealand</b></h2>\n<p>Feel the raw adventure and excitement of our guided rock climbing experience. Reach new heights under our professional instruction and feel your body and mind work together in harmony.&nbsp;Come join us for a guided rock climbing adventure in the mountains that trained Sir Edmund Hilary.&nbsp;</p>"
                },
                "adventureType": "Overnight Trip",
                "tripLength": "2 Days",
                "activity": "Rock Climbing",
                "groupSize": 3,
                "difficulty": "Intermediate",
                "price": 900,
                "primaryImage": {
                    "path": "/content/dam/wknd-shared/en/adventures/climbing-new-zealand/adobestock-140634652.jpeg",
                    "name": "adobestock-140634652.jpeg",
                    "title": "Rock Climbing and Bouldering above the lake and mountains",
                    "size": 688150,
                    "mimeType": "image/jpeg",
                    "width": 1293,
                    "height": 862,
                    "id": "7f2a2314-7701-4c27-8945-bd5388ac94da",
                    "created": {
                        "at": "2022-09-09T18:05:37.383Z",
                        "by": "janedoe@example.com"
                    },
                    "status": "MODIFIED"
                },
                "itinerary": {
                    "contentType": "text/html",
                    "value": "<h2><b>Day 1 - It's climb time</b></h2>\n<p>We depart and end from a central meeting spot in the town of Fauchere.&nbsp; From there we'll be driving 30-50 minutes to the climbing site. On our way there, we'll go over important safety and climbing procedures. After arriving at trailhead, we will confirm all safety and personal climbing equipment (shoes, harness, helmets, belay devises, and more). Our hike to the climbing area will be between 5 minutes to an hour based on where we decide to climb and the desires of the group. On the rock, we will spend about 6 hours climbing.&nbsp; We'll go over different bouldering and climbing techniques first, fundamentals of setting anchors, belay systems and hardware,. Then, we'll make our way to the top.&nbsp; Once we get there, you'll find the view to be spectactular and the sense of accomplishment even better.&nbsp; But as they say, what goes up must come down. We now face another challenge - rappelling back down. But after a day of learning and applying our skills, we will be very comfortable on the mountain and with our abilities.</p>\n<h2><b>Day 2 - Up the world's highest waterfall climb</b></h2>\n<p>Now it's time to satiate your inner-adrenaline junkie.&nbsp; We're going up behind a 60m waterfall to the top of Wanaka. You'll be pushed to reach the highest point, both mentally and physically, with overhangs to get you there. Enjoy the incredible views of surrounding lakes and mountains once you reach summit.&nbsp;</p>\n"
                },
                "gearList": {
                    "contentType": "text/html",
                    "value": "<p>We will provide all technical equipment for the course including rock shoes.</p>\n<p>Please bring appropriate clothing for the weather on each of the days, waterproofs, a warm layer, drinks and lunch for the day.&nbsp;</p>\n"
                }
            },
            "tags": []
        }
    },
    "translations": {
        "data": {
            "locale": "en",
            "languageCopies": [
                {
                    "path": "/content/dam/wknd-shared/en/adventures/climbing-new-zealand/climbing-new-zealand",
                    "title": "Climbing New Zealand",
                    "locale": "en",
                    "model": "Adventure",
                    "status": "DRAFT"
                }
            ]
        },
        "error": null
    }
}
```
