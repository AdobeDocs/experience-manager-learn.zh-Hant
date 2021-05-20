---
title: 將可編輯的元件添加到遠程SPA動態路由
description: 了解如何在遠端SPA中將可編輯的元件新增至動態路由。
topic: 無頭、SPA、開發
feature: SPA編輯器，核心元件， API，開發
role: Developer, Architect
level: Beginner
kt: 7636
thumbnail: kt-7636.jpeg
source-git-commit: 0eb086242ecaafa53c59c2018f178e15f98dd76f
workflow-type: tm+mt
source-wordcount: '958'
ht-degree: 0%

---


# 動態路由和可編輯的元件

在本章中，我們啟用兩個動態冒險詳細資訊路由，以支援可編輯的元件；在波特蘭的&#x200B;__巴釐島衝浪營__&#x200B;和&#x200B;__貝爾瓦納。__

![動態路由和可編輯的元件](./assets/spa-dynamic-routes/intro.png)

冒險詳細資訊SPA路由定義為`/adventure:path` ，其中`path`是WKND冒險（內容片段）的路徑，用於顯示相關詳細資訊。

## 將SPA URL對應至AEM頁面

在前兩個章節中，我們將可編輯的元件內容從SPA首頁檢視對應至AEM中`/content/wknd-app/us/en/`的對應Remote SPA根頁面。

為SPA動態路由的可編輯元件定義映射類似，但我們必須在路由實例和AEM頁之間設定1:1映射方案。

在本教學課程中，我們將取名路徑的最後一段WKND探險內容片段，並將其對應至`/content/wknd-app/us/en/adventure`下的簡單路徑。

| 遠程SPA路由 | AEM頁面路徑 |
|------------------------------------|--------------------------------------------|
| / | /content/wknd-app/us/en/home |
| /adventure:/content/dam/wknd/en/adventures/bali-surf-camp/__bali-surf-camp__ | /content/wknd-app/us/en/home/adventure/__bali-surf-camp__ |
| /adventure:/content/dam/wknd/en/adventures/beervana-portland/__beervana-portland__ | /content/wknd-app/us/en/home/adventure/__beervana-in-portland__ |

因此，根據此對應，我們必須在以下位置建立兩個新AEM頁面：

+ `/content/wknd-app/us/en/home/adventure/bali-surf-camp`
+ `/content/wknd-app/us/en/home/adventure/beervana-in-portland`

## 遠端SPA對應

離開Remote SPA的請求的對應是透過[中BootstrapSPA](./spa-bootstrap.md)所完成的`setupProxy`設定來設定。

## SPA編輯器對應

透過AEM SPA編輯器開啟SPA時，SPA要求的對應是透過在[Configure AEM](./aem-configure.md)中完成的Sling對應設定來設定。

## 在AEM中建立內容頁面

首先，建立中間`adventure`頁面區段：

1. 登入AEM作者
1. 導覽至「__Sites > WKND應用程式> us > en > WKND應用程式首頁__」
   + 此AEM頁面會以SPA根頁面的形式對應，因此我們就開始為其他SPA路由建立AEM頁面結構。
1. 點選&#x200B;__建立__&#x200B;並選取&#x200B;__頁面__
1. 選取&#x200B;__遠端SPA頁面__&#x200B;範本，然後點選&#x200B;__下一步__
1. 填寫頁面屬性
   + __標題__:冒險
   + __名稱__:  `adventure`
      + 此值定義AEM頁面的URL，因此必須符合SPA的路由區段。
1. 點選&#x200B;__完成__

然後，建立對應至每個需要可編輯區域的SPA URL的AEM頁面。

1. 導覽至「網站管理員」中新的&#x200B;__冒險__&#x200B;頁面
1. 點選&#x200B;__建立__&#x200B;並選取&#x200B;__頁面__
1. 選取&#x200B;__遠端SPA頁面__&#x200B;範本，然後點選&#x200B;__下一步__
1. 填寫頁面屬性
   + __標題__:巴釐島衝浪營
   + __名稱__:  `bali-surf-camp`
      + 此值會定義AEM頁面的URL，因此必須符合SPA路徑的最後一個區段
1. 點選&#x200B;__完成__
1. 重複步驟3-6以在Portland __中建立__ Beervana頁面，並使用：
   + __標題__:貝爾瓦娜在波特蘭
   + __名稱__:  `beervana-in-portland`
      + 此值會定義AEM頁面的URL，因此必須符合SPA路徑的最後一個區段

這兩個AEM頁面會保留其相符SPA路由的個別製作內容。 如果其他SPA路由需要編寫，則必須在其SPA URL的AEM中Remote SPA頁面的根頁面(`/content/wknd-app/us/en/home`)下建立新的AEM頁面。

## 更新WKND應用程式

將在[最後一章](./spa-container-component.md)中建立的`<AEMResponsiveGrid...>`元件放入`AdventureDetail` SPA元件中，建立可編輯的容器。

### 放置AEMResponsiveGrid SPA元件

將`<AEMResponsiveGrid...>`放入`AdventureDetail`元件會在該路徑中建立可編輯的容器。 訣竅在於多條路由使用`AdventureDetail`元件來呈現，因此我們必須動態調整`<AEMResponsiveGrid...>'s pagePath`屬性。 必鬚根據路由的例項顯示的歷程，衍生`pagePath`以指向對應的AEM頁面。

1. 開啟並編輯`react-app/src/components/AdventureDetail.js`
1. 在`AdventureDetail(..)'s`第二個`return(..)`語句前添加以下行，該語句從內容片段路徑派生冒險名。

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

   這會指示`AEMResponsiveGrid`元件從AEM資源擷取其內容：

   + `/content/wknd-app/us/en/home/adventure/${adventureName}/jcr:content/root/responsivegrid`


使用下列行更新`AdventureDetail.js`:

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

## 在AEM中製作容器

在`<AEMResponsiveGrid...>`已就緒且其`pagePath`已根據轉譯的歷程動態設定後，我們會嘗試在其中編寫內容。

1. 登入AEM作者
1. 導覽至「__Sites > WKND應用程式> us > en__」
1. ____ 編輯WKND __應用首__ 頁
   + 導覽至SPA中的&#x200B;__巴釐島衝浪營__&#x200B;路線以編輯它
1. 從右上角的模式選擇器中選擇&#x200B;__預覽__
1. 點選SPA中的&#x200B;__Bali Surf Camp__&#x200B;卡片，導覽至其路徑
1. 從模式選擇器中選擇&#x200B;__編輯__
1. 找到&#x200B;__Layout Container__&#x200B;可編輯區域，緊靠在&#x200B;__Interial__&#x200B;上方
1. 開啟&#x200B;__頁面編輯器的側欄__，然後選取&#x200B;__元件檢視__
1. 將部分已啟用的元件拖曳至&#x200B;__版面容器__
   + 影像
   + 文字
   + 標題

   再製作一些促銷宣傳材料。 看起來可能像這樣：

   ![Bali Adventure Detail Authoring](./assets/spa-dynamic-routes/adventure-detail-edit.png)

1. ____ 在AEM頁面編輯器中預覽您的變更
1. 重新整理在[http://localhost:3000](http://localhost:3000)本機執行的WKND應用程式，導覽至&#x200B;__Bali Surf Camp__&#x200B;路由，以查看撰寫的變更！

   ![遠程SPA Bali](./assets/spa-dynamic-routes/remote-spa-final.png)

導覽至沒有對應AEM頁面的探險詳細路由時，該路由例項將沒有編寫功能。 若要在這些頁面上啟用編寫功能，只需在&#x200B;__Adventure__&#x200B;頁面下建立具有相符名稱的AEM頁面！

## 恭喜！

恭喜！ 您已在SPA中新增動態路由的製作功能！

+ 將AEM React可編輯元件的ResponsiveGrid元件新增至動態路由
+ 建立AEM頁面，以支援SPA（Bali Surf Camp和Beervana，在Portland）中編寫兩條特定路線
+ 在動態的巴釐島衝浪營路線上製作內容！

您現在已完成探索如何使用AEM SPA編輯器將特定可編輯區域新增至遠端SPA的前幾個步驟！


>[!NOTE]
>
>請留意！ 本教學課程將擴充至Adobe的最佳作法和建議，說明如何將SPA Editor解決方案部署至AEM as aCloud Service和生產環境。