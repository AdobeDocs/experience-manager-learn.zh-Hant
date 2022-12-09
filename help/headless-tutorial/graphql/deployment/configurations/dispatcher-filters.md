---
title: AEM GraphQL的Dispatcher篩選器
description: 了解如何設定AEM Publish Dispatcher篩選器以搭配AEM GraphQL使用。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10829
thumbnail: kt-10829.jpg
source-git-commit: 442020d854d8f42c5d8a1340afd907548875866e
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 2%

---


# Dispatcher篩選器

Adobe Experience Manager as a Cloud Service使用AEM Publish Dispatcher篩選器，確保只有應到達AEM的請求才能到達AEM。 依預設，會拒絕所有要求，且必須明確新增允許URL的模式。

| 用戶端類型 | [單頁應用程式(SPA)](../spa.md) | [Web元件/JS](../web-component.md) | [行動](../mobile.md) | [伺服器對伺服器](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| 需要Dispatcher篩選器設定 | ✔ | ✔ | ✔ | ✔ |

>[!TIP]
>
> 以下是範例設定。 請確定您根據專案需求進行調整。

## Dispatcher篩選器設定

AEM Publish Dispatcher篩選器設定會定義可存取AEM的URL模式，且必須包含AEM持續查詢端點的URL首碼。

| 客戶端連接到 | AEM 作者 | AEM 發佈 | AEM預覽 |
|------------------------------------------:|:----------:|:-------------:|:-------------:|
| 需要Dispatcher篩選器設定 | ✘ | ✔ | ✔ |

新增 `allow` URL模式的規則 `/graphql/execute.json/*`，並確保檔案ID(例如 `/0600`，在範例伺服器陣列檔案中是唯一的)。
這可讓HTTPGET要求傳送至持續的查詢端點，例如 `HTTP GET /graphql/execute.json/wknd-shared/adventures-all` 到AEM發佈。

如果在您的AEM無頭體驗中使用體驗片段，請對這些路徑執行相同操作。

+ `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...
# Allow headless requests for Persisted Query endpoints
/0600 { /type "allow" /method '(POST|OPTIONS)' /url "/graphql/execute.json/*" }
# Allow headless requests for Experience Fragments
/0601 { /type "allow" /method '(GET|OPTIONS)' /url "/content/experience-fragments/*" }
...
```

### 篩選器設定範例

+ [可在WKND專案中找到Dispatcher篩選器的範例。](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/filters/filters.any#L28)
