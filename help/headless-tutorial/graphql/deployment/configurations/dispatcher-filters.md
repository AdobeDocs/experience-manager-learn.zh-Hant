---
title: GraphQL的調度AEM器篩選器
description: 瞭解如何配置AEM發佈調度程式篩選器以與AEMGraphQL一起使用。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10829
thumbnail: kt-10829.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 2%

---


# 調度器篩選器

Adobe Experience Manager as a Cloud Service使用AEM發佈調度程式篩選器來確保只能訪問到AEM的請求AEM。 預設情況下，所有請求都被拒絕，並且必須顯式添加允許的URL的模式。

| 客戶端類型 | [單頁應用(SPA)](../spa.md) | [Web元件/JS](../web-component.md) | [行動](../mobile.md) | [伺服器到伺服器](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| 需要Dispatcher篩選器配置 | ✔ | ✔ | ✔ | ✔ |

>[!TIP]
>
> 以下配置是示例。 確保根據項目要求調整它們。

## 調度器篩選器配置

AEM發佈調度程式篩選器配置定義了允許訪問的URL模式AEM，並且必須包括永續查詢終結點AEM的URL前置詞。

| 客戶端連接到 | AEM 作者 | AEM 發佈 | 預AEM覽 |
|------------------------------------------:|:----------:|:-------------:|:-------------:|
| 需要Dispatcher篩選器配置 | ✘ | ✔ | ✔ |

添加 `allow` 具有URL模式的規則 `/graphql/execute.json/*`，並確保檔案ID(例如 `/0600`，在示例場檔案中是唯一的)。
這允許對永續查詢終結點的HTTPGET請求，如 `HTTP GET /graphql/execute.json/wknd-shared/adventures-all` 到AEM發佈。

+ `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...
/0600 { /type "allow" /url "/graphql/execute.json/*" }
...
```

### 示例篩選器配置

+ [可以在WKND項目中找到Dispatcher篩選器的示例。](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/filters/filters.any#L28)