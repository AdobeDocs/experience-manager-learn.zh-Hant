---
title: GraphQL的CORS配AEM置
description: 瞭解如何配置跨源資源共用(CORS)以與AEMGraphQL一起使用。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10830
thumbnail: KT-10830.jpg
exl-id: 394792e4-59c8-43c1-914e-a92cdfde2f8a
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '619'
ht-degree: 1%

---

# 跨源資源共用(CORS)

Adobe Experience Manager as a Cloud Service的跨源資源共用(CORS)方便了非AEMweb屬性，可以對GraphQLAPI進行基於瀏覽器的客AEM戶端調用。

以下文章介紹如何配置 _單原點_ 通過CORS訪問特定AEM的無頭端點集。 單原點表示只有單個非AEM域訪AEM問，例如，https://app.example.com連接到https://www.example.com。 由於快取問題，使用此方法時，多源訪問可能無效。

>[!TIP]
>
> 以下配置是示例。 確保根據項目要求調整它們。

## CORS要求

當連接到的客戶端未從與相同的源（也稱為主機或域）提供服務AEM時，基於瀏覽器的到GraphQLAPI的連接需要CORSAEM。

| 客戶端類型 | [單頁應用(SPA)](../spa.md) | [Web元件/JS](../web-component.md) | [行動](../mobile.md) | [伺服器到伺服器](../server-to-server.md) |
|----------------------------:|:---------------------:|:-------------:|:---------:|:----------------:|
| 需要CORS配置 | ✔ | ✔ | ✘ | ✘ |

## OSGi配置

CORS OSGi配AEM置工廠定義了接受CORS HTTP請求的允許條件。

| 客戶端連接到 | AEM 作者 | AEM 發佈 | 預AEM覽 |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| 需要CORS OSGi配置 | ✔ | ✔ | ✔ |


下面的示例定義AEM發佈的OSGi配置(`../config.publish/..`)，但可以添加到 [任何受支援的運行模式資料夾](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#runmode-resolution)。

關鍵配置屬性包括：

+ `alloworigin` 和/或 `alloworiginregexp` 指定客戶端連接到WebAEM的源。
+ `allowedpaths` 指定從指定的源允許的URL路徑模式。
   + 要支援AEMGraphQL永續查詢，請添加以下模式： `/graphql/execute.json.*`
   + 要支援體驗片段，請添加以下模式： `/content/experience-fragments/.*`
+ `supportedmethods` 指定CORS請求允許的HTTP方法。 添加 `GET`，以支援AEMGraphQL永續查詢（和經驗片段）。

[瞭解有關CORS OSGi配置的詳細資訊。](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html)

此示例配置支援使用AEMGraphQL永續查詢。 要使用客戶端定義的GraphQL查詢，請在 `allowedpaths` 和 `POST` 至 `supportedmethods`。

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

### 授權AEM的GraphQLAPI請求

當訪問AEM需要授權的GraphQLAPI（通常為AEM作者或AEM發佈上的受保護內容）時，確保CORS OSGi配置具有以下附加值：

+ `supportedheaders` 還有 `"Authorization"`
+ `supportscredentials` 設定為 `true`

對需要CORS配AEM置的GraphQLAPI的授權請求是不尋常的，因為通常在 [伺服器到伺服器應用](../server-to-server.md) 因此，不需要CORS配置。 需要CORS配置的基於瀏覽器的應用，如 [單頁應用](../spa.md) 或 [Web元件](../web-component.md)，通常使用授權，因為很難保護憑據。

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

#### OSGi配置示例

+ [OSGi配置的示例可在WKND項目中找到。](https://github.com/adobe/aem-guides-wknd/blob/main/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json)

## Dispatcher 設定

必須將AEM發佈（和預覽）服務的Dispatcher配置為支援CORS。

| 客戶端連接到 | AEM 作者 | AEM 發佈 | 預AEM覽 |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| 需要Dispatcher CORS配置 | ✘ | ✔ | ✔ |

### 允許HTTP請求上的CORS標頭

更新 `clientheaders.any` 允許HTTP請求標頭的檔案 `Origin`。  `Access-Control-Request-Method`, `Access-Control-Request-Headers` 傳遞到AEM，允許HTTP請求由 [AEM CORS配置](#osgi-configuration)。

`dispatcher/src/conf.dispatcher.d/clientheaders/clientheaders.any`

```
# Allowing CORS headers to be passed through to the publish tier to support headless and SPA Editor use cases.
# https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#the_http_request_headers

"Origin"
"Access-Control-Request-Method"
"Access-Control-Request-Headers"

$include "./default_clientheaders.any"
```

#### Dispatcher配置示例

+ [Dispatcher的示例 _客戶端標頭_ 可在WKND項目中找到配置。](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/clientheaders/clientheaders.any#L10-L12)


### 傳遞CORS HTTP響應標頭

將Dispatcher場配置為快取 **CORS HTTP響應標頭** 通過添加Dispatcher快取，確保在從Dispatcher緩AEM存中提供GraphQL永續查詢時包括這些查詢 `Access-Control-...` 標題到快取標題清單。

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

#### Dispatcher配置示例

+ [Dispatcher的示例 _CORS HTTP響應標頭_ 可在WKND項目中找到配置。](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/available_farms/wknd.farm#L109-L114)
