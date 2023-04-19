---
title: AEM GraphQL的CORS設定
description: 了解如何設定跨原始資源共用(CORS)以與AEM GraphQL搭配使用。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10830
thumbnail: KT-10830.jpg
source-git-commit: cc78e59fe70686e909928e407899fcf629a651b9
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---


# 跨原始資源共用(CORS)

Adobe Experience Manager as a Cloud Service的跨原始資源共用(CORS)有助於非AEM Web屬性對AEM GraphQL API進行瀏覽器式用戶端呼叫。

以下文章說明如何設定 _單原點_ 透過CORS存取特定的AEM無頭端點集。 單一來源表示僅有單一非AEM網域存取AEM，例如連線至https://www.example.com的https://app.example.com 。 由於快取問題，使用此方法可能無法存取多原始碼。

>[!TIP]
>
> 以下是範例設定。 請確定您根據專案需求進行調整。

## CORS需求

當連線至AEM GraphQL API的用戶端未從與AEM相同的來源（亦稱為主機或網域）提供時，以瀏覽器為基礎的連線至AEM API需要CORS。

| 用戶端類型 | [單頁應用程式(SPA)](../spa.md) | [Web元件/JS](../web-component.md) | [行動](../mobile.md) | [伺服器對伺服器](../server-to-server.md) |
|----------------------------:|:---------------------:|:-------------:|:---------:|:----------------:|
| 需要CORS設定 | ✔ | ✔ | ✘ | ✘ |

## OSGi配置

AEM CORS OSGi設定工廠定義接受CORS HTTP要求的允許條件。

| 客戶端連接到 | AEM 作者 | AEM 發佈 | AEM預覽 |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| 需要CORS OSGi設定 | ✔ | ✔ | ✔ |


以下範例定義AEM Publish的OSGi設定(`../config.publish/..`)，但可以新增至 [任何支援的運行模式資料夾](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#runmode-resolution).

關鍵配置屬性為：

+ `alloworigin` 和/或 `alloworiginregexp` 指定連接到AEM web的客戶端運行的原始項。
+ `allowedpaths` 指定從指定的原點允許的URL路徑模式。
   + 若要支援AEM GraphQL持續查詢，請新增下列模式： `/graphql/execute.json.*`
   + 若要支援體驗片段，請新增下列模式： `/content/experience-fragments/.*`
+ `supportedmethods` 指定CORS要求允許的HTTP方法。 新增 `GET`，以支援AEM GraphQL持續查詢（和體驗片段）。

[深入了解CORS OSGi設定。](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html)

此範例設定支援使用AEM GraphQL持續查詢。 若要使用用戶端定義的GraphQL查詢，請在 `allowedpaths` 和 `POST` to `supportedmethods`.

+ `/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~graphql.cfg.json`

```json
{
  "alloworigin":[
    "https://spa.external.com/"
  ],
  "alloworiginregexp":[
  ],
  "allowedpaths": [
    "/graphql/execute.json.*",
    "/content/experience-fragments/.*"
  ],
  "supportedheaders": [
    "Origin",
    "Accept",
    "X-Requested-With",
    "Content-Type",
    "Access-Control-Request-Method",
    "Access-Control-Request-Headers"
  ],
  "supportedmethods":[
    "GET",
    "HEAD",
    "OPTIONS"
  ],
  "maxage:Integer": 1800,
  "supportscredentials": false,
  "exposedheaders":[ "" ]
}
```

### 授權AEM GraphQL API請求

存取需要授權的AEM GraphQL API時（通常是AEM製作或AEM發佈上的受保護內容），請確定CORS OSGi設定有其他值：

+ `supportedheaders` 另外還有清單 `"Authorization"`
+ `supportscredentials` 設為 `true`

需要CORS設定的AEM GraphQL API的授權請求不尋常，因為這些請求通常發生在 [伺服器對伺服器應用程式](../server-to-server.md) 因此，不需要CORS設定。 需要CORS設定的瀏覽器型應用程式，例如 [單頁應用程式](../spa.md) 或 [Web元件](../web-component.md)，通常會使用授權，因為認證很難保護。

例如，這兩個設定在 `CORSPolicyImpl` OSGi工廠配置：

+ `/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config/com.adobe.granite.cors.impl.CORSPolicyImpl~graphql.cfg.json`

```json
{ 
  ...
  "supportedheaders": [
    "Origin",
    "Accept",
    "X-Requested-With",
    "Content-Type",
    "Access-Control-Request-Method",
    "Access-Control-Request-Headers",
    "Authorization"
  ],
  ...
  "supportscredentials": true,
  ...
}
```

#### 範例OSGi設定

+ [在WKND項目中可找到OSGi配置的示例。](https://github.com/adobe/aem-guides-wknd/blob/main/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json)

## Dispatcher 設定

AEM發佈（和預覽）服務的Dispatcher必須設定為支援CORS。

| 客戶端連接到 | AEM 作者 | AEM 發佈 | AEM預覽 |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| 需要Dispatcher CORS設定 | ✘ | ✔ | ✔ |

### 允許HTTP要求上的CORS標題

更新 `clientheaders.any` 允許HTTP要求標題的檔案 `Origin`,  `Access-Control-Request-Method`，和 `Access-Control-Request-Headers` 以傳遞至AEM，允許HTTP要求由 [AEM CORS設定](#osgi-configuration).

`dispatcher/src/conf.dispatcher.d/clientheaders/clientheaders.any`

```
# Allowing CORS headers to be passed through to the publish tier to support headless and SPA Editor use cases.
# https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#the_http_request_headers

"Origin"
"Access-Control-Request-Method"
"Access-Control-Request-Headers"

$include "./default_clientheaders.any"
```

#### Dispatcher設定範例

+ [Dispatcher的範例 _用戶頭_ 可在WKND項目中找到配置。](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/clientheaders/clientheaders.any#L10-L12)


### 傳送CORS HTTP回應標題

將Dispatcher伺服器陣列設定為快取 **CORS HTTP回應標題** 以確保在從Dispatcher快取提供AEM GraphQL持續查詢時納入，方法是新增 `Access-Control-...` 標題至快取標題清單。

+ `dispatcher/src/conf.dispatcher.d/available_farms/wknd.farm`

```
/publishfarm {
    ...
    /cache {
        ...
        # CORS HTTP response headers
        # https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#the_http_response_headers
        /headers {
            ...
            "Access-Control-Allow-Origin"
            "Access-Control-Expose-Headers"
            "Access-Control-Max-Age"
            "Access-Control-Allow-Credentials"
            "Access-Control-Allow-Methods"
            "Access-Control-Allow-Headers"
        }
    ...
    }
...
}
```

#### Dispatcher設定範例

+ [Dispatcher的範例 _CORS HTTP回應標題_ 可在WKND項目中找到配置。](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/available_farms/wknd.farm#L109-L114)
