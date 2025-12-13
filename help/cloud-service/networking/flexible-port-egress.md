---
title: 彈性連接埠輸出
description: 瞭解如何設定和使用彈性的連線埠輸出，以支援從AEM as a Cloud Service到外部服務的外部連線。
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
jira: KT-9350
thumbnail: KT-9350.jpeg
exl-id: 5c1ff98f-d1f6-42ac-a5d5-676a54ef683c
last-substantial-update: 2024-04-26T00:00:00Z
duration: 870
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '1275'
ht-degree: 2%

---

# 彈性連接埠輸出

瞭解如何設定和使用彈性的連線埠輸出，以支援從AEM as a Cloud Service到外部服務的外部連線。

## 什麼是彈性連線埠輸出？

彈性的連線埠輸出可讓自訂、特定的連線埠轉送規則附加至AEM as a Cloud Service，以便建立從AEM到外部服務的連線。

Cloud Manager程式只能有&#x200B;__單一__&#x200B;網路基礎結構型別。 在執行下列命令之前，請確定彈性連線埠輸出是您AEM as a Cloud Service最[適當的網路基礎建設型別](./advanced-networking.md)。

>[!MORELIKETHIS]
>
> 閱讀AEM as a Cloud Service [進階網路組態檔案](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking)，以取得有關彈性連線埠輸出的詳細資訊。


## 先決條件

使用Cloud Manager API設定或設定彈性連線埠輸出時，需要下列專案：

+ 已啟用Cloud Manager API且[Cloud Manager企業所有者許可權的Adobe Developer Console專案](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/permissions/)
+ 存取[Cloud Manager API的驗證認證](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/create-api-integration/)
   + 組織ID （亦稱為IMS組織ID）
   + 使用者端ID （亦稱為API金鑰）
   + 存取權杖（亦稱為持有人權杖）
+ Cloud Manager計畫ID
+ Cloud Manager環境ID

如需詳細資訊，[請檢閱如何設定、設定和取得Cloud Manger API認證](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/developing/extensibility/app-builder/server-to-server-auth)，以使用這些認證進行Cloud Manager API呼叫。

本教學課程使用`curl`來進行Cloud Manager API設定。 提供的`curl`命令採用Linux/macOS語法。 如果使用Windows命令提示字元，請將`\`分行符號取代為`^`。


## 為每個程式啟用彈性連線埠輸出

首先在AEM as a Cloud Service上啟用彈性的連線埠輸出。

>[!BEGINTABS]

>[!TAB Cloud Manager]

彈性連線埠輸出可使用Cloud Manager啟用。 下列步驟概述如何使用Cloud Manager在AEM as a Cloud Service上啟用彈性連線埠輸出。

1. 以Cloud Manager業務負責人身分登入[Adobe Experience Manager Cloud Manager](https://experience.adobe.com/cloud-manager/)。
1. 導覽至所需的計畫。
1. 在左側功能表中，瀏覽至&#x200B;__服務>網路基礎結構__。
1. 選取&#x200B;__新增網路基礎結構__&#x200B;按鈕。

   ![新增網路基礎結構](./assets/cloud-manager__add-network-infrastructure.png)

1. 在&#x200B;__新增網路基礎結構__&#x200B;對話方塊中，選取&#x200B;__彈性連線埠輸出__&#x200B;選項，然後選取&#x200B;__區域__&#x200B;以建立專用輸出IP位址。

   ![新增彈性連線埠輸出](./assets/flexible-port-egress/select-type.png)

1. 選取&#x200B;__儲存__&#x200B;以確認新增彈性連線埠輸出。

   ![確認彈性連線埠輸出建立](./assets/flexible-port-egress/confirmation.png)

1. 等候網路基礎結構建立並標示為&#x200B;__就緒__。 此程式最多可能需要1小時。

   ![彈性連線埠輸出建立狀態](./assets/flexible-port-egress/ready.png)

建立彈性連線埠輸出後，您現在可以使用Cloud Manager API設定連線埠轉送規則，如下所述。

>[!TAB Cloud Manager API]

彈性連線埠輸出可使用Cloud Manager API來啟用。 下列步驟概述如何使用Cloud Manager API在AEM as a Cloud Service上啟用彈性連線埠輸出。

1. 首先，使用Cloud Manager API [listRegions](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/)作業，判斷在中設定進階網路的地區。 進行後續Cloud Manager API呼叫需要`region name`。 通常會使用生產環境所在的區域。

   在[環境的詳細資料](https://my.cloudmanager.adobe.com)底下的[Cloud Manager](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments)中尋找您的AEM as a Cloud Service環境地區。 Cloud Manager中顯示的地區名稱可以[對應到Cloud Manager API中使用的地區代碼](https://developer.adobe.com/experience-cloud/cloud-manager/guides/api-usage/creating-programs-and-environments/#creating-aem-cloud-service-environments)。

   __listRegions HTTP要求__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' 
   ```

2. 使用Cloud Manager API [createNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/)作業，為Cloud Manager程式啟用彈性的連線埠輸出。 使用從Cloud Manager API `region`作業取得的適當`listRegions`程式碼。

   __createNetworkInfrastructure HTTP要求__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d '{ "kind": "flexiblePortEgress", "region": "va7" }'
   ```

   等待15分鐘，讓Cloud Manager程式布建網路基礎結構。

3. 檢查環境是否已使用先前步驟中從&#x200B;__HTTP要求傳回的__，使用Cloud Manager API [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure)作業，完成`id`彈性連線埠輸出`createNetworkInfrastructure`設定。

   __getNetworkInfrastructure HTTP要求__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   確認HTTP回應包含&#x200B;__就緒__&#x200B;的&#x200B;__狀態__。 如果尚未準備就緒，請每隔幾分鐘重新檢查一次狀態。

建立彈性連線埠輸出後，您現在可以使用Cloud Manager API設定連線埠轉送規則，如下所述。

>[!ENDTABS]

## 為每個環境設定彈性的連線埠輸出代理

1. 使用Cloud Manager API __enableEnvironmentAdvancedNetworkingConfiguration__&#x200B;作業，在每個AEM as a Cloud Service環境中啟用並設定[彈性連線埠輸出](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/)設定。

   __enableEnvironmentAdvancedNetworkingConfiguration HTTP要求__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./flexible-port-egress.json
   ```

   在`flexible-port-egress.json`中定義JSON引數，並透過`... -d @./flexible-port-egress.json`提供給curl。

   [下載範例flexible-port-egress.json](./assets/flexible-port-egress.json)。 這個檔案只是一個範例。 根據[enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/)中記錄的選用/必要欄位，視需要設定您的檔案。

   ```json
   {
       "portForwards": [
           {
               "name": "mysql.example.com",
               "portDest": 3306,
               "portOrig": 30001
           },
           {
               "name": "smtp.sendgrid.com",
               "portDest": 465,
               "portOrig": 30002
           }
       ]
   }
   ```

   對於每個`portForwards`對應，進階網路會定義下列轉送規則：

   | Proxy主機 | Proxy連線埠 |  | 外部主機 | 外部連線埠 |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

   如果您的AEM部署&#x200B;__僅__&#x200B;需要外部服務的HTTP/HTTPS連線（連線埠80/443），請將`portForwards`陣列保留空白，因為這些規則僅對非HTTP/HTTPS要求是必要的。

1. 對於每個環境，請使用Cloud Manager API [getEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/)作業來驗證輸出規則是否有效。

   __getEnvironmentAdvancedNetworkingConfiguration HTTP要求__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Content-Type: application/json'
   ```

1. 可以使用Cloud Manager API [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/)作業更新彈性的連線埠輸出設定。 請記住，`enableEnvironmentAdvancedNetworkingConfiguration`是`PUT`作業，因此每次呼叫此作業時，都必須提供所有規則。

1. 現在，您可以在自訂AEM程式碼和設定中使用彈性的連線埠輸出設定。


## 透過彈性連線埠輸出連線至外部服務

啟用彈性連線埠輸出Proxy後，AEM程式碼和設定便可使用它們來呼叫外部服務。 AEM對外部呼叫的處理方式有所差異，共有兩種型別：

1. 對非標準連線埠上的外部服務進行HTTP/HTTPS呼叫
   + 包括對標準80或443連線埠以外的連線埠上執行的服務發出的HTTP/HTTPS呼叫。
1. 對外部服務的非HTTP/HTTPS呼叫
   + 包括任何非HTTP呼叫，例如與郵件伺服器、SQL資料庫或服務之間的連線，這些服務會在其他非HTTP/HTTPS通訊協定上執行。

預設允許來自標準連線埠(80/443)上AEM的HTTP/HTTPS請求，且不需要額外的設定或考量。


### 非標準連線埠上的HTTP/HTTPS

從AEM建立與非標準連線埠（非80/443）的HTTP/HTTPS連線時，必須透過提供預留位置的特殊主機和連線埠進行連線。

AEM提供兩組特殊的Java™系統變數，這些變數對應至AEM的HTTP/HTTPS代理程式。

| 變數名稱 | 使用 | Java™程式碼 | OSGi設定 |
| - |  - | - | - |
| `AEM_PROXY_HOST` | 兩個HTTP/HTTPS連線的Proxy主機 | `System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel")` | `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` |
| `AEM_HTTP_PROXY_PORT` | HTTPS連線的Proxy連線埠（設定遞補為`3128`） | `System.getenv().getOrDefault("AEM_HTTP_PROXY_PORT", 3128)` | `$[env:AEM_HTTP_PROXY_PORT;default=3128]` |
| `AEM_HTTPS_PROXY_PORT` | HTTPS連線的Proxy連線埠（設定遞補為`3128`） | `System.getenv().getOrDefault("AEM_HTTPS_PROXY_PORT", 3128)` | `$[env:AEM_HTTPS_PROXY_PORT;default=3128]` |

對非標準連線埠上的外部服務進行HTTP/HTTPS呼叫時，必須使用Cloud Manager API `portForwards`作業定義沒有對應的`enableEnvironmentAdvancedNetworkingConfiguration`，因為連線埠轉送「規則」是在「程式碼」中定義。

>[!TIP]
>
> 請參閱AEM as a Cloud Service的彈性連線埠輸出檔案，以取得[完整的路由規則集](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking)。

#### 程式碼範例

<table>
<tr>
<td>
    <a  href="./examples/http-on-non-standard-ports-flexible-port-egress.md"><img alt="非標準連線埠上的HTTP/HTTPS" src="./assets/code-examples__http.png"/></a>
    <div>非標準連線埠上的<strong><a href="./examples/http-on-non-standard-ports-flexible-port-egress.md">HTTP/HTTPS</a></strong></div>
    <p>
        Java™程式碼範例，用於從AEM as a Cloud Service透過非標準HTTP/HTTPS連線埠將HTTP/HTTPS連線至外部服務。
    </p>
</td>   
<td></td>   
<td></td>   
</tr>
</table>

### 與外部服務的非HTTP/HTTPS連線

建立非HTTP/HTTPS連線時(例如 AEM SQL、SMTP等)，必須透過AEM提供的特殊主機名稱進行連線。

| 變數名稱 | 使用 | Java™程式碼 | OSGi設定 |
| - |  - | - | - |
| `AEM_PROXY_HOST` | 非HTTP/HTTPS連線的Proxy主機 | `System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel")` | `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` |


接著會透過`AEM_PROXY_HOST`與對應的連線埠(`portForwards.portOrig`)呼叫與外部服務的連線，AEM會接著路由至對應的外部主機名稱(`portForwards.name`)與連線埠(`portForwards.portDest`)。

| Proxy主機 | Proxy連線埠 |  | 外部主機 | 外部連線埠 |
|---------------------------------|----------|----------------|------------------|----------|
| `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

#### 程式碼範例

<table><tr>
   <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="使用JDBC DataSourcePool的SQL連線" src="./assets/code-examples__sql-osgi.png"/></a>
      <div>使用JDBC DataSourcePool的<strong><a href="./examples/sql-datasourcepool.md">SQL連線</a></strong></div>
      <p>
            Java™程式碼範例透過設定AEM的JDBC資料來源集區來連線到外部SQL資料庫。
      </p>
    </td>   
   <td>
      <a  href="./examples/sql-java-apis.md"><img alt="使用Java API的SQL連線" src="./assets/code-examples__sql-java-api.png"/></a>
      <div>使用Java™ API的<strong><a href="./examples/sql-java-apis.md">SQL連線</a></strong></div>
      <p>
            Java™程式碼範例使用Java™的SQL API連線至外部SQL資料庫。
      </p>
    </td>   
   <td>
      <a  href="./examples/email-service.md"><img alt="虛擬私人網路 (VPN)" src="./assets/code-examples__email.png"/></a>
      <div><strong><a href="./examples/email-service.md">電子郵件服務</a></strong></div>
      <p>
        使用AEM連線至外部電子郵件服務的OSGi設定範例。
      </p>
    </td>   
</tr></table>
