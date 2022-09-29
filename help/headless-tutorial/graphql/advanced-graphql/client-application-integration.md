---
title: 用戶端應用程式整合 — AEM無周邊的進階概念 — GraphQL
description: 實作持續的查詢並將其整合至WKND應用程式。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: d0576962-a86a-4742-8635-02be1ec3243f
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1227'
ht-degree: 1%

---

# 客戶端應用程式整合

在上一章中，您使用HTTPPUT和POST要求建立並更新持續查詢。

本章會逐步帶您了解如何在五個React元件中，使用HTTPGET要求將這些保存的查詢與WKND應用程式整合：

* 位置
* 地址
* 講師
* 管理員
* 團隊

## 必備條件 {#prerequisites}

本檔案是多部分教學課程的一部分。 在繼續處理本章之前，請確保已完成前幾章。 完成 [基本教學課程](/help/headless-tutorial/graphql/multi-step/overview.md) 建議使用。

_本章中的IDE螢幕截圖來自 [Visual Studio代碼](https://code.visualstudio.com/)_

### 第1-4章解決方案套件（可選） {#solution-package}

可安裝解決方案套件，以完成AEM UI中章節1-4的步驟。 這個包是 **不需要** 如果先前章節已完成。

1. 下載 [Advanced-GraphQL-Tutorial-Solution-Package-1.1.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Solution-Package-1.1.zip).
1. 在AEM中，導覽至 **工具** > **部署** > **套件** 存取 **封裝管理員**.
1. 上傳並安裝上一步中下載的套件（zip檔案）。

## 目標 {#objectives}

在本教學課程中，您了解如何使用AEM無周邊JavaScript，將持續查詢的請求整合至範例WKND GraphQl React應用程式中 [SDK](https://github.com/adobe/aem-headless-client-js).

## 安裝並運行示例客戶端應用程式 {#install-client-app}

為加速本教學課程，我們提供入門React JS應用程式。

>[!NOTE]
> 
> 以下指示是將React應用程式連線至 **作者** AEMas a Cloud Service環境使用 [本機開發存取權杖](/help/headless-tutorial/authentication/local-development-access-token.md). 您也可以將應用程式連結至 [使用AEMaaCS SDK的本機製作例項](/help/headless-tutorial/graphql/quick-setup/local-sdk.md) 使用基本驗證。

1. 下載 **[aem-guides-wknd-headless-start-tutorial-zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-start-tutorial.zip)**.
1. 解壓縮檔案，然後在IDE中開啟專案。
1. 取得 [本機開發代號](/help/headless-tutorial/authentication/local-development-access-token.md) 的AEM。
1. 在專案中，開啟檔案 `.env.development`.
   1. 設定 `REACT_APP_DEV_TOKEN` 等於 `accessToken` 值。 （不是整個JSON檔案）
   1. 設定 `REACT_APP_HOST_URI` 至AEM的url **作者** 環境。

   ![更新本地環境檔案](assets/client-application-integration/update-environment-file.png)
1. 開啟新終端機並導覽至專案資料夾。 運行以下命令：

   ```shell
   $ npm install
   $ npm start
   ```

1. 新瀏覽器應在 `http://localhost:3000/aem-guides-wknd-pwa`.
1. 點選 **野營** > **約塞米蒂背包** 查看Yosemite Backpacking冒險詳細資訊。

   ![Yosemite回裝螢幕](assets/client-application-integration/yosemite-backpacking-adventure.png)

1. 開啟瀏覽器的開發人員工具，並檢查 `XHR` 請求

   ![POSTGraphQL](assets/client-application-integration/post-query-graphql.png)

   您應會看到 `POST` 到GraphQL端點。 檢視 `Payload`，您就會看到已傳送的完整GraphQL查詢。 在後續章節中，應用程式會更新為使用 **持續** 查詢。


## 快速入門

在基本教學課程中，參數化的GraphQl查詢可用來要求單一內容片段並轉譯冒險詳細資訊。 接下來，更新 `adventureDetailQuery` 包含新欄位，並使用在上一章中建立的持續查詢。

已建立五個元件：

| React元件 | 位置 |
|-------|------|
| 管理員 | `src/components/Administrator.js` |
| 團隊 | `src/components/Team.js` |
| 位置 | `src/components/Location.js` |
| 講師 | `src/components/Instructors.js` |
| 地址 | `src/components/Address.js` |

## 更新useGraphQL掛接

自訂 [反應效果鈎](https://reactjs.org/docs/hooks-overview.html#effect-hook) 建立時會監聽應用程式的變更 `query`，且變更時會向AEM GraphQL端點發出HTTPPOST請求，並傳回JSON回應至應用程式。

建立新的連結以使用 **持續** 查詢。 接著，應用程式就可以提出HTTPGET要求，以取得Adventure詳細資訊。 此 `runPersistedQuery` 的 [AEM Headless用戶端SDK](https://github.com/adobe/aem-headless-client-js) 可用來讓執行持續查詢更輕鬆。

1. 開啟檔案 `src/api/useGraphQL.js`
1. 為 `useGraphQLPersisted`:

   ```javascript
   /**
   * Custom React Hook to perform a GraphQL query to a persisted query endpoint
   * @param persistedPath - the short path to the persisted query
   * @param fragmentPathParam - optional parameters object that can be passed in for parameterized persistent queries
   */
   export function useGraphQLPersisted(persistedPath, fragmentPathVariable) {
       let [data, setData] = useState(null);
       let [errors, setErrors] = useState(null);
   
       useEffect(() => {
           let queryVariables = {};
   
           // we pass in a primitive fragmentPathVariable (String) and then construct the object {fragmentPath: fragmentPathParam} to pass as query params to the persisted query
           // It is simpler to pass a primitive into a React hooks, as comparing the state of a dependent object can be difficult. see https://reactjs.org/docs/hooks-faq.html#can-i-skip-an-effect-on-updates
           if(fragmentPathVariable) {
               queryVariables = {fragmentPath: fragmentPathVariable};
           }
   
           // execute a persisted query using the given path and pass in variables (if needed)
           sdk.runPersistedQuery(persistedPath, queryVariables)
               .then(({ data, errors }) => {
               if (errors) setErrors(mapErrors(errors));
               if (data) setData(data);
           })
           .catch((error) => {
           setErrors(error);
           });
   }, [persistedPath, fragmentPathVariable]);
   
   return { data, errors }
   }
   ```
1. 儲存檔案的變更。

## 更新冒險詳細資訊元件

檔案 `src/api/queries.js` 包含用於支援應用程式的GraphQL查詢 `adventureDetailQuery` 傳回使用標準POSTGraphQL請求之個別冒險的詳細資訊。 接下來，更新 `AdventureDetail` 要使用持續的元件 `wknd/all-adventure-details` 查詢。

1. 開啟 `src/screens/AdventureDetail.js`.
1. 首先，請註解下列行：

   ```javascript
   export default function AdventureDetail() {
   
       ...
   
       //const { data, errors } = useGraphQL(adventureDetailQuery(adventureFragmentPath));
   ```

   以上版本使用標準GraphQLPOST，根據 `adventureFragmentPath`

1. 若要使用 `useGraphQLPersisted` 連結，新增下列行：

   ```javascript
   export default function AdventureDetail() {
   
      //const { data, errors } = useGraphQL(adventureDetailQuery(adventureFragmentPath));
       const {data, errors} = useGraphQLPersisted("wknd/all-adventure-details", adventureFragmentPath);
   ```

   觀察路徑 `wknd/all-adventure-details` 是在上一章中建立的持續查詢的路徑。

   >[!CAUTION]
   >
   > 更新的查詢可運作 `wknd/all-adventure-details` 必須保留在target AEM環境中。 檢閱 [持續GraphQL查詢](/help/headless-tutorial/graphql/advanced-graphql/graphql-persisted-queries.md#cache-control-all-adventures) 或安裝 [AEM解決方案套件](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Solution-Package-1.1.zip)

1. 返回瀏覽器中執行的應用程式，並在導覽至 **冒險詳細資訊** 頁面。

   ![Get要求](assets/client-application-integration/get-request-persisted-query.png)

   ```
   http://localhost:3000/graphql/execute.json/wknd/all-adventure-details;fragmentPath=/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking
   ```

   您現在應該會看到 `GET` 使用持續查詢的請求 `wknd/all-adventure-details`.

1. 導覽至其他探險詳細資訊，並觀察相同 `GET` 請求已提出，但片段路徑不同。 應用程式應能繼續如以前般運作。

請參閱 `AdventureDetail.js` 在 [aem-guides-wknd-headless-solution-tutorial-zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-solution-tutorial.zip) 以取得更新元件的完整範例。

接下來，建立 **位置**, **管理員**，和 **講師** 要呈現位置資料的元件。 此 **地址** 元件是在 **團隊** 元件。

## 開發「位置」元件

1. 在 `AdventureDetail.js` 檔案中，將引用添加到 `<Location>` 元件會將位置資料從 `adventure` 資料物件：

   ```javascript
   export default function AdventureDetail() {
       ...
   
       return (
           ...
   
           <Location data={adventure.location} />
   ```

1. 查看檔案位置： `src/components/Location.js`. 此 `Location` 元件會轉譯要達到的位置、聯絡資訊、天氣資訊，以及來自 **位置** 內容片段模型。 至少， `Location` 元件需要 `address` 要傳遞的物件。
1. 請參閱 `Location.js` 在 [aem-guides-wknd-headless-solution-tutorial-zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-solution-tutorial.zip) 以取得更新元件的完整範例。

更新完成後，轉譯的詳細資料頁面應如下所示：

![位置元件](assets/client-application-integration/location-component.png)

## 開發團隊元件

1. 在 `AdventureDetail.js` 檔案中，將引用添加到 `<Team>` 元件（在下方） `<Location>` 元件)傳遞 `instructorTeam` 資料 `adventure` 資料物件：

   ```javascript
   <Location data={adventure.location} />
   <Team data={adventure.instructorTeam} />
   ```

1. 查看檔案位置： `src/components/Team.js`. 此 `Team` 元件會從 **團隊** 內容片段。

1. 在 `Team.js` 請注意， `Address` 元件。

   ```javascript
   export default function Team({data}) {
       ...
       {teamPath && <Address _path={teamPath}/>}
   ```

   此處會將目前團隊的路徑傳入 `Address` 元件，元件則執行查詢以根據團隊取得地址。

1. 請參閱 `Team.js` 在 [aem-guides-wknd-headless-solution-tutorial-zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-solution-tutorial.zip) 以取得元件的完整範例。

整合查詢後，應如下所示：

![團隊元件](assets/client-application-integration/address-component.png)

## 開發地址元件

1. 查看檔案位置： `src/components/Address.js`. 此 `Address` 元件會轉譯來自的地址資訊，例如街道地址、城市、州、郵遞區號、國家/地區 **地址** 內容片段，以及來自 **聯繫資訊** 片段參考。
1. 此 `Address` 元件類似於 `AdventureDetails` 元件，其會進行持續呼叫，以根據路徑擷取資料。 區別在於它使用 `/wknd/team-location-by-location-path` 來提出要求。
1. 請參閱 `Address.js` 在 [aem-guides-wknd-headless-solution-tutorial-zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-solution-tutorial.zip) 以取得元件的完整範例。

## 開發管理員元件

1. 在 `AdventureDetail.js` 檔案中，將引用添加到 `<Adminstrator>` 元件（在下方） `<Team>` 元件)傳遞 `administrator` 資料 `adventure` 資料物件：

   ```javascript
   <Location data={adventure.location} />
   <Team data={adventure.instructorTeam} />
   <Administrator data={adventure.administrator} /> 
   ```

1. 查看檔案位置： `src/components/Administrator.js`. 此 `Administrator` 元件會從 **管理員** 內容片段，以及從 **聯繫資訊** 片段參考。
1. 請參閱 `Administrator.js` in [aem-guides-wknd-headless-solution-tutorial-zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-solution-tutorial.zip) 以取得元件的完整範例。

建立管理員元件後，即可呈現應用程式。 輸出應符合下列影像：

![管理員元件](assets/client-application-integration/administrator-component.png)

## 開發「講師」元件

1. 在 `AdventureDetail.js` 檔案中，將引用添加到 `<Instructors>` 元件（在下方） `<Administrator>` 元件)傳遞 `instructorTeam` 資料 `adventure` 資料物件：

   ```javascript
   <Location data={adventure.location} />
   <Team data={adventure.instructorTeam}/>
   <Administrator data={adventure.administrator} />             
   <Instructors data={adventure.instructorTeam} />
   ```

1. 查看檔案位置： `src/components/Instructors.js`. 此 `Instructors` 元件會呈現每個團隊成員的相關資料，包括全名、傳記、圖片、電話號碼、體驗層級和技能。 該元件在陣列上反複顯示每個成員。
1. 請參閱 `Instructors.js` in [aem-guides-wknd-headless-solution-tutorial-zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-solution-tutorial.zip) 以取得元件的完整範例。

呈現應用程式後，輸出應符合下列影像：

![講師 — 元件](assets/client-application-integration/instructors-component.png)

## 已完成的示例WKND應用

完成的應用程式應如下所示：

![AEM-headless-final-experience](assets/client-application-integration/aem-headless-final-experience.gif)

### 最終客戶端應用程式

您可下載並使用應用程式的最終版本：
**[aem-guides-wknd-headless-solution-tutorial-zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-solution-tutorial.zip)**

## 恭喜

恭喜！ 您現在已完成整合，並將持續的查詢實作至範例WKND應用程式。
