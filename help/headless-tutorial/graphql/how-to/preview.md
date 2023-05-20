---
title: 內容片段預覽
description: 瞭解如何對所有作者使用內容片段預覽，以快速瞭解內容更改如何影響您的無AEM頭體驗。
version: Cloud Service
feature: Content Fragments
topic: Headless, Content Management, Development
role: Architect, Developer
level: Beginner
doc-type: Tutorial
last-substantial-update: 2023-03-17T00:00:00Z
jira: KT-10841
thumbnail: 3416906.jpeg
exl-id: 247d40a3-ff67-4c1f-86bf-3794d7ce3e32
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '513'
ht-degree: 0%

---

# 內容片段預覽

無AEM頭應用程式支援整合創作預覽。 預覽體驗將AEM作者的內容片段編輯器與您的自定義應用（可通過HTTP定址）連結起來，允許在呈現正在預覽的內容片段的應用中進行深度連結。

>[!VIDEO](https://video.tv.adobe.com/v/3416906?quality=12&learn=on)

要使用內容片段預覽，必須滿足以下幾個條件：

1. 必須將應用部署到作者可訪問的URL
1. 必須將應用配置為連接到AEM作者服務（而不是AEM發佈服務）
1. 應用必須使用可使用的URL或路由設計 [內容片段路徑或ID](#url-expressions) 選擇要在應用程式體驗中顯示以進行預覽的內容片段。

## 預覽URL

預覽URL，使用 [URL表達式](#url-expressions)，在內容片段模型的屬性上設定。

![內容片段模型預覽URL](./assets/preview/cf-model-preview-url.png)

1. 以管理員身份登錄到AEM作者服務
1. 導航到 __「工具」>「常規」>「內容片段模型」__
1. 選擇 __內容片段模型__ 選擇 __屬性__ 按鈕。
1. 使用 [URL表達式](#url-expressions)
   + 預覽URL必須指向連接到AEM Author服務的應用的部署。

### URL表達式

每個內容片段模型都可以設定預覽URL。 使用下表中列出的URL表達式，可以按內容片段參數化預覽URL。 可以在單個預覽URL中使用多個URL表達式。

|  | URL表達式 | 值 |
| --------------------------------------- | ----------------------------------- | ----------- |
| 內容片段路徑 | `${contentFragment.path}` | `/content/dam/wknd-shared/en/adventures/surf-camp-bali/surf-camp-bali` |
| 內容片段ID | `${contentFragment.id}` | `12c34567-8901-2aa3-45b6-d7890aa1c23c` |
| 內容片段變體 | `${contentFragment.variation}` | `main` |
| 內容片段模型路徑 | `${contentFragment.model.path}` | `/conf/wknd-shared/settings/dam/cfm/models/adventure` |
| 內容片段模型名稱 | `${contentFragment.model.name}` | `adventure` |

預覽URL示例：

+ 上的預覽URL __冒險__ 模型看起來 `https://preview.app.wknd.site/adventure${contentFragment.path}` 決心要 `https://preview.app.wknd.site/adventure/content/dam/wknd-shared/en/adventures/surf-camp-bali/surf-camp-bali`
+ 上的預覽URL __文章__ 模型看起來 `https://preview.news.wknd.site/${contentFragment.model.name}/${contentFragment.id}.html?variation=${contentFragment.variation}` 解析 `https://preview.news.wknd.site/article/99c34317-1901-2ab3-35b6-d7890aa1c23c.html?variation=main`

## 應用內預覽

使用配置的內容片段模型的任何內容片段都有一個「預覽」按鈕。 「預覽」按鈕開啟內容片段模型的預覽URL，並將開啟的內容片段的值彈出到 [URL表達式](#url-expressions)。

![預覽按鈕](./assets/preview/preview-button.png)

在預覽應用中的內容片段更改時執行硬刷新（清除瀏覽器的本地快取）。

## 反應示例

讓我們來瀏覽WKND應用程式，它是一個簡單的React應用程式，顯示使用無頭AEMGraphQLAEM API的冒險經歷。

示例代碼在 [吉圖布網](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/preview-tutorial)。

## URL和路由

用於預覽內容片段的URL或路由必須是可組合的 [URL表達式](#url-expressions)。 在WKND應用的此啟用預覽的版本中，通過 `AdventureDetail` 綁定到路由的元件 `/adventure<CONTENT FRAGMENT PATH>`。 因此，WKND Adventure模型的預覽URL必須設定為 `https://preview.app.wknd.site:3000/adventure${contentFragment.path}` 來解析這條路線。

僅當應用程式具有可定址的路由時，才能使用內容片段預覽 [URL表達式](#url-expressions) 以可預覽的方式在應用中呈現該內容片段。

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
        <Routes>
          {/* The route's path must match the Adventure Model's Preview URL expression. In React since the path has `/` you must use wildcards to match instead of the usual `:path` */}
          <Route path='/adventure/*' element={<AdventureDetail />}/>
          <Route path="/" element={<Home />}/>
        </Routes>
      </div>
    </Router>
  );
}

export default App;
```

### 顯示創作的內容

的 `AdventureDetail` 元件只解析通過 `${contentFragment.path}` [URL表達式](#url-expressions)，並使用它收集和呈現WKND冒險。

+ `src/components/AdventureDetail.js`

```javascript
...
function AdventureDetail() {

    // Read the `path` value which is the parameter used to query for the adventure's details
    // since the params value captures the `*` wildcard in `/adventure/*`, or everything after the first `/` in the Content Fragment path.
    const params = useParams();
    const pathParam = params["*"];

    // Add the leading '/' back on 
    const path = '/' + pathParam;
    
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
