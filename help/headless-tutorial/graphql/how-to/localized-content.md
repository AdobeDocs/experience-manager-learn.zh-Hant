---
title: 搭配AEM Headless使用本地化內容
description: 了解如何使用GraphQL查詢AEM中的本地化內容。
version: Cloud Service
feature: GraphQL API
topic: Headless
role: Developer
level: Intermediate
kt: 10254
thumbnail: KT-10254.jpeg
source-git-commit: 4fa84b0461cbdf2e25336259c4128be5585b8787
workflow-type: tm+mt
source-wordcount: '513'
ht-degree: 0%

---


# 使用AEM Headless進行本地化內容

AEM提供 [翻譯整合架構](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/reusing-content/translation/integration-framework.html) 針對無頭內容，可輕鬆翻譯內容片段和支援資產，以便跨地區設定使用。 這是用來轉譯其他AEM內容(例如頁面、體驗片段、資產和Forms)的相同架構。 一次 [無頭內容已翻譯](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/journeys/translation/overview.html)，且已發佈，可供無頭應用程式使用。

## 資產資料夾結構{#assets-folder-structure}

請確定AEM中的本地化內容片段遵循 [推薦的定位結構](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/journeys/translation/getting-started.html#recommended-structure).

![本地化的AEM資產資料夾](./assets/localized-content/asset-folders.jpg)

區域設定資料夾必須是同級資料夾，而資料夾名稱（而非標題）必須是有效的 [ISO 639-1代碼](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) 表示資料夾中所含內容的地區設定。

地區代碼也是用於篩選GraphQL查詢返回的內容片段的值。

| 地區代碼 | AEM路徑 | 內容地區 |
|--------------------------------|----------|----------|
| de | /content/dam/.../**de**/.. | 德文內容 |
| en | /content/dam/.../**en**/.. | 英文內容 |
| es | /content/dam/.../**es**/.. | 西班牙文內容 |

## GraphQL持續查詢

AEM提供 `_locale` GraphQL篩選器，可依地區設定代碼自動篩選內容。 例如，查詢 [WKND參考示範專案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/demo-add-on/create-site.html) 可對新的持續查詢完成 `wknd-shared/adventures-by-locale` 定義為：

```graphql
query($locale: String!) {
  adventureList(_locale: $locale) {
    items {      
      _path
      title
    }
  }
}
```

此 `$locale` 變數 `_locale` 篩選器需要地區代碼(例如 `en`, `en_us`，或 `de`) [AEM資產資料夾基礎本地化慣例](#assets-folder-structure).

## React範例

讓我們建立一個簡單的React應用程式，可根據使用的地區設定選取器，控制要從AEM中查詢的Adventure內容 `_locale` 篩選。

當 __英文__ 在地區選取器中選取，然後選取下方的英文冒險內容片段 `/content/dam/wknd/en` 傳回時 __西班牙文__ ，然後選取下方的西班牙文內容片段 `/content/dam/wknd/es`、等等。

![本地化React範例應用程式](./assets/localized-content/react-example.png)

### 建立 `LocaleContext`{#locale-context}

首先，建立 [React內容](https://reactjs.org/docs/context.html) 允許在React應用程式的元件中使用地區設定。

```javascript
// src/LocaleContext.js

import React from 'react'

const DEFAULT_LOCALE = 'en';

const LocaleContext = React.createContext({
    locale: DEFAULT_LOCALE, 
    setLocale: () => {}
});

export default LocaleContext;
```

### 建立 `LocaleSwitcher` 反應元件{#locale-switcher}

接下來，建立設定為 [LocaleContext的](#locale-context) 值。

此區域設定值用於驅動GraphQL查詢，確保它們只返回與所選區域設定匹配的內容。

```javascript
// src/LocaleSwitcher.js

import { useContext } from "react";
import LocaleContext from "./LocaleContext";

export default function LocaleSwitcher() {
  const { locale, setLocale } = useContext(LocaleContext);

  return (
    <select value={locale}
            onChange={e => setLocale(e.target.value)}>
      <option value="de">Deutsch</option>
      <option value="en">English</option>
      <option value="es">Español</option>
    </select>
  );
}
```

### 使用 `_locale` 篩選{#adventures}

歷險元件會依地區設定查詢AEM的所有歷險，並列出其標題。 這是透過將React內容中儲存的地區設定值傳遞至查詢來達成的，方法是使用 `_locale` 篩選。

此方法可擴展到應用程式中的其他查詢，確保所有查詢僅包括由用戶的區域設定選擇指定的內容。

對AEM的查詢會在自訂React鈎點中執行 [getAdventuresByLocale，如需查詢AEM GraphQL檔案的詳細說明](./aem-headless-sdk.md).

```javascript
// src/Adventures.js

import { useContext } from "react"
import { useAdventuresByLocale } from './api/persistedQueries'
import LocaleContext from './LocaleContext'

export default function Adventures() {
    const { locale } = useContext(LocaleContext);

    // Get data from AEM using GraphQL persisted query as defined above 
    // The details of defining a React useEffect hook are explored in How to > AEM Headless SDK
    let { data, error } = useAdventuresByLocale(locale);

    return (
        <ul>
            {data?.adventureList?.items?.map((adventure, index) => { 
                return <li key={index}>{adventure.title}</li>
            })}
        </ul>
    )
}
```

### 定義 `App.js`{#app-js}

最後，將React應用程式包裝在中，將其與 `LanguageContext.Provider` 和設定地區設定值。 這可讓其他React元件， [地區切換器](#locale-switcher)，和 [冒險](#adventures) 共用區域設定選擇狀態。

```javascript
// src/App.js

import { useState, useContext } from "react";
import LocaleContext from "./LocaleContext";
import LocaleSwitcher from "./LocaleSwitcher";
import Adventures from "./Adventures";

export default function App() {
  const [locale, setLocale] = useState(useContext(LocaleContext).locale);

  return (
    <LocaleContext.Provider value={{locale, setLocale}}>
      <LocaleSwitcher />
      <Adventures />
    </LocaleContext.Provider>
  );
}
```
