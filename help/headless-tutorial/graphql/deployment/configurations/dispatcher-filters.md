---
title: 適用於AEM GraphQL的Dispatcher篩選器
description: 瞭解如何設定AEM Publish Dispatcher篩選器，以與AEM GraphQL搭配使用。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10829
thumbnail: kt-10829.jpg
exl-id: b76b7c46-5cbd-4039-8fd6-9f0f10a4a84f
duration: 48
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 1%

---

# Dispatcher篩選器

Adobe Experience Manager as a Cloud Service使用AEM Publish Dispatcher篩選器，確保只有可連線AEM的要求才會連線AEM。 預設會拒絕所有要求，而且必須明確新增允許URL的模式。

| 使用者端型別 | [單頁應用程式(SPA)](../spa.md) | [網頁元件/JS](../web-component.md) | [行動裝置](../mobile.md) | [伺服器對伺服器](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| 需要Dispatcher篩選器設定 | ✔ | ✔ | ✔ | ✔ |

>[!TIP]
>
> 以下設定為範例。 請確定您調整這些值，以符合專案的需求。

## Dispatcher篩選器設定

AEM Publish Dispatcher篩選器設定會定義允許到達AEM的URL模式，且必須包含AEM持續查詢端點的URL首碼。

| 使用者端連線至 | AEM 作者 | AEM 發佈 | AEM預覽 |
|------------------------------------------:|:----------:|:-------------:|:-------------:|
| 需要Dispatcher篩選器設定 | ✘ | ✔ | ✔ |

新增URL模式為`/graphql/execute.json/*`的`allow`規則，並確認檔案識別碼（例如`/0600`，在範例伺服器陣列檔案中是唯一的）。
這允許對持久查詢端點發出HTTPGET要求，例如`HTTP GET /graphql/execute.json/wknd-shared/adventures-all`通過到AEM Publish。

如果在您的AEM Headless體驗中使用體驗片段，請對這些路徑執行相同的操作。

+ `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...
# Allow headless requests for Persisted Query endpoints
/0600 { /type "allow" /method '(POST|OPTIONS)' /url "/graphql/execute.json/*" }
# Allow headless requests for Experience Fragments
/0601 { /type "allow" /method '(GET|OPTIONS)' /url "/content/experience-fragments/*" }
...
```

### 篩選設定範例

+ [在WKND專案中可以找到Dispatcher篩選的範例。](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/filters/filters.any#L28)
