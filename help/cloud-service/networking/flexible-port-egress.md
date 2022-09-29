---
title: 靈活的埠輸出
description: 了解如何設定和使用彈性的埠輸出，以支援從AEMas a Cloud Service到外部服務的外部連接。
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9350
thumbnail: KT-9350.jpeg
exl-id: 5c1ff98f-d1f6-42ac-a5d5-676a54ef683c
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1076'
ht-degree: 4%

---

# 靈活的埠輸出

了解如何設定和使用彈性的埠輸出，以支援從AEMas a Cloud Service到外部服務的外部連接。

## 什麼是靈活埠輸出？

靈活的埠輸出允許將自定義的特定埠轉發規則連接到AEMas a Cloud Service，從而允許從AEM連接到外部服務。

Cloud Manager程式只能有 __單一__ 網路基礎結構類型。 請確保專用的輸出IP位址是 [適當類型的網路基礎設施](./advanced-networking.md)  以取得AEMas a Cloud Service。

>[!MORELIKETHIS]
>
> 閱讀AEMas a Cloud Service [高級網路配置文檔](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#flexible-port-egress) 有關靈活埠輸出的更多詳細資訊。

## 必備條件

設定靈活的埠輸出時，需要以下操作：

+ Adobe Developer Console專案，並啟用Cloud Manager API，同時 [Cloud Manager業務擁有者權限](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/permissions/)
+ 存取 [Cloud Manager API的驗證憑證](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/authentication/)
   + 組織ID（亦稱為IMS組織ID）
   + 用戶端ID（亦稱為API金鑰）
   + 存取權杖（亦稱為承載權杖）
+ Cloud Manager方案ID
+ Cloud Manager環境ID

如需詳細資訊，請觀看下列逐步說明，了解如何設定、設定及取得Cloud Manager API憑證，以及如何使用這些憑證來進行Cloud Manager API呼叫。

>[!VIDEO](https://video.tv.adobe.com/v/342235/?quality=12&learn=on)

本教學課程使用 `curl` 來設定Cloud Manager API。 提供的 `curl` 命令採用Linux/macOS語法。 如果使用Windows命令提示符，請替換 `\` 斷行字元，帶 `^`.

## 為每個程式啟用靈活的埠輸出

首先，在AEMas a Cloud Service上啟用彈性的埠輸出。

1. 首先，使用Cloud Manager API確定中的「進階網路」設定區域 [listRegions](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作。 此 `region name` 必要，才能進行後續的Cloud Manager API呼叫。 通常會使用生產環境所在的地區。

   __listRegions HTTP要求__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' 
   ```

1. 使用Cloud Manager API為Cloud Manager程式啟用彈性的埠輸出 [createNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作。 使用適當 `region` 從Cloud Manager API取得的程式碼 `listRegions` 操作。

   __createNetworkInfrastructure HTTP請求__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d '{ "kind": "flexiblePortEgress", "region": "va7" }'
   ```

   等待15分鐘，讓Cloud Manager計畫配置網路基礎架構。

1. 檢查環境是否已完成 __靈活的埠輸出__ 使用Cloud Manager API進行設定 [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) 操作，使用 `id` 從上一步的createNetworkInfrastructure HTTP請求中傳回。

   __getNetworkInfrastructure HTTP請求__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   確認HTTP回應包含 __狀態__ of __就緒__. 如果尚未準備好，請每隔幾分鐘重新檢查一次狀態。

## 根據環境配置靈活的埠輸出代理

1. 啟用並設定 __靈活的埠輸出__ 使用Cloud Manager API在每個AEMas a Cloud Service環境上進行設定 [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作。

   __enableEnvironmentAdvancedNetworkingConfiguration HTTP請求__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./flexible-port-egress.json
   ```

   在 `flexible-port-egress.json` 並提供通過 `... -d @./flexible-port-egress.json`.

   [下載範例flexible-port-eogress.json](./assets/flexible-port-egress.json). 此檔案只是範例。 根據上述選填/必填欄位，視需要設定檔案 [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/).

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

   針對每個 `portForwards` 映射時，高級網路定義以下轉發規則：

   | 代理主機 | 代理埠 |  | 外部主機 | 外部埠 |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

   如果您的AEM部署 __僅限__ 需要HTTP/HTTPS連線(連接埠80/443)才能連線外部服務，請離開 `portForwards` 陣列空白，因為這些規則僅對非HTTP/HTTPS要求是必要的。

1. 對於每個環境，請使用Cloud Manager API驗證輸出規則是否有效 [getEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作。

   __getEnvironmentAdvancedNetworkingConfiguration HTTP請求__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Content-Type: application/json'
   ```

1. 使用Cloud Manager API可更新彈性的埠輸出設定 [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作。 記住 `enableEnvironmentAdvancedNetworkingConfiguration` 是 `PUT` 操作，因此所有規則都必須隨此操作的每次調用一起提供。

1. 現在，您可以在自訂AEM程式碼和設定中使用彈性的連接埠輸出設定。


## 通過靈活的埠輸出連接到外部服務

啟用彈性的埠輸出代理後，AEM程式碼和設定便可使用它們來呼叫外部服務。 有兩種外部呼叫的處理方式不同：

1. 非標準埠上對外部服務的HTTP/HTTPS呼叫
   + 包括對在標準80或443埠以外的埠上運行的服務進行的HTTP/HTTPS呼叫。
1. 對外部服務的非HTTP/HTTPS呼叫
   + 包括任何非HTTP呼叫，例如與Mail伺服器、SQL資料庫或在其他非HTTP/HTTPS通訊協定上執行的服務的連線。

預設允許來自標準埠(80/443)上AEM的HTTP/HTTPS要求，且不需要額外的設定或考量。


### 非標準埠上的HTTP/HTTPS

從AEM建立HTTP/HTTPS連線至非標準埠(非-80/443)時，必須透過特殊主機和埠進行連線，並透過預留位置提供。

AEM提供兩組對應至AEM HTTP/HTTPS代理的特殊Java™系統變數。

|變數名稱 |使用 | Java™程式碼 | OSGi配置 | | - | - | - | - | | `AEM_PROXY_HOST` |兩個HTTP/HTTPS連接的代理主機 | `System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel")` | `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` | | `AEM_HTTP_PROXY_PORT` | HTTPS連接的代理埠(設為後援 `3128`) | `System.getenv().getOrDefault("AEM_HTTP_PROXY_PORT", 3128)` | `$[env:AEM_HTTP_PROXY_PORT;default=3128]` | | `AEM_HTTPS_PROXY_PORT` | HTTPS連接的代理埠(設為後援 `3128`) | `System.getenv().getOrDefault("AEM_HTTPS_PROXY_PORT", 3128)` | `$[env:AEM_HTTPS_PROXY_PORT;default=3128]` |

在非標準埠上對外部服務進行HTTP/HTTPS呼叫時，沒有對應的 `portForwards` 必須使用Cloud Manager API來定義 `enableEnvironmentAdvancedNetworkingConfiguration` 操作，因為埠轉送「規則」定義為「在代碼中」。

>[!TIP]
>
> 請參閱AEMas a Cloud Service的彈性埠輸出檔案，以了解 [完整的路由規則集](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#flexible-port-egress-traffic-routing).

#### 程式碼範例

<table>
<tr>
<td>
    <a  href="./examples/http-on-non-standard-ports-flexible-port-egress.md"><img alt="非標準埠上的HTTP/HTTPS" src="./assets/code-examples__http.png"/></a>
    <div><strong><a href="./examples/http-on-non-standard-ports-flexible-port-egress.md">非標準埠上的HTTP/HTTPS</a></strong></div>
    <p>
        Java™程式碼範例，讓從AEMas a Cloud Service的HTTP/HTTPS連線至非標準HTTP/HTTPS埠上的外部服務。
    </p>
</td>   
<td></td>   
<td></td>   
</tr>
</table>

### 與外部服務的非HTTP/HTTPS連線

建立非HTTP/HTTPS連線時(例如 SQL、SMTP等)，必須透過AEM提供的特殊主機名稱進行連線。

|變數名稱 |使用 | Java™程式碼 | OSGi配置 | | - | - | - | - | | `AEM_PROXY_HOST` |非HTTP/HTTPS連線的代理主機 | `System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel")` | `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` |


然後，會透過 `AEM_PROXY_HOST` 和映射埠(`portForwards.portOrig`),AEM接著路由至對應的外部主機名稱(`portForwards.name`)和埠(`portForwards.portDest`)。

| 代理主機 | 代理埠 |  | 外部主機 | 外部埠 |
|---------------------------------|----------|----------------|------------------|----------|
| `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

#### 程式碼範例

<table><tr>
   <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="使用JDBC DataSourcePool的SQL連接" src="./assets/code-examples__sql-osgi.png"/></a>
      <div><strong><a href="./examples/sql-datasourcepool.md">使用JDBC DataSourcePool的SQL連接</a></strong></div>
      <p>
            通過配置AEM JDBC資料源池連接到外部SQL資料庫的Java™代碼示例。
      </p>
    </td>   
   <td>
      <a  href="./examples/sql-java-apis.md"><img alt="使用Java API的SQL連接" src="./assets/code-examples__sql-java-api.png"/></a>
      <div><strong><a href="./examples/sql-java-apis.md">使用Java™ API的SQL連接</a></strong></div>
      <p>
            使用Java™的SQL API連接到外部SQL資料庫的Java™代碼示例。
      </p>
    </td>   
   <td>
      <a  href="./examples/email-service.md"><img alt="虛擬專用網(VPN)" src="./assets/code-examples__email.png"/></a>
      <div><strong><a href="./examples/email-service.md">電子郵件服務</a></strong></div>
      <p>
        OSGi設定範例，使用AEM連線至外部電子郵件服務。
      </p>
    </td>   
</tr></table>
