---
title: GraphQL的CORSAEM配置
description: 瞭解如何配置跨源資源共用(CORS)以與AEMGraphQL一起使用。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10587
thumbnail: KT-10587.jpg
mini-toc-levels: 2
source-git-commit: 56831a30a442388e34d8388546787ed22e7577f5
workflow-type: tm+mt
source-wordcount: '362'
ht-degree: 2%

---


# 跨源資源共用(CORS)

Adobe Experience Manager as a Cloud Service的跨源資源共用(CORS)方便了非AEMweb屬性對GraphQL API進行基於瀏覽器的客戶端AEM調用。

>[!TIP]
>
> 以下配置是示例。 確保根據項目要求調整它們。



## CORS要求

當連接到的客戶端未從與AEMGraphQL API相同的源（也稱為主機或域）提供服務時，基於瀏覽器的連AEM接需要CORSAEM。

| 客戶端類型 | 單頁應用(SPA) | Web元件/JS | 行動 | 伺服器到伺服器 |
|----------------------------:|:---------------------:|:-------------:|:---------:|:----------------:|
| 需要CORS配置 | ✔ | ✔ | ✘ | ✘ |

## OSGi配置

CORS OSGi配AEM置工廠定義了接受CORS HTTP請求的允許條件。

| 客戶端連接到 | AEM 作者 | AEM 發佈 | 預AEM覽 |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| 需要CORS OSGi配置 | ✔ | ✔ | ✔ |


下面的示例定義AEM發佈的OSGi配置(`../config.publish/..`)，但可以添加到 [任何受支援的runmode資料夾](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#runmode-resolution)。

關鍵配置屬性包括：

+ `alloworigin` 和/或 `alloworiginregexp` 指定客戶端連接到Web的源AEM於上運行。
+ `allowedpaths` 指定指定從指定的源允許的URL路徑模式。 要支援AEMGraphQL永續查詢，請使用以下模式： `"/graphql/execute.json.*"`
+ `supportedmethods` 指定CORS請求允許的HTTP方法。 添加 `GET`，以支援AEMGraphQL永續查詢。

[瞭解有關CORS OSGi配置的詳細資訊。](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html)


此示例配置支援使用AEMGraphQL永續查詢。 要使用客戶端定義的GraphQL查詢，請在 `allowedpaths` 和 `POST` 至 `supportedmethods`。

+ `/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~graphql.cfg.json`

```json
{
  "alloworigin":[
    "https://spa.external.com/"
  ],
  "alloworiginregexp":[
    "http://localhost:.*"
  ],
  "allowedpaths": [
    "/graphql/execute.json.*"
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
    "HEAD"
  ],
  "maxage:Integer": 1800,
  "supportscredentials": false,
  "exposedheaders":[ "" ]
}
```


## 調度程式配置

必須將AEM發佈（和預覽）服務的Dispatcher配置為支援CORS。

| 客戶端連接到 | AEM 作者 | AEM 發佈 | 預AEM覽 |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| 需要Dispatcher CORS配置 | ✘ | ✔ | ✔ |

### 允許HTTP請求上的CORS標頭

更新 `clientheaders.any` 允許HTTP請求標頭的檔案 `Origin`。  `Access-Control-Request-Method` 和 `Access-Control-Request-Headers` 傳遞到AEM，允許HTTP請求由 [AEM CORS配置](#osgi-configuration)。

`dispatcher/src/conf.dispatcher.d/clientheaders/clientheaders.any`

```
# Allowing CORS headers to be passed through to the publish tier to support headless and SPA Editor use cases.
# https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#the_http_request_headers

"Origin"
"Access-Control-Request-Method"
"Access-Control-Request-Headers"

$include "./default_clientheaders.any"
```

### 傳遞CORS HTTP響應標頭

將調度器場配置為快取 **CORS HTTP響應標頭** 通過添加Dispatcher緩AEM存來提供GraphQL永續查詢時，確保包含這些查詢 `Access-Control-...` 標題到快取標題清單。

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
