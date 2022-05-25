---
title: 專用出口IP地址
description: 瞭解如何設定和使用專用出口IP地址，該地址允許從專用IPAEM發出出站連接。
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9351
thumbnail: KT-9351.jpeg
exl-id: 311cd70f-60d5-4c1d-9dc0-4dcd51cad9c7
source-git-commit: 4f8222d3185ad4e87eda662c33c9ad05ce3b0427
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# 專用出口IP地址

瞭解如何設定和使用專用出口IP地址，該地址允許從專用IPAEM發出出站連接。

## 什麼是專用出口IP地址？

專用出口IP地址允許AEM來自as a Cloud Service的請求使用專用IP地址，從而允許外部服務按此IP地址過濾傳入的請求。 像 [靈活的出口埠](./flexible-port-egress.md)，專用出口IP允許您在非標準埠上出口。

雲管理器程式只能 __單__ 網路基礎架構類型。 確保專用出口IP地址是 [適當類型的網路基礎架構](./advanced-networking.md)  執行AEM以下命令之前為as a Cloud Service。

>[!MORELIKETHIS]
>
> 閱讀AEMas a Cloud Service [高級網路配置文檔](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#dedicated-egress-IP-address) 以獲取有關專用出口IP地址的詳細資訊。

## 必備條件

設定專用出口IP地址時，需要執行以下操作：

+ Cloud Manager API(帶 [雲管理器業務所有者權限](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/permissions/)
+ 訪問 [Cloud Manager API身份驗證憑據](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/authentication/)
   + 組織標識（又稱IMS組織標識）
   + 客戶端ID（又稱API密鑰）
   + 訪問令牌（又稱持有者令牌）
+ 雲管理器程式ID
+ 雲管理器環境ID

有關詳細資訊，請觀看以下步驟，瞭解如何設定、配置和獲取Cloud Manger API憑據，以及如何使用這些憑據進行Cloud Manager API調用。

>[!VIDEO](https://video.tv.adobe.com/v/342235/?quality=12&learn=on)

本教程使用 `curl` 來配置Cloud Manager API。 提供的 `curl` 命令採用Linux/macOS語法。 如果使用Windows命令提示符，請替換 `\` 換行符 `^`。

## 在程式上啟用專用出口IP地址

從啟用和配置as a Cloud Service上的專用出口IP地址開始AEM。

1. 首先，使用Cloud Manager API確定將在其中設定高級網路的區域 [清單區域](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 的下界。 的 `region name` 需要進行後續的Cloud Manager API調用。 通常，使用生產環境所在的區域。

   __listRegions HTTP請求__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' 
   ```

1. 使用Cloud Manager API為Cloud Manager程式啟用專用出口IP地址 [建立網路基礎架構](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 的下界。 使用適當 `region` 從Cloud Manager API獲取的代碼 `listRegions` 的下界。

   __createNetworkInfrastructure HTTP請求__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d '{ "kind": "dedicatedEgressIp", "region": "va7" }'
   ```

   等待15分鐘，使Cloud Manager計畫配置網路基礎架構。

1. 檢查環境是否已完成 __專用出口IP地址__ 使用Cloud Manager API進行配置 [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) 操作，使用 `id` 從上一步的createNetworkInfrastructure HTTP請求返回。

   __getNetworkInfrastructure HTTP請求__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   驗證HTTP響應是否包含 __狀態__ 共 __就緒__。 如果尚未準備好，請每隔幾分鐘重新檢查一次狀態。

## 按環境配置專用出口IP地址代理

1. 啟用和配置 __專用出口IP地址__ 在每個AEMas a Cloud Service環境上使用Cloud Manager API進行配置 [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 的下界。

   __enableEnvironmentAdvancedNetworkingConfiguration HTTP請求__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./dedicated-egress-ip-address.json
   ```

   在中定義JSON參數 `dedicated-egress-ip-address.json` 並提供捲曲通道 `... -d @./dedicated-egress-ip-address.json`。

   [下載dedicated-egress-ip-address.json示例](./assets/dedicated-egress-ip-address.json)。 此檔案僅是示例。 根據上記錄的可選/必填欄位，根據需要配置檔案 [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/)。

   ```json
   {
       "nonProxyHosts": [
           "example.net",
           "*.example.org",
       ],
       "portForwards": [
           {
               "name": "mysql.example.com",
               "portDest": 3306,
               "portOrig": 30001
           },
           {
               "name": "smtp.sendgrid.net",
               "portDest": 465,
               "portOrig": 30002
           }
       ]
   }
   ```

   專用出口IP地址配置的HTTP簽名僅與 [柔性出口埠](./flexible-port-egress.md#enable-dedicated-egress-ip-address-per-environment) 因為它還支援可選 `nonProxyHosts` 配置。

   `nonProxyHosts` 聲明一組主機，其埠80或443應通過預設共用IP地址範圍而不是專用出口IP路由。 `nonProxyHosts` 當通過共用IP的流量通過Adobe可以進一步自動優化時，該方法可能有用。

   每個 `portForwards` 映射，高級網路定義以下轉發規則：

   | 代理主機 | 代理埠 |  | 外部主機 | 外部埠 |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

1. 對於每個環境，使用Cloud Manager API驗證出口規則是否有效 [getEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 的下界。

   __getEnvironmentAdvancedNetworkingConfiguration HTTP請求__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: <YOUR_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. 可以使用Cloud Manager API更新專用出口IP地址配置 [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 的下界。 記住 `enableEnvironmentAdvancedNetworkingConfiguration` 是 `PUT` 操作，因此必須為此操作的每次調用提供所有規則。

1. 獲取 __專用出口IP地址__ 使用DNS解析器(如 [DNSChecker.org](https://dnschecker.org/)): `p{programId}.external.adobeaemcloud.com`，或運行 `dig` 命令行。

   ```shell
   $ dig +short p{programId}.external.adobeaemcloud.com
   ```

   主機名不能 `pinged`，因為它是個出口 _不_ 和入口。

1. 現在，您可以在自定義代碼和配置中使用AEM專用出口IP地址。 通常，當使用專用出口IP地址時，AEM將as a Cloud Service連接的外部服務配置為僅允許來自此專用IP地址的通信。

## 通過專用埠出口連接到外部服務

啟用專用出口IP地址後，AEM代碼和配置可以使用專用出口IP來調用外部服務。 外部呼叫有兩種不同的AEM處理方式：

1. 在非標準埠上對外部服務的HTTP/HTTPS調用
   + 包括對在標準80或443埠以外的埠上運行的服務進行的HTTP/HTTPS調用。
1. 對外部服務的非HTTP/HTTPS調用
   + 包括任何非HTTP調用，如與Mail伺服器、SQL資料庫或在其他非HTTP/HTTPS協定上運行的服務的連接。

預設情況下，標準端AEM口(80/443)上的HTTP/HTTPS請求是允許的，不需要額外的配置或注意事項。

>[!TIP]
>
> 請參AEM閱as a Cloud Service的專用出口IP地址文檔 [全套路由規則](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#dedcated-egress-ip-traffic-routing=)。


### 非標準埠上的HTTP/HTTPS

從建立到非標準埠（非–80/443）的HTTP/HTTPS連接時AEM，必須通過特殊主機和埠（通過佔位符提供）進行連接。

提AEM供兩組映射到HTTP/HTTPS代理的AEM特殊Java™系統變數。

|變數名稱 |使用 | Java™代碼 | OSGi配置 | | - | - | - | - | | `AEM_HTTP_PROXY_HOST` | HTTP連接的代理主機 | `System.getenv("AEM_HTTP_PROXY_HOST")` | `$[env:AEM_HTTP_PROXY_HOST]` | | `AEM_HTTP_PROXY_PORT` | HTTP連接的代理埠 | `System.getenv("AEM_HTTP_PROXY_PORT")` | `$[env:AEM_HTTP_PROXY_PORT]` | | `AEM_HTTPS_PROXY_HOST` | HTTPS連接的代理主機 | `System.getenv("AEM_HTTPS_PROXY_HOST")` | `$[env:AEM_HTTPS_PROXY_HOST]` | | `AEM_HTTPS_PROXY_PORT` | HTTPS連接的代理埠 | `System.getenv("AEM_HTTPS_PROXY_PORT")` | `$[env:AEM_HTTPS_PROXY_PORT]` |

對HTTP/HTTPS外部服務的請求應通過使用代理主機/埠值配置Java™ HTTP客戶端的代理AEM配置進行。

在非標準埠上對外部服務進行HTTP/HTTPS調用時，沒有相應的 `portForwards` 必須使用雲管理器API定義 `enableEnvironmentAdvancedNetworkingConfiguration` 操作，因為埠轉發「規則」是「在代碼中」定義的。

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

### 到外部服務的非HTTP/HTTPS連接

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
