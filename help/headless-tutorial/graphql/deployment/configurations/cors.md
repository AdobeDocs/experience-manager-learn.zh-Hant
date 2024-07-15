---
title: AEM GraphQL的CORS設定
description: 瞭解如何設定跨原始資源共用(CORS)以搭配AEM GraphQL使用。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10830
thumbnail: KT-10830.jpg
exl-id: 394792e4-59c8-43c1-914e-a92cdfde2f8a
last-substantial-update: 2024-03-22T00:00:00Z
duration: 184
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '603'
ht-degree: 1%

---

# 跨原始資源共用(CORS)

Adobe Experience Manager as a Cloud Service的跨原始資源共用(CORS)可協助非AEM Web屬性對AEM的GraphQL API和其他AEM Headless資源進行瀏覽器型使用者端呼叫。

>[!TIP]
>
> 以下設定為範例。 請確定您調整這些值，以符合專案的需求。

## CORS需求

當「不」從與AEM相同的來源（也稱為主機或網域）提供連線至AEM的使用者端時，以瀏覽器為基礎的連線至AEM GraphQL API需要CORS。

| 使用者端型別 | [單頁應用程式(SPA)](../spa.md) | [網頁元件/JS](../web-component.md) | [行動裝置](../mobile.md) | [伺服器對伺服器](../server-to-server.md) |
|----------------------------:|:---------------------:|:-------------:|:---------:|:----------------:|
| 需要CORS設定 | ✔ | ✔ | ✘ | ✘ |

## AEM 作者

在AEM Author服務上啟用CORS與AEM Publish和AEM Preview服務不同。 AEM Author服務需要將OSGi設定新增到AEM Author服務的執行模式資料夾，而且不會使用Dispatcher設定。

### OSGi設定

AEM CORS OSGi Configuration Factory會定義接受CORS HTTP要求的允許條件。

| 使用者端連線至 | AEM 作者 | AEM 發佈 | AEM預覽 |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| 需要CORS OSGi設定 | ✔ | ✘ | ✘ |


以下範例為AEM Author (`../config.author/..`)定義OSGi設定，因此它僅在AEM Author服務上有效。

主要組態屬性為：

+ `alloworigin`和/或`alloworiginregexp`指定連線至AEM Web執行之使用者端的來源。
+ `allowedpaths`指定允許來自指定來源的URL路徑模式。
   + 若要支援AEM GraphQL持續查詢，請新增下列模式： `/graphql/execute.json.*`
   + 若要支援體驗片段，請新增以下模式： `/content/experience-fragments/.*`
+ `supportedmethods`指定允許用於CORS要求的HTTP方法。 若要支援AEM GraphQL持續查詢（和體驗片段），請新增`GET` 。
+ `supportedheaders`包含`"Authorization"`，因為應該授權給AEM作者的要求。
+ `supportscredentials`已設為`true`，因為對AEM Author的要求應該獲得授權。

[進一步瞭解CORS OSGi設定。](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html)

以下範例支援在AEM Author上使用AEM GraphQL持續查詢。 若要使用使用者端定義的GraphQL查詢，請在`allowedpaths`和`POST`中新增GraphQL端點URL至`supportedmethods`。

+ `/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config.author/com.adobe.granite.cors.impl.CORSPolicyImpl~graphql.cfg.json`

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
    "Access-Control-Request-Headers",
    "Authorization"
  ],
  "supportedmethods":[
    "GET",
    "HEAD",
    "POST"
  ],
  "maxage:Integer": 1800,
  "supportscredentials": true,
  "exposedheaders":[ "" ]
}
```

#### OSGi設定範例

+ [在WKND專案中可以找到OSGi設定的範例。](https://github.com/adobe/aem-guides-wknd/blob/main/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.author/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json)

## AEM 發佈

在AEM Publish （和預覽）服務上啟用CORS與AEM Author服務不同。 AEM Publish服務需要將AEM Dispatcher設定新增到AEM Publish的Dispatcher設定。 AEM Publish不使用[OSGi設定](#osgi-configuration)。

在AEM Publish上設定CORS時，請確定：

+ 無法透過從AEM Publish專案的`clientheaders.any`檔案中移除`Origin`標頭（如果先前新增）來將`Origin` HTTP請求標頭傳送至AEM Dispatcher服務。 應從`clientheaders.any`檔案中移除任何`Access-Control-`標頭，由Dispatcher管理，而非AEM Publish服務。
+ 如果您的AEM Publish服務已啟用任何[CORS OSGi設定](#osgi-configuration)，您必須將其移除，並將其設定移轉至下列的[Dispatcher vhost設定](#set-cors-headers-in-vhost)。

### Dispatcher設定

AEM Publish （和預覽）服務的Dispatcher必須設定為支援CORS。

| 使用者端連線至 | AEM 作者 | AEM 發佈 | AEM預覽 |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| 需要Dispatcher CORS設定 | ✘ | ✔ | ✔ |

#### 在vhost中設定CORS標頭

1. 在您的Dispatcher設定專案中開啟AEM Publish服務的vhost設定檔案（通常在`dispatcher/src/conf.d/available_vhosts/<example>.vhost`）
2. 將下列`<IfDefine ENABLE_CORS>...</IfDefine>`區塊的內容複製到您啟用的vhost組態檔中。

   ```{ highlight="17"}
   <VirtualHost *:80>
     ...
     <IfModule mod_headers.c>
         ################## Start of CORS configuration ##################
   
         SetEnvIfExpr "req_novary('Origin') == ''" CORSType=none CORSProcessing=false
         SetEnvIfExpr "req_novary('Origin') != ''" CORSType=cors CORSProcessing=true CORSTrusted=false
   
         SetEnvIfExpr "req_novary('Access-Control-Request-Method') == '' && %{REQUEST_METHOD} == 'OPTIONS' && req_novary('Origin') != ''" CORSType=invalidpreflight CORSProcessing=false
         SetEnvIfExpr "req_novary('Access-Control-Request-Method') != '' && %{REQUEST_METHOD} == 'OPTIONS' && req_novary('Origin') != ''" CORSType=preflight CORSProcessing=true CORSTrusted=false
         SetEnvIfExpr "req_novary('Origin') -strcmatch 'http://%{HTTP_HOST}*'" CORSType=samedomain CORSProcessing=false
         SetEnvIfExpr "req_novary('Origin') -strcmatch 'https://%{HTTP_HOST}*'" CORSType=samedomain CORSProcessing=false
   
         # For requests that require CORS processing, check if the Origin can be trusted
         SetEnvIfExpr "%{HTTP_HOST} =~ /(.*)/ " ParsedHost=$1
   
         ################## Adapt regex to match CORS origin(s) for your environment
         SetEnvIfExpr "env('CORSProcessing') == 'true' && req_novary('Origin') =~ m#(https://.*\.your-domain\.tld(:\d+)?$)#" CORSTrusted=true
   
         # Extract the Origin header
         SetEnvIfNoCase ^Origin$ ^(.*)$ CORSTrustedOrigin=$1
   
         # Flush If already set
         Header unset Access-Control-Allow-Origin
         Header unset Access-Control-Allow-Credentials
   
         # Trusted
         Header always set Access-Control-Allow-Credentials "true" "expr=reqenv('CORSTrusted') == 'true'"
         Header always set Access-Control-Allow-Origin "%{CORSTrustedOrigin}e" "expr=reqenv('CORSTrusted') == 'true'"
         Header always set Access-Control-Allow-Methods "GET" "expr=reqenv('CORSTrusted') == 'true'"
         Header always set Access-Control-Max-Age 1800 "expr=reqenv('CORSTrusted') == 'true'"
         Header always set Access-Control-Allow-Headers "Origin, Accept, X-Requested-With, Content-Type, Access-Control-Request-Method, Access-Control-Request-Headers" "expr=reqenv('CORSTrusted') == 'true'"
   
         # Non-CORS or Not Trusted
         Header unset Access-Control-Allow-Credentials "expr=reqenv('CORSProcessing') == 'false' || reqenv('CORSTrusted') == 'false'"
         Header unset Access-Control-Allow-Origin "expr=reqenv('CORSProcessing') == 'false' || reqenv('CORSTrusted') == 'false'"
         Header unset Access-Control-Allow-Methods "expr=reqenv('CORSProcessing') == 'false' || reqenv('CORSTrusted') == 'false'"
         Header unset Access-Control-Max-Age "expr=reqenv('CORSProcessing') == 'false' || reqenv('CORSTrusted') == 'false'"
   
         # Always vary on origin, even if its not there.
         Header merge Vary Origin
   
         # CORS - send 204 for CORS requests which are not trusted
         RewriteCond expr "reqenv('CORSProcessing') == 'true' && reqenv('CORSTrusted') == 'false'"
         RewriteRule "^(.*)" - [R=204,L]
   
         # Remove Origin before sending to AEM Publish if this configuration handles the CORS
         RequestHeader unset Origin "expr=reqenv('CORSTrusted') == 'true'"
   
         ################## End of CORS configuration ##################
     </IfModule>
     ...
   </VirtualHost>
   ```

3. 更新下行中的規則運算式，以符合存取您的AEM Publish服務的所需來源。 如果需要多個原點，請複製此線並更新每個原點/原點陣列。

   ```
   SetEnvIfExpr "env('CORSProcessing') == 'true' && req_novary('Origin') =~ m#(https://.*.your-domain.tld(:\d+)?$)#" CORSTrusted=true
   ```

   + 例如，若要啟用CORS從來源存取，請：

      + `https://example.com`上的任何子網域
      + `http://localhost`上的任何連線埠

     用以下兩行取代該行：

     ```
     SetEnvIfExpr "env('CORSProcessing') == 'true' && req_novary('Origin') =~ m#(https://.*\.example\.com$)#" CORSTrusted=true
     SetEnvIfExpr "env('CORSProcessing') == 'true' && req_novary('Origin') =~ m#(http://localhost(:\d+)?$)#" CORSTrusted=true
     ```

#### Dispatcher設定範例

+ [在WKND專案中可以找到Dispatcher設定的範例。](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost)
