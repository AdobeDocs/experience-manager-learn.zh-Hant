---
title: 彈性的連線埠輸出
description: 瞭解如何設定和使用彈性的連線埠輸出，以支援從AEMas a Cloud Service到外部服務的外部連線。
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9350
thumbnail: KT-9350.jpeg
exl-id: 5c1ff98f-d1f6-42ac-a5d5-676a54ef683c
duration: 906
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '1060'
ht-degree: 1%

---

# 彈性的連線埠輸出

瞭解如何設定和使用彈性的連線埠輸出，以支援從AEMas a Cloud Service到外部服務的外部連線。

## 什麼是彈性連線埠輸出？

彈性的連線埠輸出可讓自訂、特定的連線埠轉送規則附加至AEMas a Cloud Service，以便建立從AEM到外部服務的連線。

Cloud Manager計畫只能有 __單一__ 網路基礎架構型別。 確保專用輸出IP位址為最大 [適當型別的網路基礎結構](./advanced-networking.md)  ，然後才可執行AEMas a Cloud Service的命令。

>[!MORELIKETHIS]
>
> 閱讀AEMas a Cloud Service [進階網路設定檔案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#flexible-port-egress) 以取得有關彈性連線埠出口的詳細資訊。

## 先決條件

設定彈性連線埠輸出時需要下列專案：

+ Adobe Developer Console專案，已啟用Cloud Manager API並 [Cloud Manager企業所有者許可權](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/permissions/)
+ 存取目標 [Cloud Manager API的驗證認證](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/authentication/)
   + 組織ID （亦稱為IMS組織ID）
   + 使用者端ID （亦稱為API金鑰）
   + 存取權杖（亦稱為持有人權杖）
+ Cloud Manager計畫ID
+ Cloud Manager環境ID

如需更多詳細資訊，請觀看以下逐步解說，瞭解如何設定、設定和取得Cloud Manager API認證，以及如何使用這些認證進行Cloud Manager API呼叫。

>[!VIDEO](https://video.tv.adobe.com/v/342235?quality=12&learn=on)

本教學課程使用 `curl` 以進行Cloud Manager API設定。 提供的 `curl` 命令採用Linux/macOS語法。 如果使用Windows命令提示字元，請將 `\` 換行字元 `^`.

## 為每個程式啟用彈性連線埠輸出

首先在AEMas a Cloud Service上啟用彈性的連線埠輸出。

1. 首先，使用Cloud Manager API確定在中設定進階網路的地區 [listRegions](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 作業。 此 `region name` 進行後續Cloud Manager API呼叫所必需。 通常會使用生產環境所在的區域。

   尋找您的AEMas a Cloud Service環境所在地區 [Cloud Manager](https://my.cloudmanager.adobe.com) 在 [環境的詳細資訊](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html?lang=en#viewing-environment). Cloud Manager中顯示的區域名稱可以是 [已對應至地區碼](https://developer.adobe.com/experience-cloud/cloud-manager/guides/api-usage/creating-programs-and-environments/#creating-aem-cloud-service-environments) 用於Cloud Manager API。

   __listRegions HTTP要求__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' 
   ```

1. 使用Cloud Manager API為Cloud Manager計畫啟用靈活的連線埠輸出 [createNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 作業。 使用適當的 `region` 從Cloud Manager API取得的程式碼 `listRegions` 作業。

   __createNetworkInfrastructure HTTP要求__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d '{ "kind": "flexiblePortEgress", "region": "va7" }'
   ```

   等待15分鐘，讓Cloud Manager計畫布建網路基礎結構。

1. 檢查環境是否已完成 __彈性連線埠輸出__ 使用Cloud Manager API進行設定 [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) 作業，使用 `id` 從先前步驟的createNetworkInfrastructure HTTP要求傳回。

   __getNetworkInfrastructure HTTP要求__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   確認HTTP回應包含 __狀態__ 之 __就緒__. 如果尚未準備就緒，請每隔幾分鐘重新檢查一次狀態。

## 為每個環境設定彈性的連線埠輸出代理

1. 啟用並設定 __彈性連線埠輸出__ 使用Cloud Manager API在每個AEMas a Cloud Service環境中進行設定 [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 作業。

   __enableEnvironmentAdvancedNetworkingConfiguration HTTP要求__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./flexible-port-egress.json
   ```

   在中定義JSON引數 `flexible-port-egress.json` 並提供給curl，透過 `... -d @./flexible-port-egress.json`.

   [下載範例flexible-port-egress.json](./assets/flexible-port-egress.json). 這個檔案只是一個範例。 根據以下網址記錄的選用/必要欄位，依需求設定您的檔案： [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/).

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

   針對每個 `portForwards` 對應，進階網路會定義下列轉送規則：

   | Proxy主機 | Proxy連線埠 |  | 外部主機 | 外部連線埠 |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

   如果您的AEM部署 __僅限__ 需要外部服務的HTTP/HTTPS連線（連線埠80/443），請離開 `portForwards` 陣列空白，因為只有非HTTP/HTTPS請求才需要這些規則。

1. 對於每個環境，使用Cloud Manager API驗證輸出規則是否有效 [getEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 作業。

   __getEnvironmentAdvancedNetworkingConfiguration HTTP要求__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Content-Type: application/json'
   ```

1. 彈性的連線埠輸出設定可以使用Cloud Manager API更新 [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 作業。 記住 `enableEnvironmentAdvancedNetworkingConfiguration` 是 `PUT` 作業，因此每次呼叫此作業時，都必須提供所有規則。

1. 現在您可以在自訂AEM程式碼和設定中使用彈性的連線埠輸出設定。


## 透過彈性連線埠輸出連線至外部服務

啟用彈性連線埠輸出Proxy後，AEM程式碼和設定便可使用它們來呼叫外部服務。 AEM處理外部呼叫的方式有兩種：

1. 對非標準連線埠上的外部服務進行HTTP/HTTPS呼叫
   + 包括對標準80或443連線埠以外的連線埠上執行的服務發出的HTTP/HTTPS呼叫。
1. 對外部服務的非HTTP/HTTPS呼叫
   + 包括任何非HTTP呼叫，例如與郵件伺服器、SQL資料庫或服務之間的連線，這些服務會在其他非HTTP/HTTPS通訊協定上執行。

預設允許來自標準連線埠(80/443)上AEM的HTTP/HTTPS請求，且不需要額外的設定或考量。


### 非標準連線埠上的HTTP/HTTPS

從AEM建立與非標準連線埠（非80/443）的HTTP/HTTPS連線時，必須透過特殊主機和連線埠（透過預留位置提供）進行連線。

AEM提供兩組對映至AEM HTTP/HTTPS代理的特殊Java™系統變數。

| 變數名稱 | 使用 | Java™程式碼 | OSGi設定 |
| - |  - | - | - |
| `AEM_PROXY_HOST` | 兩個HTTP/HTTPS連線的Proxy主機 | `System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel")` | `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` |
| `AEM_HTTP_PROXY_PORT` | HTTPS連線的Proxy連線埠(設定為遞補為 `3128`) | `System.getenv().getOrDefault("AEM_HTTP_PROXY_PORT", 3128)` | `$[env:AEM_HTTP_PROXY_PORT;default=3128]` |
| `AEM_HTTPS_PROXY_PORT` | HTTPS連線的Proxy連線埠(設定為遞補為 `3128`) | `System.getenv().getOrDefault("AEM_HTTPS_PROXY_PORT", 3128)` | `$[env:AEM_HTTPS_PROXY_PORT;default=3128]` |

對非標準連線埠上的外部服務進行HTTP/HTTPS呼叫時，沒有對應的 `portForwards` 必須使用Cloud Manager API定義 `enableEnvironmentAdvancedNetworkingConfiguration` 作業，因為連線埠轉送「規則」定義為「在程式碼中」。

>[!TIP]
>
> 請參閱AEMas a Cloud Service的彈性連線埠輸出檔案，以瞭解 [完整的路由規則集](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#flexible-port-egress-traffic-routing).

#### 程式碼範例

<table>
<tr>
<td>
    <a  href="./examples/http-on-non-standard-ports-flexible-port-egress.md"><img alt="非標準連線埠上的HTTP/HTTPS" src="./assets/code-examples__http.png"/></a>
    <div><strong><a href="./examples/http-on-non-standard-ports-flexible-port-egress.md">非標準連線埠上的HTTP/HTTPS</a></strong></div>
    <p>
        Java™程式碼範例，讓從AEM的HTTP/HTTPS連線在非標準HTTP/HTTPS連線埠上as a Cloud Service到外部服務。
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


然後，會透過呼叫外部服務的連線。 `AEM_PROXY_HOST` 和對應的連線埠(`portForwards.portOrig`)，則AEM會路由至對應的外部主機名稱(`portForwards.name`)和連線埠(`portForwards.portDest`)。

| Proxy主機 | Proxy連線埠 |  | 外部主機 | 外部連線埠 |
|---------------------------------|----------|----------------|------------------|----------|
| `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

#### 程式碼範例

<table><tr>
   <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="使用JDBC DataSourcePool的SQL連線" src="./assets/code-examples__sql-osgi.png"/></a>
      <div><strong><a href="./examples/sql-datasourcepool.md">使用JDBC DataSourcePool的SQL連線</a></strong></div>
      <p>
            Java™程式碼範例透過設定AEM JDBC資料來源集區來連線到外部SQL資料庫。
      </p>
    </td>   
   <td>
      <a  href="./examples/sql-java-apis.md"><img alt="使用Java API的SQL連線" src="./assets/code-examples__sql-java-api.png"/></a>
      <div><strong><a href="./examples/sql-java-apis.md">使用Java™ API的SQL連線</a></strong></div>
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
