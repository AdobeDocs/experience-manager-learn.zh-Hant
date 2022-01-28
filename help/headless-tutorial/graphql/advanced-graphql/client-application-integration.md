---
title: 客戶端應用程式整合 — 無頭的AEM高級概念 — GraphQL
description: 實施永續查詢並將其整合到WKND應用中。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
source-git-commit: 83e16ea87847182139982ea2378d8ff9f079c968
workflow-type: tm+mt
source-wordcount: '1228'
ht-degree: 1%

---


# 客戶端應用程式整合

在上一章中，您使用HTTPPUT和POST請求建立和更新了永續查詢。

本章將指導您完成以下步驟：在五個React元件內使用HTTPGET請求將這些永續查詢與WKND應用整合：

* 位置
* 地址
* 指導員
* 管理員
* 團隊

## 必備條件 {#prerequisites}

本文檔是多部分教程的一部分。 在繼續本章之前，請確保已完成前幾章。 完成 [基本教程](/help/headless-tutorial/graphql/multi-step/overview.md) 。

_本章中的IDE螢幕截圖來自 [Visual Studio代碼](https://code.visualstudio.com/)_

### 第1-4章解決方案包（可選） {#solution-package}

可以安裝一個解決方案包，以完成UI中第1AEM-4章的步驟。 此包是 **不需要** 的下界。

1. 下載 [高級 — GraphQL — 教程 — 解決方案 — 軟體包–1.1.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Solution-Package-1.1.zip)。
1. 在AEM中，導航到 **工具** > **部署** > **包** 訪問 **包管理器**。
1. 上載並安裝上一步中下載的包（zip檔案）。

## 目標 {#objectives}

在本教程中，您將瞭解如何使用無頭JavaScript將永續查詢請求整合到示例WKND GraphQl React應AEM用中 [SDK](https://github.com/adobe/aem-headless-client-js)。

## 安裝並運行示例客戶端應用程式 {#install-client-app}

為加速本教程，提供了入門版React JS應用。

>[!NOTE]
> 
> 下面的說明是將React應用連接到 **作者** 在AEMas a Cloud Service使用 [本地開發訪問令牌](/help/headless-tutorial/authentication/local-development-access-token.md)。 還可以將應用連接到 [使用AEMaaCS SDK的本地作者實例](/help/headless-tutorial/graphql/quick-setup/local-sdk.md) 使用基本身份驗證。

1. 下載 **[aem-guides-wknd-headless-start-tutorial.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-start-tutorial.zip)**。
1. 解壓檔案並在IDE中開啟項目。
1. 獲取 [本地開發令牌](/help/headless-tutorial/authentication/local-development-access-token.md) 目標環AEM境。
1. 在項目中，開啟檔案 `.env.development`。
   1. 設定 `REACT_APP_DEV_TOKEN` 等於 `accessToken` 本地開發令牌中的值。 （不是整個JSON檔案）
   1. 設定 `REACT_APP_HOST_URI` 的子例AEM子 **作者** 環境。

   ![更新本地環境檔案](assets/client-application-integration/update-environment-file.png)
1. 開啟新終端並導航到項目資料夾。 運行以下命令：

   ```shell
   $ npm install
   $ npm start
   ```

1. 新瀏覽器應在 `http://localhost:3000/aem-guides-wknd-pwa`。
1. 點擊 **野營** > **約塞米蒂背包** 查看Yosemite Backpacking冒險詳細資訊。

   ![Yosemite背包螢幕](assets/client-application-integration/yosemite-backpacking-adventure.png)

1. 開啟瀏覽器的開發人員工具並檢查 `XHR` 請求

   ![POST圖形QL](assets/client-application-integration/post-query-graphql.png)

   你應該看到 `POST` 到GraphQL端點。 查看 `Payload`，您可以看到已發送的完整GraphQL查詢。 在下一節中，將更新應用以使用 **持久** 的下界。


## 入門

在基本教程中，使用參數化的GraphQl查詢來請求單個內容片段並呈現冒險詳細資訊。 接下來，更新 `adventureDetailQuery` 包含新欄位並使用在上一章中建立的永續查詢。

建立五個元件：

| 反應元件 | 位置 |
|-------|------|
| 管理員 | `src/components/Administrator.js` |
| 團隊 | `src/components/Team.js` |
| 位置 | `src/components/Location.js` |
| 指導員 | `src/components/Instructors.js` |
| 地址 | `src/components/Address.js` |

## 更新useGraphQL掛接

自定義 [反應效果掛鈎](https://reactjs.org/docs/hooks-overview.html#effect-hook) 已建立，用於偵聽對應用的更改 `query`，且更改後向AEMGraphQL終結點發出HTTPPOST請求，並將JSON響應返回到應用。

建立新掛接以使用 **持久** 的下界。 然後，該應用可以發出HTTPGET請求以獲取冒險詳細資訊。 的 `runPersistedQuery` 的 [無AEM頭客戶端SDK](https://github.com/adobe/aem-headless-client-js) 用於使執行永續查詢更容易。

1. 開啟檔案 `src/api/useGraphQL.js`
1. 添加新掛接 `useGraphQLPersisted`:

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
1. 保存對檔案所做的更改。

## 更新冒險詳細資訊元件

檔案 `src/api/queries.js` 包含用於為應用程式提供動力的GraphQL查詢 `adventureDetailQuery` 返回使用標準POSTGraphQL請求的單個冒險的詳細資訊。 接下來，更新 `AdventureDetail` 使用保留的元件 `wknd/all-adventure-details` 的子菜單。

1. 開啟 `src/screens/AdventureDetail.js`.
1. 首先注釋掉以下行：

   ```javascript
   export default function AdventureDetail() {
   
       ...
   
       //const { data, errors } = useGraphQL(adventureDetailQuery(adventureFragmentPath));
   ```

   上面使用標準GraphQLPOST根據 `adventureFragmentPath`

1. 使用 `useGraphQLPersisted` 掛接，添加以下行：

   ```javascript
   export default function AdventureDetail() {
   
      //const { data, errors } = useGraphQL(adventureDetailQuery(adventureFragmentPath));
       const {data, errors} = useGraphQLPersisted("wknd/all-adventure-details", adventureFragmentPath);
   ```

   觀察路徑 `wknd/all-adventure-details` 是上一章中建立的永續查詢的路徑。

   >[!CAUTION]
   >
   > 要使用更新的查詢 `wknd/all-adventure-details` 必須在目標環境上保AEM留。 查看中的步驟 [永續GraphQL查詢](/help/headless-tutorial/graphql/advanced-graphql/graphql-persisted-queries.md#cache-control-all-adventures) 或安裝 [AEM解決方案包](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Solution-Package-1.1.zip)

1. 返回到在瀏覽器中運行的應用，並在導航到 **冒險詳細資訊** 的子菜單。

   ![獲取請求](assets/client-application-integration/get-request-persisted-query.png)

   ```
   http://localhost:3000/graphql/execute.json/wknd/all-adventure-details;fragmentPath=/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking
   ```

   你現在應該看到 `GET` 正在使用永續查詢的請求 `wknd/all-adventure-details`。

1. 導航到其他冒險詳細資訊，並觀察其相同 `GET` 請求是使用不同的片段路徑發出的。 應用程式應繼續像以前一樣工作。

請參閱 `AdventureDetail.js` 的 [aem-guides-wknd-headless-solution-tutorial.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-solution-tutorial.zip) 的子菜單。

接下來，建立 **位置**。 **管理員**, **指導員** 要呈現位置資料的元件。 的 **地址** 元件在 **團隊** 元件。

## 開發位置元件

1. 在 `AdventureDetail.js` 檔案，添加對 `<Location>` 從 `adventure` 資料對象：

   ```javascript
   export default function AdventureDetail() {
       ...
   
       return (
           ...
   
           <Location data={adventure.location} />
   ```

1. 查看檔案位於 `src/components/Location.js`。 的 `Location` 元件將呈現要在何處開會的資料、聯繫資訊、有關天氣的資訊以及來自 **位置** 內容片段模型。 至少， `Location` 元件需要 `address` 要傳遞的對象。
1. 請參閱 `Location.js` 的 [aem-guides-wknd-headless-solution-tutorial.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-solution-tutorial.zip) 的子菜單。

更新完成後，呈現的詳細資訊頁面應如下所示：

![位置分量](assets/client-application-integration/location-component.png)

## 開發團隊元件

1. 在 `AdventureDetail.js` 檔案，添加對 `<Team>` 元件(在 `<Location>` 元件)通過 `instructorTeam` 資料 `adventure` 資料對象：

   ```javascript
   <Location data={adventure.location} />
   <Team data={adventure.instructorTeam} />
   ```

1. 查看檔案位於 `src/components/Team.js`。 的 `Team` 元件從 **團隊** 內容片段。

1. 在 `Team.js` 注意在 `Address` 元件。

   ```javascript
   export default function Team({data}) {
       ...
       {teamPath && <Address _path={teamPath}/>}
   ```

   此時，將傳遞到 `Address` 元件，該元件又執行查詢以基於組獲取地址。

1. 請參閱 `Team.js` 的 [aem-guides-wknd-headless-solution-tutorial.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-solution-tutorial.zip) 的子菜單。

整合查詢後，它應如下所示：

![團隊元件](assets/client-application-integration/address-component.png)

## 開發地址元件

1. 查看檔案位於 `src/components/Address.js`。 的 `Address` 元件提供地址資訊，如街道地址、城市、州、郵遞區號、國家/地區 **地址** 內容片段和來自 **聯繫資訊** 片段引用。
1. 的 `Address` 元件與 `AdventureDetails` 其中，它進行永續調用以基於路徑檢索資料。 區別在於 `/wknd/team-location-by-location-path` 來提出請求。
1. 請參閱 `Address.js` 的 [aem-guides-wknd-headless-solution-tutorial.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-solution-tutorial.zip) 的子菜單。

## 開發管理員元件

1. 在 `AdventureDetail.js` 檔案，添加對 `<Adminstrator>` 元件(在 `<Team>` 元件)通過 `administrator` 資料 `adventure` 資料對象：

   ```javascript
   <Location data={adventure.location} />
   <Team data={adventure.instructorTeam} />
   <Administrator data={adventure.administrator} /> 
   ```

1. 查看檔案位於 `src/components/Administrator.js`。 的 `Administrator` 元件呈現詳細資訊，如其全名 **管理員** 內容片段，並從 **聯繫資訊** 片段引用。
1. 請參閱 `Administrator.js` 在 [aem-guides-wknd-headless-solution-tutorial.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-solution-tutorial.zip) 的子菜單。

建立Administrator元件後，即可呈現應用程式。 輸出應與下面的影像匹配：

![管理員元件](assets/client-application-integration/administrator-component.png)

## 開發「指導員」元件

1. 在 `AdventureDetail.js` 檔案，添加對 `<Instructors>` 元件(在 `<Administrator>` 元件)通過 `instructorTeam` 資料 `adventure` 資料對象：

   ```javascript
   <Location data={adventure.location} />
   <Team data={adventure.instructorTeam}/>
   <Administrator data={adventure.administrator} />             
   <Instructors data={adventure.instructorTeam} />
   ```

1. 查看檔案位於 `src/components/Instructors.js`。 的 `Instructors` 元件呈現有關每個團隊成員的資料，包括全名、傳記、圖片、電話號碼、經驗級別和技能。 該元件在陣列上迭代以顯示每個成員。
1. 請參閱 `Instructors.js` 在 [aem-guides-wknd-headless-solution-tutorial.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-solution-tutorial.zip) 的子菜單。

呈現應用程式後，輸出應與下面的影像匹配：

![輔導器元件](assets/client-application-integration/instructors-component.png)

## 已完成WKND應用示例

完成的應用程式應該如下所示：

![無AEM頭最終體驗](assets/client-application-integration/aem-headless-final-experience.gif)

### 最終客戶端應用程式

該應用的最終版本可以下載和使用：
**[aem-guides-wknd-headless-solution-tutorial.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-solution-tutorial.zip)**

## 恭喜

恭喜！ 您現在已完成將永續查詢整合併實施到示例WKND應用中。