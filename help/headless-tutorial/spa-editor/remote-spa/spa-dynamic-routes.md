---
title: 將可編輯元件添加到遠程SPA動態路由
description: 瞭解如何將可編輯元件添加到遠程動態路由SPA中。
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

# 動態路由和可編輯元件

在本章中，我們啟用兩條動態冒險詳細資訊路由來支援可編輯元件； __巴釐島衝浪營__ 和 __貝爾瓦納在波特蘭__。

![動態路由和可編輯元件](./assets/spa-dynamic-routes/intro.png)

「冒險細節SPA」路由定義為 `/adventure/:slug` 何處 `slug` 是冒險內容片段上的唯一標識符屬性。

## 將URLSPA映射到AEM頁

在前兩章中，我們將可編輯的元件內容從「首頁」視SPA圖映射到位於的相SPA應「遠程根」AEM頁 `/content/wknd-app/us/en/`。

為動態路由的可編輯元件定SPA義映射類似，但必須在路由實例和頁面之間提出1:1映射AEM方案。

在本教程中，我們使用WKND Adventure Content Fragment（WKND冒險內容片段）的名稱，將其映射到以下的簡單路徑 `/content/wknd-app/us/en/adventure`。

| 遠程路SPA由 | AEM頁路徑 |
|------------------------------------|--------------------------------------------|
| / | /content/wknd-app/us/en/home |
| /adventure/__巴釐島衝浪營__ | /content/wknd app/us/en/home/adventure/__巴釐島衝浪營__ |
| /adventure/__貝爾瓦納波特蘭__ | /content/wknd app/us/en/home/adventure/__貝爾瓦納因波特蘭__ |

因此，根據此映射，我們必須在以下位置建立AEM兩個新頁：

+ `/content/wknd-app/us/en/home/adventure/bali-surf-camp`
+ `/content/wknd-app/us/en/home/adventure/beervana-in-portland`

## 遠程映SPA射

通過配置從遠程請求SPA的映射 `setupProxy` 配置完成 [BootstrapSPA](./spa-bootstrap.md)。

## 編SPA輯器映射

通過編SPA輯器打SPA開請求時的AEM映射SPA是通過 [配AEM置](./aem-configure.md)。

## 在中建立內容頁AEM

首先，建立中間 `adventure` 頁段：

1. 登錄到AEM作者
1. 導航到 __站點> WKND應用程式>我們> en > WKND應用程式首頁__
   + 此AEM頁映射為其根，因此SPA我們開始構建其他路AEM由的頁結構SPA。
1. 點擊 __建立__ 選擇 __頁面__
1. 選擇 __遠程SPA頁__ 模板，然後點擊 __下一個__
1. 填寫頁面屬性
   + __標題__:冒險
   + __名稱__: `adventure`
      + 此值定AEM義頁面的URL，因此必須與路SPA由段匹配。
1. 點擊 __完成__

然後，建立AEM與每個需要可編輯區SPA域的URL對應的頁面。

1. 導航到新 __冒險__ 頁面
1. 點擊 __建立__ 選擇 __頁面__
1. 選擇 __遠程SPA頁__ 模板，然後點擊 __下一個__
1. 填寫頁面屬性
   + __標題__:巴釐島衝浪營
   + __名稱__: `bali-surf-camp`
      + 此值定AEM義頁面的URL，因此必須與路SPA由的最後段匹配
1. 點擊 __完成__
1. 重複步驟3-6以建立 __貝爾瓦納在波特蘭__ 頁，其中：
   + __標題__:貝爾瓦納在波特蘭
   + __名稱__: `beervana-in-portland`
      + 此值定AEM義頁面的URL，因此必須與路SPA由的最後段匹配

這兩AEM個頁面保存各自創作的內容，用於其匹SPA配路由。 如果其SPA他路由需要創AEM作，則必須在其SPAURL的「遠程」頁的根SPA頁(`/content/wknd-app/us/en/home`)AEM。

## 更新WKND應用

我們把 `<ResponsiveGrid...>` 在 [最後一章](./spa-container-component.md)進入 `AdventureDetail` 組SPA件，建立可編輯容器。

### 放置ResponsedGrid組SPA件

放置 `<ResponsiveGrid...>` 的 `AdventureDetail` 元件在該路由中建立可編輯的容器。 訣竅在於，多條路由使用 `AdventureDetail` 要渲染的元件，必須動態調整  `<ResponsiveGrid...>'s pagePath` 屬性。 的 `pagePath` 必鬚根據路由實例AEM顯示的歷程派生指向相應頁面。

1. 開啟並編輯 `react-app-/src/components/AdventureDetail.js`
1. 導入 `ResponsiveGrid` 並將其置於 `<h2>Itinerary</h2>` 元件。
1. 在 `<ResponsiveGrid...>` 元件。 注意 `pagePath` 屬性添加當前 `slug` 按上面定義的映射映射到冒險頁。
   + `pagePath = '/content/wknd-app/us/en/home/adventure/${slug}'`
   + `itemPath = 'root/responsivegrid'`

   這指示 `ResponsiveGrid` 從資源中檢索其內容的組AEM件：

   + `/content/wknd-app/us/en/home/adventure/${slug}/jcr:content/root/responsivegrid`


更新 `AdventureDetail.js` 下面的行：

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

的 `AdventureDetail.js` 檔案應如下所示：

![AdventureDetail.js](./assets/spa-dynamic-routes/adventure-detail-js.png)

## 在中建立容器

使用 `<ResponsiveGrid...>` 就位，以及 `pagePath` 根據正在呈現的冒險動態設定，我們嘗試在其中創作內容。

1. 登錄到AEM作者
1. 導航到 __站點> WKND應用>我們> en__
1. __編輯__ 這樣 __WKND應用首頁__ 頁
   + 導航到 __巴釐島衝浪營__ 路SPA由
1. 選擇 __預覽__ 從右上角的模式選擇器
1. 點擊 __巴釐島衝浪營__ 卡SPA以導航到其路線
1. 選擇 __編輯__ 從模式選擇器
1. 查找 __佈局容器__ 位於上方的可編輯區域 __行程__
1. 開啟 __頁面編輯器側欄__，然後選擇 __元件視圖__
1. 將某些已啟用的元件拖到 __佈局容器__
   + 影像
   + 文字
   + 標題

   再製作一些促銷宣傳材料。 它可能是這樣的：

   ![Bali Adventure Detail創作](./assets/spa-dynamic-routes/adventure-detail-edit.png)

1. __預覽__ 在頁面編輯器AEM中所做的更改
1. 刷新本地運行的WKND應用 [http://localhost:3000](http://localhost:3000)，導航至 __巴釐島衝浪營__ 查看所作更改的路由！

   ![偏遠的巴SPA釐島](./assets/spa-dynamic-routes/remote-spa-final.png)

導航到沒有映射頁面的探險詳細資訊路AEM由時，該路由實例沒有創作能力。 要在這些頁面上啟用創作，只需在AEM以下位置建立具有匹配名稱的頁面 __冒險__ 頁面！

## 恭喜！

恭喜！您已將創作能力添加到中的動態路由SPA!

+ 已將React AEM Editable元件的ResponsedGrid元件添加到動態路由
+ 在AEMBali Surf Camp和Beervana（波特蘭）SPA中建立支援兩條特定路線創作的頁面
+ 在動態的巴釐島衝浪訓練營路線上創作內容！

您現在已完成了如何使用AEMEditor將特SPA定可編輯區域添加到RemoteSPA!
