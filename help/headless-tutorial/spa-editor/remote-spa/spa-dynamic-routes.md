---
title: 將可編輯的元件添加到遠SPA程動態路由
description: 瞭解如何將可編輯的元件新增至遠端的動態路由SPA。
topic: 無頭、SPA開發
feature: 編SPA輯器、核心元件、API、開發
role: Developer, Architect
level: Beginner
kt: 7636
thumbnail: kt-7636.jpeg
translation-type: tm+mt
source-git-commit: 0eb086242ecaafa53c59c2018f178e15f98dd76f
workflow-type: tm+mt
source-wordcount: '958'
ht-degree: 0%

---


# 動態路由和可編輯的元件

在本章中，我們啟用兩條動態Adventure Detail路由，以支援可編輯的元件；波特蘭的巴釐衝浪營和貝爾瓦納。________

![動態路由和可編輯的元件](./assets/spa-dynamic-routes/intro.png)

Adventure Detail路SPA由定義為`/adventure:path`，其中`path`是WKND Adventure（內容片段）的路徑，以顯示相關細節。

## 將URLSPA對應至頁AEM面

在前兩章中，我們將可編輯的元件內容從「首頁」(Home)SPA檢視對應SPA至AEM`/content/wknd-app/us/en/`的對應遠端根頁面。

為動態路由的可編輯元件定SPA義映射很類似，但我們必須在路由實例和頁之間設定1:1映AEM射方案。

在本教學課程中，我們將取WKND Adventure Content Fragment（路徑的最後一段）的名稱，並將其映射到`/content/wknd-app/us/en/adventure`下的簡單路徑。

| 遠程路SPA由 | AEM頁面路徑 |
|------------------------------------|--------------------------------------------|
| / | /content/wknd-app/us/tw/home |
| /adventure:/content/dam/wknd/en/adventures/bali-surf-camp/__bali-surf-camp__ | /content/wknd-app/us/tw/home/adventure/__bali-surf-camp__ |
| /adventure:/content/dam/wknd/en/adventures/beervana-portland/__beervana-portland__ | /content/wknd-app/us/tw/home/adventure/__beervana-in-portland__ |

因此，根據此映射，我們必須在以下位置建立兩AEM個新頁面：

+ `/content/wknd-app/us/en/home/adventure/bali-surf-camp`
+ `/content/wknd-app/us/en/home/adventure/beervana-in-portland`

## 遠程映SPA射

通過在[中BootstrapSPA](./spa-bootstrap.md)中完成的`setupProxy`配置，配置對於離開RemoteSPA的請求的映射。

## 編SPA輯器映射

透過編輯器開啟SPA請SPA求時的對應，是透過在[ConfigureAEM](./aem-configure.md)中完成的Sling Mappings設定來設定。

## 建立內容頁AEM面

首先，建立中介`adventure`頁面區段：

1. 登入AEM作者
1. 導覽至「__網站> WKND應用程式>我們> en > WKND應用程式首頁__」
   + 此頁AEM面會映射為路由的根SPA，因此，我們將開始在此建立其AEM他路由的頁面結SPA構。
1. 點選「__建立__」並選取「__頁面__」
1. 選擇&#x200B;__遠SPA程頁__&#x200B;模板，然後點選&#x200B;__Next__
1. 填寫頁面屬性
   + __標題__:冒險
   + __名稱__:  `adventure`
      + 此值定義AEM頁面的URL，因此必須符合SPA路由段。
1. 點選&#x200B;__Done__

然後，建立AEM對應至每個需要可編輯SPA區域的URL的頁面。

1. 導覽至「網站管理員」中新的&#x200B;__Adventure__&#x200B;頁面
1. 點選「__建立__」並選取「__頁面__」
1. 選擇&#x200B;__遠SPA程頁__&#x200B;模板，然後點選&#x200B;__Next__
1. 填寫頁面屬性
   + __標題__:巴釐島衝浪營
   + __名稱__:  `bali-surf-camp`
      + 此值定義AEM頁面的URL，因此必須符合路SPA由的最後一個區段
1. 點選&#x200B;__Done__
1. 重複步驟3-6以建立&#x200B;__Beervana in Portland__&#x200B;頁，其中包含：
   + __標題__:波特蘭貝爾瓦納
   + __名稱__:  `beervana-in-portland`
      + 此值定義AEM頁面的URL，因此必須符合路SPA由的最後一個區段

這兩個頁AEM面會保存各自編寫的內容，以供其相符的路SPA由使用。 如果其SPA他路由需要編AEM寫，則必須在其SPAURL的「遠端頁面」的根頁SPA面(`/content/wknd-app/us/en/home`)下建立新的頁AEM面。

## 更新WKND應用程式

讓我們將在[最後一章](./spa-container-component.md)中建立的`<AEMResponsiveGrid...>`元件放入我們的`AdventureDetail`元SPA件中，建立可編輯的容器。

### 放置AEMResponsiveGrid組SPA件

將`<AEMResponsiveGrid...>`置於`AdventureDetail`元件中，會在該路由中建立可編輯的容器。 訣竅在於，多條路由使用`AdventureDetail`元件來演算，因此我們必須動態調整`<AEMResponsiveGrid...>'s pagePath`屬性。 `pagePath`必鬚根據路由實例所顯示的AEM冒險，衍生為指向相應頁面。

1. 開啟並編輯`react-app/src/components/AdventureDetail.js`
1. 在`AdventureDetail(..)'s`第二個`return(..)`陳述式之前新增下列行，此陳述式會從內容片段路徑衍生冒險名稱。

   ```
   ...
   // Get the last segment of the Adventure Content Fragment path to used to generate the pagePath for the AEMResponsiveGrid
   const adventureName = adventureData._path.split('/').pop();
   ...
   ```

1. 匯入`AEMResponsiveGrid`元件，並將其置於`<h2>Itinerary</h2>`元件上方。
1. 在`<AEMResponsiveGrid...>`元件上設定以下屬性
   + `pagePath = '/content/wknd-app/us/en/home/adventure/${adventureName}'`
   + `itemPath = 'root/responsivegrid'`

   這指示`AEMResponsiveGrid`元件從資源中檢索其內AEM容：

   + `/content/wknd-app/us/en/home/adventure/${adventureName}/jcr:content/root/responsivegrid`


使用以下行更新`AdventureDetail.js`:

```
...
import AEMResponsiveGrid from '../components/aem/AEMResponsiveGrid';
...

function AdventureDetail(props) {
    ...
    // Get the last segment of the Adventure Content Fragment path to used to generate the pagePath for the AEMResponsiveGrid
    const adventureName = adventureData._path.split('/').pop();

    return(
        ...
        <AEMResponsiveGrid 
            pagePath={`/content/wknd-app/us/en/home/adventure/${adventureName}`}
            itemPath="root/responsivegrid"/>
            
        <h2>Itinerary</h2>
        ...
    )
}
```

`AdventureDetail.js`檔案應該如下所示：

![AdventureDetail.js](./assets/spa-dynamic-routes/adventure-detail-js.png)

## 在中編寫容器AEM

在`<AEMResponsiveGrid...>`就緒且其`pagePath`會根據所轉譯的探險案動態設定後，我們會嘗試在其中編寫內容。

1. 登入AEM作者
1. 導覽至「__網站> WKND應用程式>我們> en__」
1. __「編__ 輯 __WKND應用程式首頁__ 」頁
   + 導覽至&#x200B;__Bali Surf Camp__&#x200B;路由，以SPA編輯它
1. 從右上角的模式選擇器中選擇&#x200B;__預覽__
1. 點選&#x200B;__Bali Surf Camp__&#x200B;卡，導覽至SPA其路線
1. 從模式選擇器中選擇&#x200B;__編輯__
1. 在&#x200B;__Interinal__&#x200B;的正上方，找到&#x200B;__Layout Container__&#x200B;可編輯區域
1. 開啟&#x200B;__頁面編輯器的側欄__，然後選擇&#x200B;__元件視圖__
1. 將部分已啟用的元件拖曳至&#x200B;__版面容器__
   + 影像
   + 文字
   + 標題

   並製作一些促銷行銷文宣。 它可能看起來像這樣：

   ![Bali Adventure Detail Authoring](./assets/spa-dynamic-routes/adventure-detail-edit.png)

1. __在頁__ 面編輯器中預AEM覽變更
1. 重新整理在[http://localhost:3000](http://localhost:3000)本機執行的WKND應用程式，導覽至&#x200B;__Bali Surf Camp__&#x200B;路線，以檢視撰寫的變更！

   ![遙遠的巴釐SPA島](./assets/spa-dynamic-routes/remote-spa-final.png)

當導覽至沒有對應頁面的探險詳細資訊路由時，該路AEM由例項將不提供編寫功能。 若要啟用這些頁面上的編寫功能，只需在AEM __Adventure__&#x200B;頁面下建立具有相符名稱的頁面！

## 恭喜！

恭喜！ 您已將製作功能新增至動態路由SPA!

+ 已將React AEM Editable Component的ResponsiveGrid元件新增至動態路由
+ 建立頁AEM面，以支援在波特蘭（Bali Surf Camp和Beervana）SPA製作兩條特定路線
+ 在動態的巴釐島衝浪訓練營路線上製作內容！

您現在已完成探索如何使用AEMEditor將特定可編SPA輯區域新增至遠端的第一步SPA!


>[!NOTE]
>
>敬請期待！ 本教學課程將擴充至Adobe的最佳實務和建議，說明如何將SPAEditor解決方案部署AEM為Cloud Service和生產環境。