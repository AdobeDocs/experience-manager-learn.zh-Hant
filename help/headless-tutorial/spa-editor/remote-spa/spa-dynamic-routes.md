---
title: 將可編輯的元件新增至遠端SPA動態路由
description: 瞭解如何將可編輯的元件新增至遠端SPA中的動態路由。
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7636
thumbnail: kt-7636.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: 4accc1ca-6f4b-449e-bf2e-06f19d2fe17d
duration: 260
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '903'
ht-degree: 0%

---

# 動態路由和可編輯的元件

在本章中，我們啟用兩個動態冒險詳細資訊路徑來支援可編輯的元件； __巴厘島衝浪營__ 和 __波特蘭貝爾瓦納__.

![動態路由和可編輯的元件](./assets/spa-dynamic-routes/intro.png)

Adventure Detail SPA路由定義為 `/adventure/:slug` 位置 `slug` 是冒險內容片段上的唯一識別碼屬性。

## 將SPA URL對應至AEM頁面

在前兩章中，我們將可編輯的元件內容從SPA首頁檢視對應至AEM中對應的遠端SPA根目錄頁面，位於 `/content/wknd-app/us/en/`.

為SPA動態路由的可編輯元件定義對映類似，但是必須在路由的執行個體與AEM頁面之間進行1:1對映配置。

在本教學課程中，我們使用WKND冒險內容片段的名稱（此為路徑的最後一個區段），並將其對應至下的簡單路徑 `/content/wknd-app/us/en/adventure`.

| 遠端SPA路由 | AEM頁面路徑 |
|------------------------------------|--------------------------------------------|
| / | /content/wknd-app/us/en/home |
| /adventure/__bali-surf-camp__ | /content/wknd-app/us/en/home/adventure/__bali-surf-camp__ |
| /adventure/__貝爾瓦納 — 波特蘭__ | /content/wknd-app/us/en/home/adventure/__波特蘭貝爾瓦納__ |

因此，根據此對應，我們必須在以下位置建立兩個新的AEM頁面：

+ `/content/wknd-app/us/en/home/adventure/bali-surf-camp`
+ `/content/wknd-app/us/en/home/adventure/beervana-in-portland`

## 遠端SPA對應

對於離開遠端SPA的請求，其對應是透過 `setupProxy` 設定完成於 [BootstrapSPA](./spa-bootstrap.md).

## SPA編輯器對應

透過SPA SPA編輯器開啟SPA時，AEM要求的對應可透過在中完成的Sling對應設定進行設定 [設定AEM](./aem-configure.md).

## 在AEM中建立內容頁面

首先，建立中介 `adventure` 頁面區段：

1. 登入AEM Author
1. 瀏覽至 __網站> WKND應用程式>我們>英文> WKND應用程式首頁__
   + 此AEM頁面會對應為SPA的根目錄，因此我們在這裡開始為其他SPA路由建立AEM頁面結構。
1. 點選 __建立__ 並選取 __頁面__
1. 選取 __遠端SPA頁面__ 範本，然後點選 __下一個__
1. 填寫頁面屬性
   + __標題__：冒險
   + __名稱__：`adventure`
      + 此值會定義AEM頁面的URL，因此必須符合SPA的路由區段。
1. 點選 __完成__

然後，建立與需要可編輯區域的每個SPA URL相對應的AEM頁面。

1. 瀏覽至新的 __冒險__ 網站管理員中的頁面
1. 點選 __建立__ 並選取 __頁面__
1. 選取 __遠端SPA頁面__ 範本，然後點選 __下一個__
1. 填寫頁面屬性
   + __標題__：巴厘島衝浪營
   + __名稱__：`bali-surf-camp`
      + 此值會定義AEM頁面的URL，因此必須符合SPA路由的最後一個區段
1. 點選 __完成__
1. 重複步驟3至6以建立 __波特蘭貝爾瓦納__ 頁面，包含：
   + __標題__：波特蘭的Beervana
   + __名稱__：`beervana-in-portland`
      + 此值會定義AEM頁面的URL，因此必須符合SPA路由的最後一個區段

這兩個AEM頁面分別擁有其相符SPA路由的編寫內容。 如果其他SPA路由需要編寫，則必須在其SPA SPA URL的遠端AEM頁面的根頁面(`/content/wknd-app/us/en/home`AEM )。

## 更新WKND應用程式

讓我們放下 `<ResponsiveGrid...>` 在中建立的元件 [上一章](./spa-container-component.md)，至我們的 `AdventureDetail` SPA元件，建立可編輯的容器。

### 放置ResponsiveGrid SPA元件

置入 `<ResponsiveGrid...>` 在 `AdventureDetail` 元件會在該路徑中建立可編輯的容器。 訣竅在於多重路由使用 `AdventureDetail` 要呈現的元件，我們必須動態調整  `<ResponsiveGrid...>'s pagePath` 屬性。 此 `pagePath` 必須衍生為根據路由例項顯示的冒險指向對應的AEM頁面。

1. 開啟並編輯 `react-app-/src/components/AdventureDetail.js`
1. 匯入 `ResponsiveGrid` 元件，並將其放在 `<h2>Itinerary</h2>` 元件。
1. 在 `<ResponsiveGrid...>` 元件。 請注意 `pagePath` 屬性新增目前的 `slug` 會依照上述定義的對應，對應至冒險頁面。
   + `pagePath = '/content/wknd-app/us/en/home/adventure/${slug}'`
   + `itemPath = 'root/responsivegrid'`

   這說明 `ResponsiveGrid` 元件以從AEM資源擷取其內容：

   + `/content/wknd-app/us/en/home/adventure/${slug}/jcr:content/root/responsivegrid`

更新 `AdventureDetail.js` 包含以下行：

```javascript
...
import { ResponsiveGrid } from '@adobe/aem-react-editable-components';
...

function AdventureDetailRender(props) {
    ...
    // Get the slug from the React route parameter, this will be used to specify the AEM Page to store/read editable content from
    const { slug } = useParams();

    return(
        ...
        // Pass the slug in
        function AdventureDetailRender({ title, primaryImage, activity, adventureType, tripLength, 
                groupSize, difficulty, price, description, itinerary, references, slug }) {
            ...
            return (
                ...
                <ResponsiveGrid 
                    pagePath={`/content/wknd-app/us/en/home/adventure/${slug}`}
                    itemPath="root/responsivegrid"/>
                    
                <h2>Itinerary</h2>
                ...
            )
        }
    )
}
```

此 `AdventureDetail.js` 檔案應如下所示：

![AdventureDetail.js](./assets/spa-dynamic-routes/adventure-detail-js.png)

## 在AEM中編寫容器

使用 `<ResponsiveGrid...>` 就位，及其 `pagePath` 我們會根據呈現的冒險動態設定，嘗試在其中製作內容。

1. 登入AEM Author
1. 瀏覽至 __網站> WKND應用程式>我們>英文__
1. __編輯__ 此 __WKND應用程式首頁__ 頁面
   + 導覽至 __巴厘島衝浪營__ 在SPA中路由以進行編輯
1. 選取 __預覽__ 從右上角的模式選取器
1. 點選 __巴厘島衝浪營__ SPA中用於導覽至其路由的卡片
1. 選取 __編輯__ 從模式選取器
1. 找到 __配置容器__ 上方的可編輯區域 __行程__
1. 開啟 __頁面編輯器側欄__，然後選取 __元件檢視__
1. 將部分已啟用的元件拖曳至 __配置容器__
   + 影像
   + 文字
   + 標題

   並建立一些促銷行銷資料。 它看起來可能像這樣：

   ![Bali Adventure詳細資料製作](./assets/spa-dynamic-routes/adventure-detail-edit.png)

1. __預覽__ 您在AEM頁面編輯器中進行的變更
1. 重新整理在本機執行的WKND應用程式 [http://localhost:3000](http://localhost:3000)，導覽至 __巴厘島衝浪營__ 檢視所編寫變更的路由！

   ![遠端SPA巴厘島](./assets/spa-dynamic-routes/remote-spa-final.png)

當導覽至沒有對應AEM頁面的冒險詳細資訊路由時，該路由執行個體上沒有製作功能。 若要在這些頁面上啟用撰寫功能，只需在「 」下建立具有相符名稱的AEM頁面 __冒險__ 頁面！

## 恭喜！

恭喜！您已新增在SPA中編寫動態路由的功能！

+ 將AEM React Editable元件的ResponsiveGrid元件新增至動態路由
+ 建立AEM頁面，以支援在SPA中製作兩個特定路徑（Bali Surf Camp和Beervana in Portland）
+ 動態巴厘島衝浪營路線上的撰寫內容！

您現在已經完成了探索AEM SPA Editor如何用來將特定可編輯區域新增到遠端SPA的第一步！
