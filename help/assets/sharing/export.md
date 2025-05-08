---
title: 匯出資產
description: 瞭解如何大量匯出資產並將資產下載到您的本機電腦。
feature: Asset Management
version: Experience Manager as a Cloud Service
topic: Content Management
role: Developer
level: Experienced
last-substantial-update: 2025-04-28T00:00:00Z
doc-type: Tutorial
jira: KT-15313
thumbnail: KT-15313.jpeg
exl-id: d04c3316-6f8f-4fd1-9df1-3fe09d44f735
duration: 256
source-git-commit: 107a9a77a1bf2337f309d503a4a310d8d0781f0d
workflow-type: tm+mt
source-wordcount: '510'
ht-degree: 0%

---

# 匯出資產

瞭解如何使用可自訂的Node.js指令碼，將資產匯出至本機電腦。 此匯出指令碼提供如何使用[AEM HTTP API](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets)從AEM Assets以程式設計方式下載資產的範例，特別著重於原始轉譯以確保最高品質。 其設計可在本機磁碟機上複製AEM Assets的資料夾結構，以輕鬆備份或移轉資產。

指令碼只會下載資產的原始轉譯，不會下載相關聯的中繼資料，除非該中繼資料已內嵌到資產中做為XMP。 這表示所有儲存在AEM中但未整合至資產檔案的描述性資訊、分類或標籤，都不會納入下載中。 修改指令碼以包含其他轉譯專案，也可以下載這些轉譯。 確保您有足夠的空間來儲存匯出的資產。

指令碼通常會針對AEM Author執行，但也可以針對AEM Publish執行，只要可透過Dispatcher存取AEM Assets HTTP API端點和資產轉譯。

在執行指令碼之前，您必須使用AEM執行個體URL、使用者認證（存取權杖）以及您要匯出的資料夾路徑來設定它。

## 匯出指令碼

編寫為JavaScript模組的指令碼是Node.js專案的一部分，因為它有對`node-fetch`和`p-limit`的相依性。 您可以將下列指令碼複製到型別`module`的空的Node.js專案中，並執行`npm install node-fetch p-limit`以安裝相依性。

此指令碼會瀏覽AEM Assets資料夾樹狀結構，並將資產和資料夾下載到您電腦上的本機資料夾。 它會使用[AEM Assets HTTP API](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets)來擷取資料夾和資產資料，並下載資產的原始轉譯。

```javascript
// export-assets.js

import fetch from 'node-fetch';
import { promises as fs } from 'fs';
import path from 'path';
import pLimit from 'p-limit';

// Do not process the contents of these well-known AEM system folders
const SKIP_FOLDERS = ['/content/dam/appdata', '/content/dam/projects', '/content/dam/_CSS', '/content/dam/_DMSAMPLE' ];

/**
 * Determine if the folder should be processed based on the entity and AEM path.
 * 
 * @param {Object} entity the AEM entity that should represent a folder returned from AEM Assets HTTP API
 * @param {String} aemPath the path in AEM of this source
 * @returns true if the entity should be processed, false otherwise
 */
function isValidFolder(entity, aemPath) {
    if (aemPath === '/content/dam') {
        return true;
    } else if (!entity.class.includes('assets/folder')) {
        return false;
    } else if (SKIP_FOLDERS.find((path) => path === aemPath)) {
        return false;
    } else if (entity.properties.hidden) {
        return false;
    }
    
    return true;
}

/**
 * Determine if the entity is downloadable.
 * @param {Object} entity the AEM entity that should represent an asset returned from AEM Assets HTTP API
 * @returns true if the entity is downloadable, false otherwise
 */
function isDownloadable(entity) {
    if (entity.class.includes('assets/folder')) {
        return false;
    } else if (entity.properties.contentFragment) {
        return false;
    }
    return true;
}

/**
 * Helper function to get the link from the entity based on the relationship name.
 * @param {Object} entity the entity from the AEM Assets HTTP API
 * @param {String} rel the relationship name
 * @returns {String} link URL
 */
function getLink(entity, rel) {
    return entity.links.find(link => link.rel.includes(rel));
}

/**
 * Helper function to fetch JSON data from the AEM Assets HTTP API.
 * @param {String} url the AEM Assets HTTP API URL to fetch data from
 * @returns {Object} the JSON response
 */
async function fetchJSON(url) {
    const response = await fetch(url, {
        method: 'GET',
        headers: {
            'Authorization': `Bearer ${AEM_ACCESS_TOKEN}`,
            'Content-Type': 'application/json'
        }
    });

    if (!response.ok) {
        throw new Error(`Error fetching ${url}: ${response.status}`);
    }

    return response.json();
}

/**
 * Helper function to download a file from AEM Assets.
 * @param {String} url the URL of the asset rendition to download
 * @param {String} outputPath the local path to save the downloaded file
 */
async function downloadFile(url, outputPath) {
    const response = await fetch(url, {
        method: 'GET',
        headers: {
            'Authorization': `Bearer ${AEM_ACCESS_TOKEN}`,
        }
    });

    if (!response.ok) {
        throw new Error(`Failed to download file: ${response.statusText}`);
    }

    const arrayBuffer = await response.arrayBuffer();
    await fs.writeFile(outputPath, Buffer.from(arrayBuffer));

    console.log(`Downloaded asset: ${outputPath}`);
}

/**
 * Main entry point to download assets from AEM.
 * 
 * @param {Object} options 
 * @param {String} options.apiUrl (optional) the direct AEM Assets HTTP API URL
 * @param {String} options.localPath local filesystem path to save the assets
 * @param {String} options.aemPath AEM folder path
 */
async function downloadAssets({ apiUrl, localPath = LOCAL_DOWNLOAD_FOLDER, aemPath = '/content/dam' }) {
    if (!apiUrl) {
        const prefix = "/content/dam/";
        let apiPath = aemPath.startsWith(prefix) ? aemPath.substring(prefix.length) : aemPath;    

        if (!apiPath.startsWith('/')) {
            apiPath = '/' + apiPath;
        }

        apiUrl = `${AEM_HOST}/api/assets.json${apiPath}`;
    }
    
    const data = await fetchJSON(apiUrl);
    const entities = data.entities || [];

    // First, process folders
    for (const folder of entities.filter(entity => entity.class.includes('assets/folder'))) {
        const newLocalPath = path.join(localPath, folder.properties.name);
        const newAemPath = path.join(aemPath, folder.properties.name);

        if (!isValidFolder(folder, newAemPath)) {
            continue;
        }

        await fs.mkdir(newLocalPath, { recursive: true });

        await downloadAssets({
            apiUrl: getLink(folder, 'self')?.href,
            localPath: newLocalPath,
            aemPath: newAemPath
        });
    }

    // Now, process assets with concurrency limit
    const limit = pLimit(MAX_CONCURRENT_DOWNLOADS);
    const downloads = [];

    for (const asset of entities.filter(entity => entity.class.includes('assets/asset'))) {
        const assetLocalPath = path.join(localPath, asset.properties.name);
        if (isDownloadable(asset)) {
            downloads.push(limit(() => downloadFile(getLink(asset, 'content')?.href, assetLocalPath)));
        }
    }

    await Promise.all(downloads);

    // Handle pagination
    const nextUrl = getLink(data, 'next');
    if (nextUrl) {
        await downloadAssets({
            apiUrl: nextUrl?.href,
            localPath,
            aemPath
        });
    }
}

/***** SCRIPT CONFIGURATION *****/

// AEM host is the URL of the AEM environment to download the assets from
const AEM_HOST = 'https://author-p123-e456.adobeaemcloud.com';

// AEM access token used to access the AEM host. 
// This access token must have read access to the folders and assets to download.
const AEM_ACCESS_TOKEN = "eyJhbGciOiJS...zCprYZD0rSjg6g";

// The root folder in AEM to download assets from.
const AEM_ASSETS_FOLDER = '/content/dam/wknd-shared';

// The local folder to save the downloaded assets.
const LOCAL_DOWNLOAD_FOLDER = './exported-assets';

// The number of maximum concurrent downloads to avoid overwhelming the client or server.
const MAX_CONCURRENT_DOWNLOADS = 10;

/***** SCRIPT ENTRY POINT *****/

console.time('Download AEM assets');

await downloadAssets({
    aemPath: AEM_ASSETS_FOLDER,
    localPath: LOCAL_DOWNLOAD_FOLDER
}).catch(console.error);

console.timeEnd('Download AEM assets');
```

## 設定匯出

下載指令碼後，請更新指令碼底部的設定變數。

可以使用[對AEM as a Cloud Service](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview)的權杖式驗證教學課程中的步驟來取得`AEM_ACCESS_TOKEN`。 通常24小時的開發人員權杖就足夠了，只要匯出不到24小時的時間完成，而且產生權杖的使用者擁有要匯出的資產的讀取存取權即可。

```javascript
...
/***** SCRIPT CONFIGURATION *****/

// AEM host is the URL of the AEM environment to download the assets from
const AEM_HOST = 'https://author-p123-e456.adobeaemcloud.com';

// AEM access token used to access the AEM host. 
// This access token must have read access to the folders and assets to download.
const AEM_ACCESS_TOKEN = "eyJhbGciOiJS...zCprYZD0rSjg6g";

// The root folder in AEM to download assets from.
const AEM_ASSETS_FOLDER = '/content/dam/wknd-shared';

// The local folder to save the downloaded assets.
const LOCAL_DOWNLOAD_FOLDER = './export-assets';

// The number of maximum concurrent downloads to avoid overwhelming the client or server. 10 is typically a good value.
const MAX_CONCURRENT_DOWNLOADS = 10;
```

## 匯出資產

使用Node.js執行指令碼，將資產匯出至本機電腦。

視資產數量及其大小而定，指令碼可能需要一些時間才能完成。 執行指令碼時，它[將進度](#output)記錄到主控台。

```shell
$ node export-assets.js
```

## 匯出輸出

匯出指令碼會將進度記錄到主控台，指出正在下載的資產。 指令碼完成時，資產會儲存到設定中指定的本機資料夾，且記錄會以下載資產所花費的總時間結束。

```plaintext
...
Downloaded asset: exported-assets/wknd-shared/en/magazine/skitouring/skitouring3sjoeberg.jpg
Downloaded asset: exported-assets/wknd-shared/en/magazine/skitouring/skitouring5sjoeberg.jpg
Downloaded asset: exported-assets/wknd-shared/en/magazine/skitouring/skitouring6sjoeberg.jpg
Downloaded asset: exported-assets/wknd-shared/en/magazine/western-australia/wa_camping_adobe.pdf
Downloaded asset: exported-assets/wknd-shared/en/magazine/western-australia/adobestock-156407519.jpeg
Downloaded asset: exported-assets/wknd-shared/en/magazine/western-australia/adobe-waadobe-wa-mg-3094.jpg
Downloaded asset: exported-assets/wknd-shared/en/magazine/western-australia/adobe-waadobe-wa-mg-3851.jpg
Downloaded asset: exported-assets/wknd-shared/en/magazine/western-australia/adobe-waadobe-wa-b6a7083.jpg
Downloaded asset: exported-assets/wknd-shared/en/magazine/western-australia/adobe-waadobe-wa-b6a6978.jpg
Download AEM assets: 24.770s
```

匯出的資產可在組態`LOCAL_DOWNLOAD_FOLDER`中指定的本機資料夾中找到。 資料夾結構會反映AEM Assets資料夾結構，並將資產下載至適當的子資料夾。 這些檔案可以上傳至[支援的雲端儲存提供者](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/assets/assets-view/bulk-import-assets-view)，以便[大量匯入](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/cloud-service/migration/bulk-import)至其他AEM執行個體，或用於備份目的。

![已匯出資產](./assets/export/exported-assets.png)
