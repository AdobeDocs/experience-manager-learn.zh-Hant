---
title: 如何清除CDN快取
description: 瞭解如何從AEM as a Cloud Service的CDN中清除或移除快取的HTTP回應。
version: Experience Manager as a Cloud Service
feature: Operations, CDN Cache
topic: Administration, Performance
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-08-13T00:00:00Z
jira: KT-15963
thumbnail: KT-15963.jpeg
exl-id: 5d81f6ee-a7df-470f-84b9-12374c878a1b
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '924'
ht-degree: 0%

---

# 如何清除CDN快取

瞭解如何從AEM as a Cloud Service的CDN中清除或移除快取的HTTP回應。 使用名為&#x200B;**清除API Token**&#x200B;的自助服務功能，您可以清除特定資源、資源群組和整個快取的快取。

在本教學課程中，您將瞭解如何使用自助服務功能，設定及使用Purge API Token來清除範例[AEM WKND](https://github.com/adobe/aem-guides-wknd)網站的CDN快取。

>[!VIDEO](https://video.tv.adobe.com/v/3432948?quality=12&learn=on)

## 快取失效vs明確清除

有兩種方法可從CDN移除快取資源：

1. **快取失效：**&#x200B;這是根據快取標題（如`Cache-Control`、`Surrogate-Control`或`Expires`）從CDN移除快取資源的程式。 快取標頭的`max-age`屬性值是用來決定資源的快取存留期，也稱為快取TTL （存留時間）。 快取存留期過期時，系統會自動從CDN快取中移除快取資源。

1. **明確清除：**&#x200B;這是在TTL到期之前，從CDN快取中手動移除快取資源的程式。 當您想要立即移除快取的資源時，明確清除會很有用。 但是，它會增加到原始伺服器的流量。

從CDN快取中移除快取的資源時，同一資源的下一個請求會從原始伺服器擷取最新版本。

## 設定清除API Token

讓我們瞭解如何設定清除API權杖以清除CDN快取。

### 設定CDN規則

清除API權杖是透過在您的AEM專案代碼中設定CDN規則來建立。

1. 從您AEM專案的主要`cdn.yaml`資料夾開啟`config`檔案。 例如，[WKND專案的cdn.yaml](https://github.com/adobe/aem-guides-wknd/blob/main/config/cdn.yaml)檔案。

1. 將下列CDN規則新增至`cdn.yaml`檔案：

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:  
  authentication: # The main authentication configuration
    authenticators: # The list of authenticators
       - name: purge-auth # The name of the authenticator
         type: purge  # The type of the authenticator, must be purge
         purgeKey1: ${{CDN_PURGEKEY_081324}} # The first purge key, must be referenced by the Cloud Manager secret-type environment variable name ${{CDN_EDGEKEY_073124}}
         purgeKey2: ${{CDN_PURGEKEY_111324}} # The second purge key, must be referenced by the Cloud Manager secret-type environment variable name ${{CDN_EDGEKEY_111324}}. It is used for the rotation of secrets without any interruptions.
    rules: # The list of authentication rules
       - name: purge-auth-rule # The name of the rule
         when: { reqProperty: tier, equals: "publish" } # The condition when the rule should be applied
         action: # The action to be taken when the rule is applied
           type: authenticate # The type of the action, must be authenticate
           authenticator: purge-auth # The name of the authenticator to be used, must match the name from the above authenticators list               
```

在上述規則中，`purgeKey1`和`purgeKey2`都是從頭開始新增，以支援密碼輪換而不會造成任何中斷。 但是，您只能從`purgeKey1`開始，並在稍後旋轉密碼時新增`purgeKey2`。

1. 儲存、提交變更，並將其推送至Adobe上游存放庫。

### 建立Cloud Manager環境變數

接下來，建立Cloud Manager環境變數來儲存「清除API Token」值。

1. 在[my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com/)登入Cloud Manager並選取您的組織和程式。

1. 在&#x200B;__環境__&#x200B;區段中，按一下所需環境旁的&#x200B;**省略符號** (...)，並選取&#x200B;**檢視詳細資料**。

   ![檢視詳細資料](../assets/how-to/view-env-details.png)

1. 然後選取&#x200B;**組態**&#x200B;索引標籤，再按一下&#x200B;**新增組態**&#x200B;按鈕。

1. 在&#x200B;**環境組態**&#x200B;對話方塊中，輸入下列詳細資料：
   - **名稱**：輸入環境變數的名稱。 它必須符合`purgeKey1`檔案中的`purgeKey2`或`cdn.yaml`值。
   - **值**：輸入清除API Token值。
   - **已套用服務**：選取&#x200B;**全部**&#x200B;選項。
   - **型別**：選取&#x200B;**密碼**&#x200B;選項。
   - 按一下&#x200B;**新增**&#x200B;按鈕。

   ![新增變數](../assets/how-to/add-cloud-manager-secrete-variable.png)

1. 重複上述步驟，為`purgeKey2`值建立第二個環境變數。

1. 按一下[儲存]以儲存並套用變更。****

### 部署CDN規則

最後，透過Cloud Manager管道將已設定的CDN規則部署至AEM as a Cloud Service環境。

1. 在Cloud Manager中，導覽至&#x200B;**管道**&#x200B;區段。

1. 建立新管道或選取僅部署&#x200B;**Config**&#x200B;檔案的現有管道。 如需詳細步驟，請參閱[建立設定管道](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager)。

1. 按一下&#x200B;**執行**&#x200B;按鈕以部署CDN規則。

   ![執行管道](../assets/how-to/run-config-pipeline.png)

## 使用清除API Token

若要清除CDN快取，請使用清除API權杖叫用AEM服務特定的網域URL。 清除快取的語法如下：

```
PURGE <URL> HTTP/1.1
Host: <AEM_SERVICE_SPECIFIC_DOMAIN>
X-AEM-Purge-Key: <PURGE_API_TOKEN>
X-AEM-Purge: <PURGE_TYPE>
Surrogate-Key: <SURROGATE_KEY>
```

其中：

- **清除`<URL>`**： `PURGE`方法後面接著您要清除之資源的URL路徑。
- **主機：`<AEM_SERVICE_SPECIFIC_DOMAIN>`**：它指定AEM服務的網域。
- **X-AEM-Purge-Key：`<PURGE_API_TOKEN>`**：包含清除API Token值的自訂標頭。
- **X-AEM-Purge：`<PURGE_TYPE>`**：指定清除作業型別的自訂標頭。 值可以是`hard`、`soft`或`all`。 下表說明每種永久刪除型別：

  | 清除型別 | 說明 |
  |:------------:|:-------------:|
  | hard （預設） | 立即移除快取的資源。 避免使用它，因為它會增加流向原始伺服器的流量。 |
  | 柔和 | 將快取的資源標示為過時，並從原始伺服器擷取最新版本。 |
  | 全部 | 從CDN快取中移除所有快取的資源。 |

- **Surrogate-Key：`<SURROGATE_KEY>`**： （選擇性）指定要清除之資源群組的代理金鑰（以空格分隔）的自訂標頭。 替代索引鍵是用來將資源分組，且必須在資源的回應標題中設定。

>[!TIP]
>
>在下列範例中，`X-AEM-Purge: hard`是用於示範用途。 您可以根據您的需求，以`soft`或`all`取代。 使用`hard`清除型別時，請務必小心，因為它會增加原始伺服器的流量。

### 清除特定資源的快取

在此範例中，在AEM as a Cloud Service環境中部署的WKND站台上，`curl`命令會清除`/us/en.html`資源的快取。

```bash
curl -X PURGE "https://publish-p46652-e1315806.adobeaemcloud.com/us/en.html" \
-H "X-AEM-Purge-Key: 123456789" \
-H "X-AEM-Purge: hard"
```

成功清除後，會傳回JSON內容的`200 OK`回應。

```json
{ "status": "ok", "id": "1000098-1722961031-13237063" }
```

### 清除資源群組的快取

在此範例中，`curl`命令會清除具有代理機碼`wknd-assets`的資源群組的快取。 `Surrogate-Key`回應標頭設定在[`wknd.vhost`](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost#L176)中，例如：

```http
<VirtualHost *:80>
    ...

    # Core Component Image Component: long-term caching (30 days) for immutable URLs, background refresh to avoid MISS
    <LocationMatch "^/content/.*\.coreimg.*\.(?i:jpe?g|png|gif|svg)$">
        Header set Cache-Control "max-age=2592000,stale-while-revalidate=43200,stale-if-error=43200,public,immutable" "expr=%{REQUEST_STATUS} < 400"
        # Set Surrogate-Key header to group the cache of WKND assets, thus it can be flushed independtly
        Header set Surrogate-Key "wknd-assets"
        Header set Age 0
    </LocationMatch>

    ...
</VirtualHost>
```

```bash
curl -X PURGE "https://publish-p46652-e1315806.adobeaemcloud.com" \
-H "Surrogate-Key: wknd-assets" \
-H "X-AEM-Purge-Key: 123456789" \
-H "X-AEM-Purge: hard"
```

成功清除後，會傳回JSON內容的`200 OK`回應。

```json
{ "wknd-assets": "10027-1723478994-2597809-1" }
```

### 清除整個快取

在此範例中，使用`curl`命令會從部署在AEM as a Cloud Service環境上的範例WKND網站中清除整個快取。

```bash
curl -X PURGE "https://publish-p46652-e1315806.adobeaemcloud.com/" \
-H "X-AEM-Purge-Key: 123456789" \
-H "X-AEM-Purge: all"
```

成功清除後，會傳回JSON內容的`200 OK`回應。

```json
{"status":"ok"}
```

### 驗證快取清除

若要驗證快取清除，請存取網頁瀏覽器中的資源URL並檢閱回應標頭。 `X-Cache`標頭值應為`MISS`。

![X-Cache標頭](../assets/how-to/x-cache-miss.png)
