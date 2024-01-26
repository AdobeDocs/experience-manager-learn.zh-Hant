---
title: 內容片段控制檯中的自訂格線欄
description: 瞭解如何將自訂格線欄新增到內容片段主控台。
feature: Developer Tools, Content Fragments
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13453
thumbnail: KT-13453.jpeg
doc-type: article
last-substantial-update: 2023-06-07T00:00:00Z
exl-id: 87143cf9-e932-4ad6-afe2-cce093c520f4
duration: 182
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '406'
ht-degree: 0%

---

# 自訂格線欄

![內容片段控制檯自訂格線欄](./assets/custom-grid-columns/hero.png){align="center"}

可以使用將自訂格線欄新增到內容片段控制檯  `contentFragmentGrid` 延伸點。 此範例說明如何新增自訂欄，以人類可讀的格式，根據上次修改日期顯示內容片段年齡。

## 擴充點

此範例會延伸至擴充點 `contentFragmentGrid` 以新增自訂欄到內容片段控制檯。

| AEM UI已擴充 | 擴充點 |
| ------------------------ | --------------------- | 
| [內容片段主控台](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/) | [格線欄](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/grid-columns/) |

## 範例擴充功能

以下範例會建立自訂欄， `Age` 以人類可讀的格式顯示內容片段的年齡。 年齡是從內容片段的最後修改日期開始計算的。

此程式碼說明如何在擴充功能的註冊檔案中取得內容片段的中繼資料，以及如何匯出內容片段的JSON內容。

此範例使用 [Luxon](https://moment.github.io/luxon/) 用於計算內容片段年齡的程式庫，透過安裝 `npm i luxon`.

### 擴充功能註冊

`ExtensionRegistration.js`，對應至index.html路由，是AEM擴充功能的進入點，並定義：

+ 擴充功能插入本身的位置(`contentFragmentGrid`)在AEM編寫體驗中
+ 自訂欄的定義，在 `getColumns()` 函式
+ 每個自訂欄的值（依列）

```javascript
import { generatePath } from "react-router";
import { Text } from "@adobe/react-spectrum";
import { register } from "@adobe/uix-guest";
import { extensionId } from "./Constants";
import { Duration } from "luxon";

/**
 * Set up a in-memory cache of the custom column data.
 * This is important if the work to contain column data is expensive, such as making HTTP requests to obtain the value.
 * 
 * The caching of computed value is optional, but recommended if the work to compute the column data is expensive.
 */
const COLUMN_AGE_ID = "age";
const cache = {
  [COLUMN_AGE_ID]: {},
};

function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId,
      methods: {
        contentFragmentGrid: {
          getColumns() {
            return [
              {
                id: COLUMN_AGE_ID,          // Give the column a unique ID.
                label: "Age",               // The label to display
                render: async function (fragments) {
                  // This function is run for each row in the grid.
                  // The fragments parameter is an array of all the fragments (aka each row) in the grid.
                  const context = await guestConnection.hostConnection.getRemoteApi().getSharedContext();

                  // Iterate over each fragment in the grid
                  for (const fragment of fragments) {
                    // Check if a previous pass has computed the value for this fragment.id. If it has, we can skip it.
                    if (!cache[COLUMN_AGE_ID][fragment.id]) {
                      // If the fragment.id has not been computed and cached, then compute the value and cache it.
                      cache[COLUMN_AGE_ID][fragment.id] = await computeAgeColumnValue(fragment, context);
                    }
                  }
                  // Return the populated cache of the custom column data.
                  return cache[COLUMN_AGE_ID];
                }
              },
              // Add other custom columns here...
            ];
          },
        },
      },
    });
  };
  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}

async function computeAgeColumnValue(fragment, context) {
  // Various data is available in the sharedContext, such as the AEM host, the IMS token, and the fragment itself.
  
  // Accessing AEM APIs requires the IMS token, which is available in the sharedContext, and also supporting CORS configurations deployed to AEM Author.
  //const aemHost = context.aemHost;
  //const accessToken = context.auth.imsToken;
  
  // Get the user's locale to format the value of the column.
  const locale = context.locale;

  // Compute the value of the column, in this case we are computing the age of the fragment from its last modified date.
  const duration = Duration.fromMillis(Date.now() - fragment.modifiedDate).rescale();

  let largestUnit = {};

  if (duration.years > 0) {
    largestUnit = {years: duration.years};
  } else if (duration.months > 0) {
    largestUnit = {months: duration.months};
  } else if (duration.days > 0) {
    largestUnit = {days: duration.days};
  } else if (duration.hours > 0) {
    largestUnit = {hours: duration.hours};
  } else if (duration.minutes > 0) {
    largestUnit = {minutes: duration.minutes};
  } else if (duration.seconds > 0) {
    largestUnit = {seconds: duration.seconds};
  }

  // Convert the largest unit of the age to human readable format.
  const columnValue = Duration.fromObject(largestUnit, {locale: locale}).toHuman();

  // Return the value of the column.
  return columnValue;
}

export default ExtensionRegistration;
```

#### 內容片段資料

此 `render(..)` 中的方法 `getColumns()` 傳遞了片段的陣列。 陣列中的每個物件代表格線中的一列，並包含下列有關內容片段的中繼資料。 此中繼資料可用於格線中的熱門自訂欄。


```javascript
render: async function (fragments) {
    for (const fragment of fragments) {
        // An example value from this console log is displayed below.
        console.log(fragment);
    }
}
```

可作為元素使用的範例內容片段JSON `fragments` 中的引數 `render(..)` 方法。

```json
{
    "id": "/content/dam/wknd-shared/en/magazine/alaska-adventure/alaskan-adventures",
    "name": "alaskan-adventures",
    "title": "Alaskan Adventures",
    "status": "draft",
    "statusPreview": null,
    "model": {
        "name": "Article",
        "id": "/conf/wknd-shared/settings/dam/cfm/models/article"
    },
    "folderId": "/content/dam/wknd-shared/en/magazine/alaska-adventure",
    "folderName": "Alaska Adventure",
    "createdBy": "admin",
    "createdDate": 1684875665786,
    "modifiedBy": "admin",
    "modifiedDate": 1654104774889,
    "publishedBy": "",
    "publishedDate": 0,
    "locale": "",
    "main": true,
    "translations": {
        "locale": "en",
        "languageCopies": [
            {
                "path": "/content/dam/wknd-shared/en/magazine/alaska-adventure/alaskan-adventures",
                "title": "Alaskan Adventures",
                "locale": "en",
                "model": "Article",
                "status": "DRAFT",
                "id": "/content/dam/wknd-shared/en/magazine/alaska-adventure/alaskan-adventures"
            }
        ]
    },
    "transientStatus": {},
    "extensions": {
        "age": "1 year old"
    }
}
```

如果需要其他資料才能填入自訂欄，可向AEM Author發出HTTP請求以擷取資料。

>[!IMPORTANT]
>
> 確保AEM Author執行個體設定為允許 [跨來源請求](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/configurations/cors.html) 從原始位置，AppBuilder應用程式正在執行中。 允許的原始項包括 `https://localhost:9080`、AppBuilder階段來源和AppBuilder生產來源。
>
> 或者，擴充功能可呼叫自訂 [AppBuilder動作](../../runtime-action.md) 會代表擴充功能向AEM Author提出要求。


```javascript
const context = await guestConnection.hostConnection.getRemoteApi().getSharedContext();
...
// Fetch the Content Fragment JSON from AEM Author using the AEM Assets HTTP API
const response = await fetch(`${context.aemHost}${fragment.id.slice('/content/dam'.length)}.json`, {
    headers: {
        'Authorization': `Bearer ${context.auth.imsToken}`
    }
})
...
```

#### 欄定義

轉譯方法的結果為JavaScript物件，其索引鍵為內容片段的路徑(或 `fragment.id`)，而值為要在欄中顯示的值。

例如，此擴充功能在 `age` 欄為：

```json
{
    "/content/dam/wknd-shared/en/magazine/la-skateparks/ultimate-guide-to-la-skateparks": "22 minutes",
    "/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp": "1 hour",
    "/content/dam/wknd-shared/en/magazine/western-australia/western-australia-by-camper-van": "1 hour",
    "/content/dam/wknd-shared/en/adventures/climbing-new-zealand/climbing-new-zealand": "10 months",
    "/content/dam/wknd-shared/en/magazine/skitouring/skitouring": "1 year",
    "/content/dam/wknd-shared/en/adventures/beervana-portland/beervana-in-portland": "1 year",
    "/content/dam/wknd-shared/en/magazine/alaska-adventure/alaskan-adventures": "1 year",
    "/content/dam/wknd-shared/en/magazine/arctic-surfing/aloha-spirits-in-northern-norway": "1 year",
    "/content/dam/wknd-shared/en/magazine/san-diego-surf-spots/san-diego-surfspots": "1 year",
    "/content/dam/wknd-shared/en/magazine/fly-fishing-amazon/fly-fishing": "1 year",
    "/content/dam/wknd-shared/en/adventures/napa-wine-tasting/napa-wine-tasting": "1 year",
    "/content/dam/wknd-shared/en/adventures/cycling-southern-utah/cycling-southern-utah": "1 year",
    "/content/dam/wknd-shared/en/adventures/gastronomic-marais-tour/gastronomic-marais-tour": "1 year",
    "/content/dam/wknd-shared/en/adventures/tahoe-skiing/tahoe-skiing": "1 year",
    "/content/dam/wknd-shared/en/adventures/surf-camp-in-costa-rica/surf-camp-costa-rica": "1 year",
    "/content/dam/wknd-shared/en/adventures/yosemite-backpacking/yosemite-backpacking": "1 year",
    "/content/dam/wknd-shared/en/adventures/whistler-mountain-biking/whistler-mountain-biking": "1 year",
    "/content/dam/wknd-shared/en/adventures/colorado-rock-climbing/colorado-rock-climbing": "1 year",
    "/content/dam/wknd-shared/en/adventures/ski-touring-mont-blanc/ski-touring-mont-blanc": "1 year",
    "/content/dam/wknd-shared/en/adventures/cycling-tuscany/cycling-tuscany": "1 year",
    "/content/dam/wknd-shared/en/adventures/west-coast-cycling/west-coast-cycling": "1 year",
    "/content/dam/wknd-shared/en/adventures/downhill-skiing-wyoming/downhill-skiing-wyoming": "1 year",
    "/content/dam/wknd-shared/en/adventures/riverside-camping-australia/riverside-camping-australia": "1 year",
    "/content/dam/wknd-shared/en/contributors/ian-provo": "1 year",
    "/content/dam/wknd-shared/en/contributors/sofia-sj-berg": "1 year",
    "/content/dam/wknd-shared/en/contributors/justin-barr": "1 year",
    "/content/dam/wknd-shared/en/contributors/jake-hammer": "1 year",
    "/content/dam/wknd-shared/en/contributors/jacob-wester": "1 year",
    "/content/dam/wknd-shared/en/contributors/stacey-roswells": "1 year",
    "/content/dam/wknd-shared/en/contributors/kumar-selveraj": "1 year"
}
```
