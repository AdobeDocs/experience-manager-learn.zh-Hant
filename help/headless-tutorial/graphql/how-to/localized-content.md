---
title: 將本地化內容與無AEM頭
description: 瞭解如何使用GraphQL查詢AEM本地化內容。
version: Cloud Service
feature: GraphQL API
topic: Headless
role: Developer
level: Intermediate
kt: 10254
thumbnail: KT-10254.jpeg
source-git-commit: 4966a48c29ae1b5d0664cb43feeb4ad94f43b4e1
workflow-type: tm+mt
source-wordcount: '495'
ht-degree: 0%

---


# 帶無頭的本地化AEM內容

提AEM供 [翻譯整合框架](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/reusing-content/translation/integration-framework.html) 用於無頭內容，允許輕鬆翻譯內容片段和支援資產以跨語言環境使用。 這是用於翻譯其他內容的同AEM一框架，如頁面、體驗片段、資產和Forms。 一次 [已翻譯無頭內容](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/journeys/translation/overview.html)已發佈，可供無頭應用程式使用。

## 資產資料夾結構{#assets-folder-structure}

確保中的本地化內容AEM片段 [推薦定位結構](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/journeys/translation/getting-started.html#recommended-structure)。

![本地化AEM資產資料夾](./assets/localized-content/asset-folders.jpg)

區域設定資料夾必須是同級資料夾，而資料夾名稱（而不是標題）必須是有效的 [ISO 639-1代碼](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) 表示資料夾中包含的內容的區域設定。

區域設定代碼也是用於篩選GraphQL查詢返回的內容片段的值。

| 區域設定代碼 | AEM路徑 | 內容區域設定 |
|--------------------------------|----------|----------|
| de | /content/dam/.../**德**/.. | 德國內容 |
| en | /content/dam/.../**恩**/.. | 英語內容 |
| es | /content/dam/.../**es**/.. | 西班牙語內容 |

## GraphQL查詢

提AEM供 `_locale` GraphQL篩選器，它按區域設定代碼自動篩選內容。 例如，查詢 [WKND參考演示項目](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/demo-add-on/create-site.html) 會是這樣的：

```graphql
{
  adventureList(_locale: "en") {
    items {      
      _path
      adventureTitle
    }
  }
}
```

的 `_locale` 篩選器需要使用 [資AEM產資料夾基礎本地化慣例](#assets-folder-structure)。

## 反應示例

讓我們建立一個簡單的React應用程式，該應用程式控制根據區域設定選擇器從AEM中查詢的Adventure內容 `_locale` 的子菜單。

當 __英語__ 在區域設定選擇器中選擇，然後在下面的「英語冒險內容片段」 `/content/dam/wknd/en` 返回， __西班牙語__ 選中，然後在下面選擇「西班牙文內容片段」 `/content/dam/wknd/es`等等。

![本地化的React示例應用](./assets/localized-content/react-example.png)

### 建立 `LocaleContext`{#locale-context}

首先，建立 [反應上下文](https://reactjs.org/docs/context.html) 允許在React應用程式的元件中使用區域設定。

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

### 建立 `LocaleSwitcher` 反應組分{#locale-switcher}

接下來，建立一個區域設定切換器React元件，該元件 [區域設定上下文](#locale-context) 值。

此區域設定值用於驅動GraphQL查詢，確保它們僅返回與所選區域設定匹配的內容。

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

### 使用 `_locale` 濾波器{#adventures}

Adventures元件按區域設AEM置查詢所有冒險，並列出其標題。 這是通過使用 `_locale` 的子菜單。

此方法可以擴展到應用程式中的其他查詢，確保所有查詢僅包括用戶的區域設定選擇指定的內容。

針對的查詢AEM在自定義React掛接中執行 [useGraphQL，有關查詢GraphQL文檔的詳細AEM說明](./aem-headless-sdk.md)。

```javascript
// src/Adventures.js

import { useContext } from "react"
import { useGraphQL } from './useGraphQL'
import LocaleContext from './LocaleContext'

export default function Adventures() {
    const { locale } = useContext(LocaleContext);

    let {data} = useGraphQL(`{
            adventureList(_locale: "${locale}") {
                items {      
                _path
                adventureTitle
             }
        }
    }`);

    return (
        <ul>
            {data?.adventureList?.items?.map((adventure, index) => { 
                return <li key={index}>{adventure.adventureTitle}</li>
            })}
        </ul>
    )
}
```

### 定義 `App.js`{#app-js}

最後，將React應用程式與 `LanguageContext.Provider` 和設定區域設定值。 這允許其它反應元件， [區域設定切換器](#locale-switcher), [冒險](#adventures) 共用區域設定選擇狀態。

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
