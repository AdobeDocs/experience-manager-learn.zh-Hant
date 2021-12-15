---
title: 虛擬專用網(VPN)
description: 了解如何將AEMas a Cloud Service與您的VPN連接，以在AEM與內部服務之間建立安全的通訊通道。
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9352
thumbnail: KT-9352.jpeg
exl-id: 74cca740-bf5e-4cbd-9660-b0579301a3b4
source-git-commit: ba2c299baeda632d6ebeff0c6ee07de5ef29b9cb
workflow-type: tm+mt
source-wordcount: '1259'
ht-degree: 0%

---

# 虛擬專用網(VPN)

了解如何將AEMas a Cloud Service與您的VPN連接，以在AEM與內部服務之間建立安全的通訊通道。

## 什麼是虛擬專用網路？

虛擬專用網路(VPN)可讓AEMas a Cloud Service客戶將Cloud Manager程式連結至現有 [受支援](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#vpn) VPN。 這可讓AEMas a Cloud Service與客戶網路內的服務之間安全且受控的連線。

Cloud Manager程式只能有 __單一__ 網路基礎結構類型。 確保虛擬專用網路是 [適當類型的網路基礎設施](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#general-vpn-considerations) 以取得AEMas a Cloud Service。

>[!MORELIKETHIS]
>
> 閱讀AEMas a Cloud Service [高級網路配置文檔](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#vpn) 有關虛擬專用網路的詳細資訊。

## 必備條件

設定虛擬專用網路時，需要下列項目：

+ Adobe帳戶 [Cloud Manager業務擁有者權限](https://www.adobe.io/experience-cloud/cloud-manager/guides/getting-started/permissions/#cloud-manager-api-permissions)
+ 存取 [Cloud Manager API的驗證憑證](https://www.adobe.io/experience-cloud/cloud-manager/guides/getting-started/authentication/)
   + 組織ID（亦稱為IMS組織ID）
   + 用戶端ID（亦稱為API金鑰）
   + 存取權杖（亦稱為承載權杖）
+ Cloud Manager方案ID
+ Cloud Manager環境ID
+ 虛擬專用網路，可訪問所有必要的連接參數。

本教學課程使用 `curl` 來設定Cloud Manager API。 提供的 `curl` 命令採用Linux/macOS語法。 如果使用Windows命令提示符，請替換 `\` 斷行字元，帶 `^`.

## 按程式啟用虛擬專用網路

首先，在AEMas a Cloud Service上啟用虛擬專用網路。

1. 首先，使用Cloud Manager API確定要設定進階網路的區域 [listRegions](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/getProgramRegions) 操作。 此 `region name` 後續進行Cloud Manager API呼叫時，將需要用到ID服務。 通常會使用生產環境所在的地區。

   __listRegions HTTP要求__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. 使用Cloud Manager API為Cloud Manager程式啟用虛擬專用網路 [createNetworkInfrastructure](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/createNetworkInfrastructure) 操作。 使用適當 `region` 從Cloud Manager API取得的程式碼 `listRegions` 操作。

   __createNetworkInfrastructure HTTP請求__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
       -d @./vpn-create.json
   ```

   在 `vpn-create.json` 並提供通過 `... -d @./vpn-create.json`.

[下載範例vpn-create.json](./assets/vpn-create.json)

   ```json
   {
       "kind": "vpn",
       "region": "va7",
       "addressSpace": [
           "10.104.182.64/26"
       ],
       "dns": {
           "resolvers": [
               "10.151.201.22",
               "10.151.202.22",
               "10.154.155.22"
           ]
       },
       "connections": [{
           "name": "connection-1",
           "gateway": {
               "address": "195.231.212.78",
               "addressSpace": [
                   "10.151.0.0/16",
                   "10.152.0.0/16",
                   "10.153.0.0/16",
                   "10.154.0.0/16",
                   "10.142.0.0/16",
                   "10.143.0.0/16",
                   "10.124.128.0/17"
               ]
           },
           "sharedKey": "<secret_shared_key>",
           "ipsecPolicy": {
               "dhGroup": "ECP256",
               "ikeEncryption": "AES256",
               "ikeIntegrity": "SHA256",
               "ipsecEncryption": "AES256",
               "ipsecIntegrity": "SHA256",
               "pfsGroup": "ECP256",
               "saDatasize": 102400000,
               "saLifetime": 3600
           }
       }]
   }
   ```

   等待45-60分鐘，以便Cloud Manager計畫配置網路基礎架構。

1. 檢查環境是否已完成 __虛擬專用網__ 使用Cloud Manager API進行設定 [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) 操作，使用 `id` 從上一步的createNetworkInfrastructure HTTP請求中傳回。

   __getNetworkInfrastructure HTTP請求__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: <YOUR_BEARER_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   確認HTTP回應包含 __狀態__ of __就緒__. 如果尚未準備好，請每隔幾分鐘重新檢查一次狀態。

## 按環境配置虛擬專用網路代理

1. 啟用並設定 __虛擬專用網__ 使用Cloud Manager API在每個AEMas a Cloud Service環境上進行設定 [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) 操作。

   __enableEnvironmentAdvancedNetworkingConfiguration HTTP請求__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./vpn-configure.json
   ```

   在 `vpn-configure.json` 並提供通過 `... -d @./vpn-configure.json`.

[下載範例vpn-configure.json](./assets/vpn-configure.json)

   ```json
   {
       "nonProxyHosts": [
           "example.net",
           "*.example.org"
       ],
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

   `nonProxyHosts` 聲明一組主機，這些主機應通過預設的共用IP地址範圍而不是專用的輸出IP路由埠80或443。 這可能很有用，因為透過共用IP的流量處理可能會由Adobe自動進一步最佳化。

   針對每個 `portForwards` 映射時，高級網路定義以下轉發規則：

   | 代理主機 | 代理埠 |  | 外部主機 | 外部埠 |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

   如果您的AEM部署 __僅限__ 需要HTTP/HTTPS連線至外部服務，請離開 `portForwards` 陣列空白，因為這些規則僅對非HTTP/HTTPS要求是必要的。


1. 對於每個環境，請使用Cloud Manager API驗證vpn路由規則是否有效 [getEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/getEnvironmentAdvancedNetworkingConfiguration) 操作。

   __getEnvironmentAdvancedNetworkingConfiguration HTTP請求__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. 可使用Cloud Manager API的 [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) 操作。 記住 `enableEnvironmentAdvancedNetworkingConfiguration` 是 `PUT` 操作，因此所有規則都必須隨此操作的每次調用一起提供。

1. 現在，您可以在自訂AEM程式碼和設定中使用虛擬專用網路輸出設定。

## 通過虛擬專用網路連接到外部服務

啟用虛擬專用網路後，AEM程式碼和設定便可使用它們透過VPN對外部服務進行呼叫。 有兩種外部呼叫的處理方式不同：

1. 非標準埠上對外部服務的HTTP/HTTPS呼叫
   + 包括對在標準80或443埠以外的埠上運行的服務進行的HTTP/HTTPS呼叫。
1. 對外部服務的非HTTP/HTTPS呼叫
   + 包括任何非HTTP呼叫，例如與Mail伺服器、SQL資料庫或在其他非HTTP/HTTPS通訊協定上執行的服務的連線。

預設允許來自標準埠(80/443)上AEM的HTTP/HTTPS要求，且不需要額外的設定或考量。

### 非標準埠上的HTTP/HTTPS

從AEM建立HTTP/HTTPS連線至非標準埠(非-80/443)時，必須透過特殊主機和埠進行連線，並透過預留位置提供。

AEM提供兩組對應至AEM HTTP/HTTPS代理的特殊Java™系統變數。

|變數名稱 |使用 | Java™程式碼 | OSGi配置 | Apache Web伺服器mod_proxy配置 | | - | - | - | - | - | | `AEM_HTTP_PROXY_HOST` |用於HTTP連接的代理主機 | `System.getenv("AEM_HTTP_PROXY_HOST")` | `$[env:AEM_HTTP_PROXY_HOST]` | `${AEM_HTTP_PROXY_HOST}` | | `AEM_HTTP_PROXY_PORT` | HTTP連接的代理埠 | `System.getenv("AEM_HTTP_PROXY_PORT")` | `$[env:AEM_HTTP_PROXY_PORT]` |  `${AEM_HTTP_PROXY_PORT}` | | `AEM_HTTPS_PROXY_HOST` |用於HTTPS連接的代理主機 | `System.getenv("AEM_HTTPS_PROXY_HOST")` | `$[env:AEM_HTTPS_PROXY_HOST]` | `${AEM_HTTPS_PROXY_HOST}` | | `AEM_HTTPS_PROXY_PORT` | HTTPS連接的代理埠 | `System.getenv("AEM_HTTPS_PROXY_PORT")` | `$[env:AEM_HTTPS_PROXY_PORT]` | `${AEM_HTTPS_PROXY_PORT}` |

請求HTTP/HTTPS外部服務時，應透過AEM代理主機/埠值來設定Java™ HTTP用戶端的代理設定。

在非標準埠上對外部服務進行HTTP/HTTPS呼叫時，沒有對應的 `portForwards` 必須使用Cloud Manager API定義 `__enableEnvironmentAdvancedNetworkingConfiguration` 操作，因為埠轉送「規則」定義為「在代碼中」。

>[!TIP]
>
> 如需相關資訊，請參閱AEMas a Cloud Service的虛擬專用網路檔案 [完整的路由規則集](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#vpn-traffic-routing).

#### 程式碼範例

<table>
<tr>
<td>
    <a  href="./examples/http-on-non-standard-ports.md"><img alt="非標準埠上的HTTP/HTTPS" src="./assets/code-examples__http.png"/></a>
    <div><strong><a href="./examples/http-on-non-standard-ports.md">非標準埠上的HTTP/HTTPS</a></strong></div>
    <p>
        Java™程式碼範例，讓從AEMas a Cloud Service的HTTP/HTTPS連線至非標準HTTP/HTTPS埠上的外部服務。
    </p>
</td>
<td></td>
<td></td>
</tr>
</table>

### 非HTTP/HTTPS連線程式碼範例

建立非HTTP/HTTPS連線時(例如 SQL、SMTP等)，必須透過AEM提供的特殊主機名稱進行連線。

|變數名稱 |使用 | Java™程式碼 | OSGi配置 | | - | - | - | - | | `AEM_PROXY_HOST` |非HTTP/HTTPS連線的代理主機 | `System.getenv("AEM_PROXY_HOST")` | `$[env:AEM_PROXY_HOST]` |


然後，會透過 `AEM_PROXY_HOST` 和映射埠(`portForwards.portOrig`),AEM接著路由至對應的外部主機名稱(`portForwards.name`)和埠(`portForwards.portDest`)。

| 代理主機 | 代理埠 |  | 外部主機 | 外部埠 |
|---------------------------------|----------|----------------|------------------|----------|
| `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |


#### 程式碼範例

<table><tr>
   <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="使用JDBC DataSourcePool的SQL連接" src="./assets//code-examples__sql-osgi.png"/></a>
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

### 限制透過VPN存取AEMas a Cloud Service

虛擬專用網路配置允許訪問AEMas a Cloud Service環境，但僅限於VPN訪問。

#### 設定範例

<table><tr>
   <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html?lang=en"><img alt="套用IP允許清單" src="./assets/code_examples__vpn-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html?lang=en">套用IP允許清單</a></strong></div>
      <p>
            配置IP允許清單，使得只有VPN流量才能訪問AEM。
      </p>
    </td>
   <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections"><img alt="AEM Publish的路徑型VPN存取限制" src="./assets/code_examples__vpn-path-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections">AEM Publish的路徑型VPN存取限制</a></strong></div>
      <p>
            AEM發佈上的特定路徑需要VPN存取權。
      </p>
    </td>
   <td></td>
</tr></table>
