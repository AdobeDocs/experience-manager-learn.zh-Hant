---
title: 虛擬專用網(VPN)
description: 瞭解如何將AEMas a Cloud Service與VPN連接，以在與內部服務之間建立安AEM全通信通道。
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9352
thumbnail: KT-9352.jpeg
exl-id: 74cca740-bf5e-4cbd-9660-b0579301a3b4
source-git-commit: 52a2303f75c23c72e201b1f674f7f882db00710b
workflow-type: tm+mt
source-wordcount: '1364'
ht-degree: 0%

---

# 虛擬專用網(VPN)

瞭解如何將AEMas a Cloud Service與VPN連接，以在與內部服務之間建立安AEM全通信通道。

## 什麼是虛擬專用網路？

虛擬專用網(VPN)允許AEMas a Cloud Service客戶連接 **環AEM境** Cloud Manager程式中的 [支援](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#vpn) VPN。 這允許在客戶網路內的AEMas a Cloud Service和服務之間進行安全和受控的連接。

雲管理器程式只能 __單__ 網路基礎架構類型。 確保虛擬專用網路是 [適當類型的網路基礎架構](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#general-vpn-considerations) 執行AEM以下命令之前為as a Cloud Service。

>[!NOTE]
>
>請注意，不支援將生成環境從雲管理器連接到VPN。 如果必須從專用儲存庫訪問二進位對象，則必須設定具有公共Internet上可用URL的安全且受密碼保護的儲存庫 [此處描述](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/create-application-project/setting-up-project.html#password-protected-maven-repositories)。

>[!MORELIKETHIS]
>
> 閱讀AEMas a Cloud Service [高級網路配置文檔](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#vpn) 的子菜單。

## 必備條件

設定虛擬專用網路時，需要執行以下操作：

+ Adobe帳戶 [雲管理器業務所有者權限](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/permissions/)
+ 訪問 [Cloud Manager API的驗證憑據](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/authentication/)
   + 組織標識（又稱IMS組織標識）
   + 客戶端ID（又稱API密鑰）
   + 訪問令牌（又稱持有者令牌）
+ 雲管理器程式ID
+ 雲管理器環境ID
+ 一個虛擬專用網路，可訪問所有必需的連接參數。

有關詳細資訊，請觀看以下步驟，瞭解如何設定、配置和獲取Cloud Manger API憑據，以及如何使用這些憑據進行Cloud Manager API調用。

>[!VIDEO](https://video.tv.adobe.com/v/342235/?quality=12&learn=on)

本教程使用 `curl` 來配置Cloud Manager API。 提供的 `curl` 命令採用Linux/macOS語法。 如果使用Windows命令提示符，請替換 `\` 換行符 `^`。

## 按程式啟用虛擬專用網路

從啟用as a Cloud Service上的虛擬專用網路開始AEM。

1. 首先，使用Cloud Manager API確定將在其中設定高級網路的區域 [清單區域](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 的下界。 的 `region name` 將需要進行後續的Cloud Manager API調用。 通常，使用生產環境所在的區域。

   __listRegions HTTP請求__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. 使用Cloud Manager API為Cloud Manager程式啟用虛擬專用網路 [建立網路基礎架構](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 的下界。 使用適當 `region` 從Cloud Manager API獲取的代碼 `listRegions` 的下界。

   __createNetworkInfrastructure HTTP請求__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
       -d @./vpn-create.json
   ```

   在中定義JSON參數 `vpn-create.json` 並提供捲曲通道 `... -d @./vpn-create.json`。

   [下載示例vpn-create.json](./assets/vpn-create.json)。  此檔案僅是示例。 根據上記錄的可選/必填欄位，根據需要配置檔案 [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/)。

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
           ],
           "domains": [
               "wknd.site",
               "wknd.com"
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

   等待45-60分鐘，Cloud Manager計畫才能預配網路基礎架構。

1. 檢查環境是否已完成 __虛擬專用網路__ 使用Cloud Manager API進行配置 [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) 操作，使用 `id` 從上一步的createNetworkInfrastructure HTTP請求返回。

   __getNetworkInfrastructure HTTP請求__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: <YOUR_BEARER_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   驗證HTTP響應是否包含 __狀態__ 共 __就緒__。 如果尚未準備好，請每隔幾分鐘重新檢查一次狀態。

## 按環境配置虛擬專用網路代理

1. 啟用和配置 __虛擬專用網路__ 在每個AEMas a Cloud Service環境上使用Cloud Manager API進行配置 [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 的下界。

   __enableEnvironmentAdvancedNetworkingConfiguration HTTP請求__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./vpn-configure.json
   ```

   在中定義JSON參數 `vpn-configure.json` 並提供捲曲通道 `... -d @./vpn-configure.json`。

[下載示例vpn-configure.json](./assets/vpn-configure.json)

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

   `nonProxyHosts` 聲明一組主機，其埠80或443應通過預設共用IP地址範圍而不是專用出口IP路由。 `nonProxyHosts` 當通過共用IP的流量通過Adobe可以進一步自動優化時，該方法可能有用。

   每個 `portForwards` 映射，高級網路定義以下轉發規則：

   | 代理主機 | 代理埠 |  | 外部主機 | 外部埠 |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

   如AEM果 __僅__ 需要HTTP/HTTPS連接到外部服務，請 `portForwards` 陣列為空，因為這些規則僅對非HTTP/HTTPS請求是必需的。


1. 對於每個環境，使用Cloud Manager API驗證VPN路由規則是否有效 [getEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 的下界。

   __getEnvironmentAdvancedNetworkingConfiguration HTTP請求__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. 可使用Cloud Manager API更新虛擬專用網路代理配置 [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 的下界。 記住 `enableEnvironmentAdvancedNetworkingConfiguration` 是 `PUT` 操作，因此必須為此操作的每次調用提供所有規則。

1. 現在，您可以在自定義代碼和配置中使用虛擬專AEM用網出口配置。

## 通過虛擬專用網路連接到外部服務

啟用虛擬專用網路後，AEM代碼和配置可以使用它們通過VPN對外部服務進行呼叫。 外部呼叫有兩種不同的AEM處理方式：

1. 在非標準埠上對外部服務的HTTP/HTTPS調用
   + 包括對在標準80或443埠以外的埠上運行的服務進行的HTTP/HTTPS調用。
1. 對外部服務的非HTTP/HTTPS調用
   + 包括任何非HTTP調用，如與Mail伺服器、SQL資料庫或在其他非HTTP/HTTPS協定上運行的服務的連接。

預設情況下，標準端AEM口(80/443)上的HTTP/HTTPS請求是允許的，不需要額外的配置或注意事項。

### 非標準埠上的HTTP/HTTPS

從建立到非標準埠（非–80/443）的HTTP/HTTPS連接時AEM，必須通過特殊主機和埠（通過佔位符提供）進行連接。

提AEM供兩組映射到HTTP/HTTPS代理的AEM特殊Java™系統變數。

|變數名稱 |使用 | Java™代碼 | OSGi配置 | Apache Web伺服器mod_proxy配置 | | - | - | - | - | - | | `AEM_HTTP_PROXY_HOST` | HTTP連接的代理主機 | `System.getenv("AEM_HTTP_PROXY_HOST")` | `$[env:AEM_HTTP_PROXY_HOST]` | `${AEM_HTTP_PROXY_HOST}` | | `AEM_HTTP_PROXY_PORT` | HTTP連接的代理埠 | `System.getenv("AEM_HTTP_PROXY_PORT")` | `$[env:AEM_HTTP_PROXY_PORT]` |  `${AEM_HTTP_PROXY_PORT}` | | `AEM_HTTPS_PROXY_HOST` | HTTPS連接的代理主機 | `System.getenv("AEM_HTTPS_PROXY_HOST")` | `$[env:AEM_HTTPS_PROXY_HOST]` | `${AEM_HTTPS_PROXY_HOST}` | | `AEM_HTTPS_PROXY_PORT` | HTTPS連接的代理埠 | `System.getenv("AEM_HTTPS_PROXY_PORT")` | `$[env:AEM_HTTPS_PROXY_PORT]` | `${AEM_HTTPS_PROXY_PORT}` |

通過代理主機/埠值配置Java™ HTTP客戶端的代理配置，應請求HTTP/HTTPSAEM外部服務。

在非標準埠上對外部服務進行HTTP/HTTPS調用時，沒有相應的 `portForwards` 必須使用Cloud Manager API定義 `__enableEnvironmentAdvancedNetworkingConfiguration` 操作，因為埠轉發「規則」是「在代碼中」定義的。

>[!TIP]
>
> 請參AEM閱as a Cloud Service的虛擬專用網路文檔 [全套路由規則](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#vpn-traffic-routing)。

#### 代碼示例

<table>
<tr>
<td>
    <a  href="./examples/http-on-non-standard-ports.md"><img alt="非標準埠上的HTTP/HTTPS" src="./assets/code-examples__http.png"/></a>
    <div><strong><a href="./examples/http-on-non-standard-ports.md">非標準埠上的HTTP/HTTPS</a></strong></div>
    <p>
        Java™代碼示例：在非標準HTTP/HTTPS端AEM口上將HTTP/HTTPS從as a Cloud Service連接到外部服務。
    </p>
</td>
<td></td>
<td></td>
</tr>
</table>

### 非HTTP/HTTPS連接代碼示例

建立非HTTP/HTTPS連接時(例如 SQL 、 SMTP等),AEM必須通過提供的特殊主機名建立連AEM接。

|變數名稱 |使用 | Java™代碼 | OSGi配置 | | - | - | - | - | | `AEM_PROXY_HOST` |非HTTP/HTTPS連接的代理主機 | `System.getenv("AEM_PROXY_HOST")` | `$[env:AEM_PROXY_HOST]` |


然後，通過 `AEM_PROXY_HOST` 和映射的埠(`portForwards.portOrig`),AEM然後路由到映射的外部主機名(`portForwards.name`)和埠(`portForwards.portDest`)。

| 代理主機 | 代理埠 |  | 外部主機 | 外部埠 |
|---------------------------------|----------|----------------|------------------|----------|
| `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |


#### 代碼示例

<table><tr>
   <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="使用JDBC DataSourcePool的SQL連接" src="./assets//code-examples__sql-osgi.png"/></a>
      <div><strong><a href="./examples/sql-datasourcepool.md">使用JDBC DataSourcePool的SQL連接</a></strong></div>
      <p>
            通過配置JDBC資料源池連接到外部SQL資料AEM庫的Java™代碼示例。
      </p>
    </td>
   <td>
      <a  href="./examples/sql-java-apis.md"><img alt="使用Java API的SQL連接" src="./assets/code-examples__sql-java-api.png"/></a>
      <div><strong><a href="./examples/sql-java-apis.md">使用Java™ API的SQL連接</a></strong></div>
      <p>
            Java™代碼示例使用Java™的SQL API連接到外部SQL資料庫。
      </p>
    </td>
   <td>
      <a  href="./examples/email-service.md"><img alt="虛擬專用網(VPN)" src="./assets/code-examples__email.png"/></a>
      <div><strong><a href="./examples/email-service.md">電子郵件服務</a></strong></div>
      <p>
        OSGi配置示例AEM，用於連接到外部電子郵件服務。
      </p>
    </td>
</tr></table>

### 限制通過VPNAEM訪問as a Cloud Service

虛擬專用網路配置限制對AEMVPN的as a Cloud Service環境的訪問。

#### 配置示例

<table><tr>
   <td>
      <a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html"><img alt="應用IP允許清單" src="./assets/code_examples__vpn-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html">應用IP允許清單</a></strong></div>
      <p>
            配置IP允許清單，以便只有VPN通信可以訪問AEM。
      </p>
    </td>
   <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections"><img alt="基於路徑的VPN對AEM發佈的訪問限制" src="./assets/code_examples__vpn-path-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections">基於路徑的VPN對AEM發佈的訪問限制</a></strong></div>
      <p>
            AEM發佈上的特定路徑需要VPN訪問。
      </p>
    </td>
   <td></td>
</tr></table>
