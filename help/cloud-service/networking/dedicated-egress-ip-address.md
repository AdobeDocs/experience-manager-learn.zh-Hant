---
title: 專用的輸出IP地址
description: 了解如何設定和使用專用的輸出IP位址，如此可讓來自AEM的傳出連線源自於專用的IP。
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9351
thumbnail: KT-9351.jpeg
exl-id: 311cd70f-60d5-4c1d-9dc0-4dcd51cad9c7
source-git-commit: 6ae98ce749f8a485bdaa4c6c6232e52d8d6246b3
workflow-type: tm+mt
source-wordcount: '1218'
ht-degree: 4%

---

# 專用的輸出IP地址

了解如何設定和使用專用的輸出IP位址，如此可讓來自AEM的傳出連線源自於專用的IP。

## 什麼是專用的輸出IP地址？

專用輸出IP地址允許來自AEMas a Cloud Service的請求使用專用的IP地址，允許外部服務按此IP地址過濾傳入的請求。 贊 [靈活的輸出埠](./flexible-port-egress.md)，專用的輸出IP可讓您在非標準埠上輸出。

Cloud Manager程式只能有 __單一__ 網路基礎結構類型。 請確保專用的輸出IP位址是 [適當類型的網路基礎設施](./advanced-networking.md)  以取得AEMas a Cloud Service。

>[!MORELIKETHIS]
>
> 閱讀AEMas a Cloud Service [高級網路配置文檔](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#dedicated-egress-IP-address) 如需專用輸出IP位址的詳細資訊。

## 必備條件

設定專用的輸出IP位址時，需要下列項目：

+ Cloud Manager API搭配 [Cloud Manager業務擁有者權限](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/permissions/)
+ 存取 [Cloud Manager API驗證憑證](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/authentication/)
   + 組織ID（亦稱為IMS組織ID）
   + 用戶端ID（亦稱為API金鑰）
   + 存取權杖（亦稱為承載權杖）
+ Cloud Manager方案ID
+ Cloud Manager環境ID

如需詳細資訊，請觀看下列逐步說明，了解如何設定、設定及取得Cloud Manager API憑證，以及如何使用這些憑證來進行Cloud Manager API呼叫。

>[!VIDEO](https://video.tv.adobe.com/v/342235/?quality=12&learn=on)

本教學課程使用 `curl` 來設定Cloud Manager API。 提供的 `curl` 命令採用Linux/macOS語法。 如果使用Windows命令提示符，請替換 `\` 斷行字元，帶 `^`.

## 在程式上啟用專用的輸出IP地址

首先，在AEMas a Cloud Service上啟用並設定專用的輸出IP位址。

1. 首先，使用Cloud Manager API確定需要進階網路的地區 [listRegions](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作。 此 `region name` 必要，才能進行後續的Cloud Manager API呼叫。 通常會使用生產環境所在的地區。

   在中找出AEMas a Cloud Service環境的地區 [Cloud Manager](https://my.cloudmanager.adobe.com) 在 [環境的詳細資訊](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html?lang=en#viewing-environment). Cloud Manager中顯示的地區名稱可以是 [映射至地區代碼](https://developer.adobe.com/experience-cloud/cloud-manager/guides/api-usage/creating-programs-and-environments/#creating-aem-cloud-service-environments) 用於Cloud Manager API。

   __listRegions HTTP要求__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' 
   ```

1. 使用Cloud Manager API為Cloud Manager程式啟用專用的輸出IP位址 [createNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作。 使用適當 `region` 從Cloud Manager API取得的程式碼 `listRegions` 操作。

   __createNetworkInfrastructure HTTP請求__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d '{ "kind": "dedicatedEgressIp", "region": "va7" }'
   ```

   等待15分鐘，讓Cloud Manager計畫配置網路基礎架構。

1. 檢查程式是否已完成 __專用輸出IP地址__ 使用Cloud Manager API進行設定 [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) 操作，使用 `id` 從上一步的createNetworkInfrastructure HTTP請求中傳回。

   __getNetworkInfrastructure HTTP請求__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   確認HTTP回應包含 __狀態__ of __就緒__. 如果尚未就緒，請每隔幾分鐘重新檢查狀態。

## 按環境配置專用的輸出IP地址代理

1. 設定 __專用輸出IP地址__ 使用Cloud Manager API在每個AEMas a Cloud Service環境上進行設定 [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作。

   __enableEnvironmentAdvancedNetworkingConfiguration HTTP請求__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./dedicated-egress-ip-address.json
   ```

   在 `dedicated-egress-ip-address.json` 並提供通過 `... -d @./dedicated-egress-ip-address.json`.

   [下載example decipated-eogress-ip-address.json](./assets/dedicated-egress-ip-address.json). 此檔案只是範例。 根據上述選填/必填欄位，視需要設定檔案 [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/).

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

   專用輸出IP地址配置的HTTP簽名與 [靈活的輸出埠](./flexible-port-egress.md#enable-dedicated-egress-ip-address-per-environment) 因為它也支援選用 `nonProxyHosts` 設定。

   `nonProxyHosts` 聲明一組主機，這些主機應通過預設的共用IP地址範圍而不是專用的輸出IP路由埠80或443。 `nonProxyHosts` 當透過共用IP的流量處理可能會由Adobe自動進一步最佳化時，可能會很有用。

   針對每個 `portForwards` 映射時，高級網路定義以下轉發規則：

   | 代理主機 | 代理埠 |  | 外部主機 | 外部埠 |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

1. 對於每個環境，請使用Cloud Manager API驗證輸出規則是否有效 [getEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作。

   __getEnvironmentAdvancedNetworkingConfiguration HTTP請求__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: <YOUR_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. 可使用Cloud Manager API更新專屬輸出IP位址設定 [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作。 記住 `enableEnvironmentAdvancedNetworkingConfiguration` 是 `PUT` 操作，因此所有規則都必須隨此操作的每次調用一起提供。

1. 取得 __專用輸出IP地址__ 使用DNS解析器(例如 [DNSChecker.org](https://dnschecker.org/)): `p{programId}.external.adobeaemcloud.com`，或執行 `dig` 從命令列。

   ```shell
   $ dig +short p{programId}.external.adobeaemcloud.com
   ```

   主機名不能是 `pinged`，因為這是進步 _not_ 和入口。

   請注意 __專用輸出IP地址__ 共用給方案中所有AEMas a Cloud Service環境。

1. 現在，您可以在自訂AEM程式碼和設定中使用專屬的輸出IP位址。 使用專用輸出IP位址時，所連線的外部服務AEMas a Cloud Service通常會設定為僅允許來自此專用IP位址的流量。

## 通過專用的輸出IP地址連接到外部服務

啟用專用的輸出IP位址後，AEM程式碼和設定便可使用專用的輸出IP來呼叫外部服務。 有兩種外部呼叫的處理方式不同：

1. 對外部服務的HTTP/HTTPS呼叫
   + 包括對在標準80或443埠以外的埠上運行的服務進行的HTTP/HTTPS呼叫。
1. 對外部服務的非HTTP/HTTPS呼叫
   + 包括任何非HTTP呼叫，例如與Mail伺服器、SQL資料庫或在其他非HTTP/HTTPS通訊協定上執行的服務的連線。

預設允許來自標準埠(80/443)上AEM的HTTP/HTTPS要求，但如果未依下列說明適當設定，則不會使用專用的輸出IP位址。

>[!TIP]
>
> 請參閱AEMas a Cloud Service的專屬輸出IP位址檔案，以取得 [完整的路由規則集](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#dedcated-egress-ip-traffic-routing=).


### HTTP/HTTPS

從AEM建立HTTP/HTTPS連線時，若使用專用的輸出IP位址，則使用專用的輸出IP位址，自動從AEM中推進HTTP/HTTPS連線。 支援HTTP/HTTPS連線不需要額外的程式碼或設定。

#### 程式碼範例

<table>
<tr>
<td>
    <a  href="./examples/http-dedicated-egress-ip-vpn.md"><img alt="HTTP/HTTPS" src="./assets/code-examples__http.png"/></a>
    <div><strong><a href="./examples/http-dedicated-egress-ip-vpn.md">HTTP/HTTPS</a></strong></div>
    <p>
        Java™程式碼範例，使用HTTP/HTTPS通訊協定，將從AEMas a Cloud Service的HTTP/HTTPS連線至外部服務。
    </p>
</td>   
<td></td>   
<td></td>   
</tr>
</table>

### 與外部服務的非HTTP/HTTPS連線

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
      <a  href="./examples/email-service.md"><img alt="虛擬私人網路 (VPN)" src="./assets/code-examples__email.png"/></a>
      <div><strong><a href="./examples/email-service.md">電子郵件服務</a></strong></div>
      <p>
        OSGi設定範例，使用AEM連線至外部電子郵件服務。
      </p>
    </td>   
</tr></table>
