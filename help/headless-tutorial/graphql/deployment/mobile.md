---
title: 無AEM頭移動部署
description: null
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: null
thumbnail: KT-.jpg
mini-toc-levels: 2
source-git-commit: 10032375a23ba99f674fb59a6f6e48bf93908421
workflow-type: tm+mt
source-wordcount: '83'
ht-degree: 4%

---


# 無AEM頭移動部署

無AEM頭移動部署是iOS、安卓等的本機移動應用。 以無頭方式消費和AEM與內容交互。

移動部署需要最少的配置，AEM因為到無頭API的HTTP連接不是在瀏覽器上下文中啟動的。

## 部署配置

| 移動應用連接到 | AEM 作者 | AEM 發佈 | 預AEM覽 |
|-----------------------:|:----------:|:-----------:|:-----------:|
| [調度器篩選器](./dispatcher-fitlers.md) | ✘ | ✔ | ✔ |
| [CORS配置](./cors.md) | ✘ | ✘ | ✘ |
| 影像URL主機名 | ✔ | ✔ | ✔ |

## 移動應用示例

Adobe提供了iOS和Android移動應用的示例。


