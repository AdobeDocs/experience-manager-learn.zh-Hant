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
duration: 202
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '903'
ht-degree: 0%

---

# 動態路由和可編輯的元件

在本章中，我們啟用兩個動態冒險詳細資訊路徑來支援可編輯的元件；__Bali Surf Camp__&#x200B;和&#x200B;__波特蘭的Beervana__。

![動態路由和可編輯的元件](./assets/spa-dynamic-routes/intro.png)

Adventure Detail SPA路由定義為`/adventure/:slug`，其中`slug`是Adventure內容片段上的唯一識別碼屬性。

## 將SPA URL對應至AEM頁面

在前兩章中，我們將可編輯的元件內容從SPA首頁檢視對應至AEM中位於`/content/wknd-app/us/en/`的對應遠端SPA根頁面。

為SPA動態路由的可編輯元件定義對映類似，但是必須在路由的執行個體與AEM頁面之間進行1:1對映配置。

在本教學課程中，我們會使用WKND冒險內容片段的名稱（此為路徑的最後一個區段），並將其對應至`/content/wknd-app/us/en/adventure`下的簡單路徑。

| 遠端SPA路由 | AEM頁面路徑 |
|------------------------------------|--------------------------------------------|
| / | /content/wknd-app/us/en/home |
| /adventure/__bali-surf-camp__ | /content/wknd-app/us/en/home/adventure/__bali-surf-camp__ |
| /adventure/__beervana-portland__ | /content/wknd-app/us/en/home/adventure/__beervana-in-portland__ |

因此，根據此對應，我們必須在以下位置建立兩個新的AEM頁面：

+ `/content/wknd-app/us/en/home/adventure/bali-surf-camp`
+ `/content/wknd-app/us/en/home/adventure/beervana-in-portland`

## 遠端SPA對應

已透過[BootstrapSPA](./spa-bootstrap.md)中完成的`setupProxy`設定來設定離開遠端SPA之要求的對應。

## SPA編輯器對應

透過SPA SPA Editor開啟SPA時AEM要求的對應是透過[設定AEM](./aem-configure.md)中完成的Sling對應設定所設定。

## 在AEM中建立內容頁面

首先，建立中介`adventure`頁面區段：

1. 登入AEM Author
1. 導覽至&#x200B;__網站> WKND應用程式>我們>英文> WKND應用程式首頁__
   + 此AEM頁面會對應為SPA的根目錄，因此我們在這裡開始為其他SPA路由建立AEM頁面結構。
1. 點選&#x200B;__建立__&#x200B;並選取&#x200B;__頁面__
1. 選取&#x200B;__遠端SPA頁面__&#x200B;範本，然後點選&#x200B;__下一步__
1. 填寫頁面屬性
   + __標題__：冒險
   + __名稱__：`adventure`
      + 此值會定義AEM頁面的URL，因此必須符合SPA的路由區段。
1. 點選&#x200B;__完成__

然後，建立與需要可編輯區域的每個SPA URL相對應的AEM頁面。

1. 在網站管理員中導覽至新的&#x200B;__Adventure__&#x200B;頁面
1. 點選&#x200B;__建立__&#x200B;並選取&#x200B;__頁面__
1. 選取&#x200B;__遠端SPA頁面__&#x200B;範本，然後點選&#x200B;__下一步__
1. 填寫頁面屬性
   + __標題__：巴厘島衝浪營
   + __名稱__：`bali-surf-camp`
      + 此值會定義AEM頁面的URL，因此必須符合SPA路由的最後一個區段
1. 點選&#x200B;__完成__
1. 重複步驟3-6以在Portland __中建立__ Beervana頁面，包含：
   + __標題__：波特蘭的Beervana
   + __名稱__：`beervana-in-portland`
      + 此值會定義AEM頁面的URL，因此必須符合SPA路由的最後一個區段

這兩個AEM頁面分別擁有其相符SPA路由的編寫內容。 如果其他SPA路由需要編寫，則必須在AEM中遠端AEM頁面的根頁面(`/content/wknd-app/us/en/home`)底下的SPA URL上建立新的SPA頁面。

## 更新WKND應用程式

讓我們將在[最後一個章節](./spa-container-component.md)中建立的`<ResponsiveGrid...>`元件放到`AdventureDetail` SPA元件中，建立可編輯的容器。

### 放置ResponsiveGrid SPA元件

將`<ResponsiveGrid...>`放置在`AdventureDetail`元件中，會在該路由中建立可編輯的容器。 訣竅在於，因為多重路由使用`AdventureDetail`元件來呈現，我們必須動態調整`<ResponsiveGrid...>'s pagePath`屬性。 必須根據路由執行個體顯示的冒險來衍生`pagePath`，以指向對應的AEM頁面。

1. 開啟並編輯`react-app-/src/components/AdventureDetail.js`
1. 匯入`ResponsiveGrid`元件，並將其置於`<h2>Itinerary</h2>`元件上方。
1. 在`<ResponsiveGrid...>`元件上設定下列屬性。 請注意，`pagePath`屬性會新增目前的`slug`，其會根據上述定義的對應對應對應至冒險頁面。
   + `pagePath = '/content/wknd-app/us/en/home/adventure/${slug}'`
   + `itemPath = 'root/responsivegrid'`

   這會指示`ResponsiveGrid`元件從AEM資源擷取其內容：

   + `/content/wknd-app/us/en/home/adventure/${slug}/jcr:content/root/responsivegrid`

使用以下行更新`AdventureDetail.js`：

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

`AdventureDetail.js`檔案應該如下所示：

![AdventureDetail.js](./assets/spa-dynamic-routes/adventure-detail-js.png)

## 在AEM中編寫容器

在`<ResponsiveGrid...>`已就緒且其`pagePath`已根據轉譯的冒險動態設定後，我們會嘗試在其中製作內容。

1. 登入AEM Author
1. 導覽至&#x200B;__網站> WKND應用程式>我們> en__
1. __編輯__ __WKND應用程式首頁__&#x200B;頁面
   + 導覽至SPA中的&#x200B;__巴厘島衝浪營__&#x200B;路線以進行編輯
1. 從右上角的模式選擇器中選取&#x200B;__預覽__
1. 點選SPA中的&#x200B;__Bali Surf Camp__&#x200B;卡以瀏覽其路線
1. 從模式選取器中選取&#x200B;__編輯__
1. 在&#x200B;__行程__&#x200B;的正上方找到&#x200B;__配置容器__&#x200B;可編輯區域
1. 開啟&#x200B;__頁面編輯器的側列__，然後選取&#x200B;__元件檢視__
1. 將部分已啟用的元件拖曳至&#x200B;__配置容器__
   + 影像
   + 文字
   + 標題

   並建立一些促銷行銷資料。 它看起來可能像這樣：

   ![Bali Adventure Detail Authoring](./assets/spa-dynamic-routes/adventure-detail-edit.png)

1. 在AEM頁面編輯器中&#x200B;__預覽__&#x200B;您的變更
1. 重新整理在[http://localhost:3000](http://localhost:3000)本機執行的WKND應用程式，瀏覽至&#x200B;__巴厘島衝浪營__&#x200B;路線，以檢視所編寫的變更！

   ![遠端SPA巴厘島](./assets/spa-dynamic-routes/remote-spa-final.png)

當導覽至沒有對應AEM頁面的冒險詳細資訊路由時，該路由執行個體上沒有製作功能。 若要啟用這些頁面的撰寫功能，只要在&#x200B;__冒險__&#x200B;頁面下建立具有相符名稱的AEM頁面即可！

## 恭喜！

恭喜！您已新增在SPA中編寫動態路由的功能！

+ 將AEM React Editable元件的ResponsiveGrid元件新增至動態路由
+ 建立AEM頁面，以支援在SPA中製作兩個特定路徑（Bali Surf Camp和Beervana in Portland）
+ 動態巴厘島衝浪營路線上的撰寫內容！

您現在已經完成了探索AEM SPA Editor如何用來將特定可編輯區域新增到遠端SPA的第一步！
