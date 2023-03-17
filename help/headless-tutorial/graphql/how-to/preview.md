---
title: 內容片段預覽
description: 了解如何對所有作者使用內容片段預覽，以快速了解內容變更對AEM無頭體驗有何影響。
version: Cloud Service
feature: Content Fragments
topic: Headless, Content Management, Development
role: Architect, Developer
level: Beginner
doc-type: Tutorial
last-substantial-update: 2023-03-17T00:00:00Z
jira: KT-10841
thumbnail: 3416906.jpeg
source-git-commit: 024b2faac2e5a1a8d4bac64d1f70f292aac75dd0
workflow-type: tm+mt
source-wordcount: '513'
ht-degree: 0%

---


# 內容片段預覽

AEM無頭應用程式支援整合式製作預覽。 預覽體驗會將AEM作者的內容片段編輯器與您的自訂應用程式連結（可透過HTTP定址），讓應用程式內可有深層連結，可轉譯正在預覽的內容片段。

>[!VIDEO](https://video.tv.adobe.com/v/3416906/?quality=12&learn=on)

若要使用內容片段預覽，必須符合數個條件：

1. 應用程式必須部署至可供作者存取的URL
1. 應用程式必須設定為連線至AEM製作服務（而非AEM發佈服務）
1. 應用程式的設計必須包含可使用的URL或路由 [內容片段路徑或ID](#url-expressions) 選取要顯示以在應用程式體驗中預覽的內容片段。

## 預覽URL

預覽URL，使用 [URL運算式](#url-expressions)，則會設定在內容片段模型的屬性上。

![內容片段模型預覽URL](./assets/preview/cf-model-preview-url.png)

1. 以管理員身分登入AEM製作服務
1. 導覽至 __工具>一般>內容片段模型__
1. 選取 __內容片段模型__ 選取 __屬性__ ，即可檢視。
1. 輸入內容片段模型的預覽URL，使用 [URL運算式](#url-expressions)
   + 預覽URL必須指向連線至AEM製作服務之應用程式的部署。

### URL運算式

每個內容片段模型都可以設定預覽URL。 可使用下表所列的URL運算式，為每個內容片段的預覽URL進行參數化。 單一預覽URL中可使用多個URL運算式。

|  | URL運算式 | 值 |
| --------------------------------------- | ----------------------------------- | ----------- |
| 內容片段路徑 | `${contentFragment.path}` | `/content/dam/wknd-shared/en/adventures/surf-camp-bali/surf-camp-bali` |
| 內容片段ID | `${contentFragment.id}` | `12c34567-8901-2aa3-45b6-d7890aa1c23c` |
| 內容片段變化 | `${contentFragment.variation}` | `main` |
| 內容片段模型路徑 | `${contentFragment.model.path}` | `/conf/wknd-shared/settings/dam/cfm/models/adventure` |
| 內容片段模型名稱 | `${contentFragment.model.name}` | `adventure` |

預覽URL範例：

+ 上的預覽URL __冒險__ 模型看起來 `https://preview.app.wknd.site/adventure${contentFragment.path}` 它解析為 `https://preview.app.wknd.site/adventure/content/dam/wknd-shared/en/adventures/surf-camp-bali/surf-camp-bali`
+ 上的預覽URL __文章__ 模型看起來 `https://preview.news.wknd.site/${contentFragment.model.name}/${contentFragment.id}.html?variation=${contentFragment.variation}` 解析 `https://preview.news.wknd.site/article/99c34317-1901-2ab3-35b6-d7890aa1c23c.html?variation=main`

## 應用程式內預覽

任何使用已設定內容片段模型的內容片段都有「預覽」按鈕。 「預覽」按鈕會開啟內容片段模型的預覽URL，並將開啟的內容片段值插入 [URL運算式](#url-expressions).

![預覽按鈕](./assets/preview/preview-button.png)

在應用程式中預覽內容片段變更時，執行硬式重新整理（清除瀏覽器的本機快取）。

## React範例

讓我們來探索WKND應用程式，這是一個簡單的React應用程式，可透過AEM Headless GraphQL API顯示來自AEM的歷險經歷。

上提供范常式式碼 [Github.com](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/preview-tutorial).

## URL和路由

用來預覽內容片段的URL或路由必須可使用 [URL運算式](#url-expressions). 在此可預覽的WKND應用程式版本中，探險內容片段會透過 `AdventureDetail` 綁定到路由的元件 `/adventure<CONTENT FRAGMENT PATH>`. 因此，WKND Adventure模型的預覽URL必須設為 `https://preview.app.wknd.site:3000/adventure${contentFragment.path}` 來解決這條路。

內容片段預覽僅適用於應用程式具有可定址的路由（可填入）時 [URL運算式](#url-expressions) 以可預覽的方式在應用程式中呈現內容片段。

+ `src/App.js`

```javascript
...
function App() {
  return (
    <Router>
      <div className="App">
        <header>
            <Link to={"/"}>
                <img src={logo} className="logo" alt="WKND Logo"/>
            </Link>        
            <hr />
        </header>
        <Switch>
          {/* The route's path must match the Adventure Model's Preview URL expression. In React since the path has `/` you must use wildcards to match instead of the usual `:path` */}
          <Route path='/adventure/*'>
            <AdventureDetail />
          </Route>
          <Route path="/">
            <Home />
          </Route>
        </Switch>
      </div>
    </Router>
  );
}

export default App;
```

### 顯示製作的內容

此 `AdventureDetail` 元件只會剖析內容片段路徑，透過 `${contentFragment.path}` [URL運算式](#url-expressions)，並使用它來收集和轉譯WKND探險。

+ `src/components/AdventureDetail.js`

```javascript
...
function AdventureDetail() {

    // Read the content fragment path value which is the parameter used to query for the adventure's details
    
    // Add the leading '/' back on since the params value captures the `*` wildcard in `/adventure/*`, or everything after the first `/` in the Content Fragment path.
    const path = '/' + useParams()[0];

    // Query AEM for the Adventures's details, using the Content Fragment's `path`
    const { adventure, references, error } = useAdventureByPath(path);

    // Handle error and loading conditions
    if (error) {
        return <Error errorMessage={error} />;
    } else if (!adventure) {
        return <Loading />;
    }

    return (<div className="adventure-detail">
        ...
        <AdventureDetailRender {...adventure} references={references} />
    </div>);
}
...
```
