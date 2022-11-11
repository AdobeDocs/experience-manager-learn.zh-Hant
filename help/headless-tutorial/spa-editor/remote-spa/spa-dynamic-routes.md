---
title: 將可編輯的元件添加到遠程SPA動態路由
description: 了解如何在遠端SPA中將可編輯的元件新增至動態路由。
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7636
thumbnail: kt-7636.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
exl-id: 4accc1ca-6f4b-449e-bf2e-06f19d2fe17d
source-git-commit: ece15ba61124972bed0667738ccb37575d43de13
workflow-type: tm+mt
source-wordcount: '901'
ht-degree: 1%

---

# 動態路由和可編輯的元件

在本章中，我們啟用兩個動態冒險詳細資訊路由，以支援可編輯的元件； __巴釐島衝浪營__ 和 __貝爾瓦娜在波特蘭__.

![動態路由和可編輯的元件](./assets/spa-dynamic-routes/intro.png)

Adventure Detail SPA路由定義為 `/adventure/:slug` where `slug` 是「冒險內容片段」上的唯一識別碼屬性。

## 將SPA URL對應至AEM頁面

在前兩章中，我們將可編輯的元件內容從SPA首頁檢視對應至AEM中對應的Remote SPA根頁面(位於 `/content/wknd-app/us/en/`.

為SPA動態路由的可編輯元件定義映射類似，但我們必須在路由實例和AEM頁之間設定1:1映射方案。

在本教學課程中，我們會取名WKND冒險內容片段（路徑的最後一段），並將其對應至下方的簡單路徑 `/content/wknd-app/us/en/adventure`.

| 遠程SPA路由 | AEM頁面路徑 |
|------------------------------------|--------------------------------------------|
| / | /content/wknd-app/us/en/home |
| /adventure/__巴釐島衝浪營__ | /content/wknd-app/us/en/home/adventure/__巴釐島衝浪營__ |
| /adventure/__貝爾瓦納 — 波特蘭__ | /content/wknd-app/us/en/home/adventure/__貝爾瓦納因波特蘭__ |

因此，根據此對應，我們必須在以下位置建立兩個新AEM頁面：

+ `/content/wknd-app/us/en/home/adventure/bali-surf-camp`
+ `/content/wknd-app/us/en/home/adventure/beervana-in-portland`

## 遠端SPA對應

離開遠端SPA之請求的對應會透過 `setupProxy` 完成配置 [BootstrapSPA](./spa-bootstrap.md).

## SPA編輯器對應

透過AEM SPA編輯器開啟SPA時，SPA要求的對應是透過完成的Sling Mappings設定來設定 [設定AEM](./aem-configure.md).

## 在AEM中建立內容頁面

首先，建立中介 `adventure` 頁面區段：

1. 登入AEM作者
1. 導覽至 __Sites > WKND應用程式> us > en > WKND應用程式首頁__
   + 此AEM頁面會以SPA根頁面的形式對應，因此我們就開始為其他SPA路由建立AEM頁面結構。
1. 點選 __建立__ 選取 __頁面__
1. 選取 __遠端SPA頁面__ 範本，然後點選 __下一個__
1. 填寫頁面屬性
   + __標題__:冒險
   + __名稱__: `adventure`
      + 此值定義AEM頁面的URL，因此必須符合SPA的路由區段。
1. 點選 __完成__

然後，建立對應至每個需要可編輯區域的SPA URL的AEM頁面。

1. 導覽至新 __冒險__ 網站管理員中的頁面
1. 點選 __建立__ 選取 __頁面__
1. 選取 __遠端SPA頁面__ 範本，然後點選 __下一個__
1. 填寫頁面屬性
   + __標題__:巴釐島衝浪營
   + __名稱__: `bali-surf-camp`
      + 此值會定義AEM頁面的URL，因此必須符合SPA路徑的最後一個區段
1. 點選 __完成__
1. 重複步驟3-6以建立 __貝爾瓦娜在波特蘭__ 頁面，包含：
   + __標題__:貝爾瓦娜在波特蘭
   + __名稱__: `beervana-in-portland`
      + 此值會定義AEM頁面的URL，因此必須符合SPA路徑的最後一個區段

這兩個AEM頁面會保留各自所編寫的內容，以供其相符的SPA路由使用。 如果其他SPA路由需要編寫，則必須在其SPA URL的遠端SPA頁面根頁面(`/content/wknd-app/us/en/home`)。

## 更新WKND應用程式

我們把 `<ResponsiveGrid...>` 在 [最後一章](./spa-container-component.md)，進入 `AdventureDetail` SPA元件，建立可編輯的容器。

### 放置ResponsiveGrid SPA元件

將 `<ResponsiveGrid...>` 在 `AdventureDetail` 元件會在該路由中建立可編輯的容器。 訣竅在於，多條路線都使用 `AdventureDetail` 要呈現的元件，我們必須動態調整  `<ResponsiveGrid...>'s pagePath` 屬性。 此 `pagePath` 必須衍生，以根據路由的例項顯示的歷程，指向對應的AEM頁面。

1. 開啟和編輯 `react-app-/src/components/AdventureDetail.js`
1. 匯入 `ResponsiveGrid` 元件並放在上面 `<h2>Itinerary</h2>` 元件。
1. 在 `<ResponsiveGrid...>` 元件。 請注意 `pagePath` 屬性會新增目前的 `slug` 會根據上述定義的對應，對應至探險頁面。
   + `pagePath = '/content/wknd-app/us/en/home/adventure/${slug}'`
   + `itemPath = 'root/responsivegrid'`

   這會指示 `ResponsiveGrid` 從AEM資源擷取其內容的元件：

   + `/content/wknd-app/us/en/home/adventure/${slug}/jcr:content/root/responsivegrid`


更新 `AdventureDetail.js` 填入下列行：

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

此 `AdventureDetail.js` 檔案看起來應該像這樣：

![AdventureDetail.js](./assets/spa-dynamic-routes/adventure-detail-js.png)

## 在AEM中製作容器

使用 `<ResponsiveGrid...>` 就位，及其 `pagePath` 我們會嘗試在其中編寫內容，以根據轉譯的冒險動態設定。

1. 登入AEM作者
1. 導覽至 __網站> WKND應用程式>我們>結束__
1. __編輯__ the __WKND應用首頁__ 頁面
   + 導覽至 __巴釐島衝浪營__ 在SPA中路由以進行編輯
1. 選擇 __預覽__ 從右上角的模式選取器
1. 點選 __巴釐島衝浪營__ 卡片以導覽至其路線
1. 選擇 __編輯__ 從模式選取器
1. 找出 __版面容器__ 可編輯區域 __行程__
1. 開啟 __頁面編輯器側邊列__，然後選取 __元件視圖__
1. 將部分已啟用的元件拖曳至 __版面容器__
   + 影像
   + 文字
   + 標題

   再製作一些促銷宣傳材料。 看起來可能像這樣：

   ![Bali Adventure Detail Authoring](./assets/spa-dynamic-routes/adventure-detail-edit.png)

1. __預覽__ AEM頁面編輯器中的變更
1. 重新整理在本機執行的WKND應用程式 [http://localhost:3000](http://localhost:3000)，導覽至 __巴釐島衝浪營__ 查看所作變更的路由！

   ![遠程SPA Bali](./assets/spa-dynamic-routes/remote-spa-final.png)

導覽至沒有對應AEM頁面的探險詳細路由時，該路由例項沒有編寫功能。 若要在這些頁面上啟用編寫，只需在 __冒險__ 頁面！

## 恭喜！

恭喜！您已在SPA中新增動態路由的製作功能！

+ 將AEM React可編輯元件的ResponsiveGrid元件新增至動態路由
+ 建立AEM頁面，以支援SPA（Bali Surf Camp和Beervana，在Portland）中編寫兩條特定路線
+ 在動態的巴釐島衝浪營路線上製作內容！

您現在已完成探索如何使用AEM SPA編輯器將特定可編輯區域新增至遠端SPA的前幾個步驟！
