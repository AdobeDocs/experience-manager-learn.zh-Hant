---
title: 以電子郵件傳送表單附件
description: 使用Power Automate工作流程在電子郵件中擷取並傳送提交的表單附件
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-11077
exl-id: 1be90d9b-3669-44a0-84fb-cbdec44074d8
duration: 391
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '78'
ht-degree: 0%

---

# 從提交的表單資料中擷取表單附件

在Power Automate工作流程中擷取表單附件並以電子郵件傳送附件。
以下影片說明從提交的資料中形成附件所需的步驟。
>[!VIDEO](https://video.tv.adobe.com/v/3409017?quality=12&learn=on)

以下是您需要在「剖析JSON」結構描述步驟中使用的附件物件結構描述

```json
{
    "type": "object",
    "properties": {
        "filename": {
            "type": "string"
        },
        "data": {
            "type": "string"
        },
        "contentType": {
            "type": "string"
        },
        "size": {
            "type": "integer"
        }
    }
}
```
