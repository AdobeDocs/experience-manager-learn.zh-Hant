---
title: 專用輸出 IP 位址
description: 瞭解如何設定及使用專用輸出IP位址，此位址允許來自AEM的輸出連線源自於專用IP。
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9351
thumbnail: KT-9351.jpeg
exl-id: 311cd70f-60d5-4c1d-9dc0-4dcd51cad9c7
last-substantial-update: 2024-04-26T00:00:00Z
duration: 891
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1365'
ht-degree: 1%

---

# 專用輸出 IP 位址

瞭解如何設定及使用專用輸出IP位址，此位址允許來自AEM的輸出連線源自於專用IP。

## 什麼是專用輸出IP位址？

專用輸出IP位址允許來自AEMas a Cloud Service的請求使用專用IP位址，允許外部服務根據此IP位址篩選傳入請求。 按讚 [彈性的輸出埠](./flexible-port-egress.md)，專用輸出IP可讓您從非標準連線埠輸出。

Cloud Manager計畫只能有 __單一__ 網路基礎架構型別。 確保專用輸出IP位址為最多 [適當型別的網路基礎結構](./advanced-networking.md) ，然後才可執行AEMas a Cloud Service的命令。

>[!MORELIKETHIS]
>
> 閱讀AEMas a Cloud Service [進階網路設定檔案](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking) 以取得專用輸出IP位址的詳細資訊。

## 先決條件

使用Cloud Manager API設定專用輸出IP位址時，需要以下專案：

+ Cloud Manager API與 [Cloud Manager企業所有者許可權](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/permissions/)
+ 存取目標 [Cloud Manager API驗證認證](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/create-api-integration/)
   + 組織ID （亦稱為IMS組織ID）
   + 使用者端ID （亦稱為API金鑰）
   + 存取權杖（亦稱為持有人權杖）
+ Cloud Manager計畫ID
+ Cloud Manager環境ID

如需更多詳細資訊，請觀看以下逐步解說，瞭解如何設定、設定和取得Cloud Manager API認證，以及如何使用這些認證進行Cloud Manager API呼叫。

>[!VIDEO](https://video.tv.adobe.com/v/342235?quality=12&learn=on)

本教學課程使用 `curl` 以進行Cloud Manager API設定。 提供的 `curl` 命令採用Linux/macOS語法。 如果使用Windows命令提示字元，請將 `\` 換行字元 `^`.

## 在程式上啟用專用輸出IP位址

首先，在AEMas a Cloud Service上啟用並設定專用輸出IP位址。

>[!BEGINTABS]

>[!TAB Cloud Manager]

專用輸出IP地址可以使用Cloud Manager啟用。 下列步驟概述如何使用Cloud Manager在AEMas a Cloud Service上啟用專用輸出IP位址。

1. 登入 [Adobe Experience Manager Cloud Manager](https://experience.adobe.com/cloud-manager/) 作為Cloud Manager企業所有者。
1. 導覽至所需的計畫。
1. 在左側功能表中，導覽至 __服務>網路基礎結構__.
1. 選取 __新增網路基礎結構__ 按鈕。

   ![新增網路基礎結構](./assets/cloud-manager__add-network-infrastructure.png)

1. 在 __新增網路基礎結構__ 對話方塊中，選取 __專用輸出IP位址__ 選項，然後選取 __地區__ 以建立專用輸出IP位址。

   ![新增專用輸出IP位址](./assets/dedicated-egress-ip-address/select-type.png)

1. 選取 __儲存__ 以確認新增專用輸出IP位址。

   ![確認建立專用輸出IP位址](./assets/dedicated-egress-ip-address/confirmation.png)

1. 等待網路基礎架構建立並標示為 __就緒__. 此程式最多可能需要1小時。

   ![專用輸出IP位址建立狀態](./assets/dedicated-egress-ip-address/ready.png)

在建立專用輸出IP位址後，您現在可以使用Cloud Manager API進行設定，如下所述。

>[!TAB Cloud Manager API]

專用輸出IP地址可以使用Cloud Manager API啟用。 下列步驟概述如何使用Cloud Manager API在AEMas a Cloud Service上啟用專用輸出IP位址。


1. 首先，使用Cloud Manager API判斷需要進階網路的地區 [listRegions](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 作業。 此 `region name` 進行後續Cloud Manager API呼叫所必需。 通常會使用生產環境所在的區域。

   尋找您的AEMas a Cloud Service環境所在地區 [Cloud Manager](https://my.cloudmanager.adobe.com) 在 [環境的詳細資訊](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments). Cloud Manager中顯示的區域名稱可以是 [已對應至地區碼](https://developer.adobe.com/experience-cloud/cloud-manager/guides/api-usage/creating-programs-and-environments/#creating-aem-cloud-service-environments) 用於Cloud Manager API。

   __listRegions HTTP要求__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' 
   ```

2. 使用Cloud Manager API為Cloud Manager計畫啟用專用輸出IP地址 [createNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 作業。 使用適當的 `region` 從Cloud Manager API取得的程式碼 `listRegions` 作業。

   __createNetworkInfrastructure HTTP要求__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d '{ "kind": "dedicatedEgressIp", "region": "va7" }'
   ```

   等待15分鐘，讓Cloud Manager計畫布建網路基礎結構。

3. 檢查程式是否已完成 __專用輸出IP位址__ 使用Cloud Manager API進行設定 [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) 作業，使用 `id` 已從傳回 `createNetworkInfrastructure` 上一步驟中的HTTP要求。

   __getNetworkInfrastructure HTTP要求__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   確認HTTP回應包含 __狀態__ 之 __就緒__. 如果尚未準備就緒，請每隔幾分鐘重新檢查一次狀態。

在建立專用輸出IP位址後，您現在可以使用Cloud Manager API進行設定，如下所述。

>[!ENDTABS]


## 為每個環境設定專用輸出IP位址代理

1. 設定 __專用輸出IP位址__ 使用Cloud Manager API在每個AEMas a Cloud Service環境中進行設定 [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 作業。

   __enableEnvironmentAdvancedNetworkingConfiguration HTTP要求__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./dedicated-egress-ip-address.json
   ```

   在中定義JSON引數 `dedicated-egress-ip-address.json` 並提供給curl，透過 `... -d @./dedicated-egress-ip-address.json`.

   [下載範例dedicated-egress-ip-address.json](./assets/dedicated-egress-ip-address.json). 這個檔案只是範例。 根據以下網址記錄的選用/必要欄位，依需求設定您的檔案： [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/).

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

   專用輸出IP位址設定的HTTP簽章只與不同 [彈性輸出連線埠](./flexible-port-egress.md#enable-dedicated-egress-ip-address-per-environment) 因為它也支援選用的 `nonProxyHosts` 設定。

   `nonProxyHosts` 宣告應透過預設共用IP位址範圍而不是專用輸出IP路由連線埠80或443的一組主機。 `nonProxyHosts` 當Adobe會自動最佳化從共用IP傳出的流量時，這項功能可能會相當實用。

   針對每個 `portForwards` 對應，進階網路會定義下列轉送規則：

   | Proxy主機 | Proxy連線埠 |  | 外部主機 | 外部連線埠 |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

1. 對於每個環境，使用Cloud Manager API驗證輸出規則是否有效 [getEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 作業。

   __getEnvironmentAdvancedNetworkingConfiguration HTTP要求__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: <YOUR_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. 專用輸出IP地址配置可以使用Cloud Manager API更新 [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 作業。 記住 `enableEnvironmentAdvancedNetworkingConfiguration` 是 `PUT` 作業，因此每次呼叫此作業時，都必須提供所有規則。

1. 取得 __專用輸出IP位址__ 藉由使用DNS解析程式(例如 [DNSChecker.org](https://dnschecker.org/))： `p{programId}.external.adobeaemcloud.com`，或透過執行 `dig` 命令列中的。

   ```shell
   $ dig +short p{programId}.external.adobeaemcloud.com
   ```

   主機名稱不可以是 `pinged`，因為是輸出和 _非_ 和入口。

   請注意，專用輸出IP位址由計畫中的所有AEMas a Cloud Service環境共用。

1. 現在，您可以在自訂AEM程式碼和設定中使用專用輸出IP位址。 通常在使用專用輸出IP位址時，AEMas a Cloud Service連線的外部服務會設定為只允許來自此專用IP位址的流量。

## 透過專用輸出IP位址連線到外部服務

在啟用專用輸出IP位址的情況下，AEM程式碼和設定可以使用專用輸出IP來呼叫外部服務。 AEM處理外部呼叫的方式有兩種：

1. 對外部服務的HTTP/HTTPS呼叫
   + 包括對標準80或443連線埠以外的連線埠上執行的服務發出的HTTP/HTTPS呼叫。
1. 對外部服務的非HTTP/HTTPS呼叫
   + 包括任何非HTTP呼叫，例如與郵件伺服器、SQL資料庫或服務之間的連線，這些服務會在其他非HTTP/HTTPS通訊協定上執行。

預設允許來自標準連線埠(80/443)上AEM的HTTP/HTTPS請求，但如果未依照以下所述正確設定，則這些請求不會使用專用輸出IP位址。

>[!TIP]
>
> 請參閱AEMas a Cloud Service的專用輸出IP位址檔案，以瞭解 [完整的路由規則集](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking).


### HTTP/HTTPS

從AEM建立HTTP/HTTPS連線時，若使用專用輸出IP位址，HTTP/HTTPS連線會自動使用專用輸出IP位址代理出AEM。 不需要其他程式碼或設定即可支援HTTP/HTTPS連線。

#### 程式碼範例

<table>
<tr>
<td>
    <a  href="./examples/http-dedicated-egress-ip-vpn.md"><img alt="HTTP/HTTPS" src="./assets/code-examples__http.png"/></a>
    <div><strong><a href="./examples/http-dedicated-egress-ip-vpn.md">HTTP/HTTPS</a></strong></div>
    <p>
        Java™程式碼範例使用HTTP/HTTPS通訊協定，使從AEM的HTTP/HTTPS連線as a Cloud Service到外部服務。
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
| `AEM_PROXY_HOST` | 非HTTP/HTTPS連線的Proxy主機 | `System.getenv("AEM_PROXY_HOST")` | `$[env:AEM_PROXY_HOST]` |


然後，會透過呼叫外部服務的連線。 `AEM_PROXY_HOST` 和對應的連線埠(`portForwards.portOrig`)，則AEM會路由至對應的外部主機名稱(`portForwards.name`)和連線埠(`portForwards.portDest`)。

| Proxy主機 | Proxy連線埠 |  | 外部主機 | 外部連線埠 |
|---------------------------------|----------|----------------|------------------|----------|
| `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

#### 程式碼範例

<table><tr>
   <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="使用JDBC DataSourcePool的SQL連線" src="./assets//code-examples__sql-osgi.png"/></a>
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
