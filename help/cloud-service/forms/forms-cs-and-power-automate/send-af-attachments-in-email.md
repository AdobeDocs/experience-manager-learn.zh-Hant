---
title: 在電子郵件中發送表單附件
description: 使用自動化工作流提取併發送電子郵件中提交的表單附件
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 11077
exl-id: 1be90d9b-3669-44a0-84fb-cbdec44074d8
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '72'
ht-degree: 0%

---

# 從提交的表單資料中提取表單附件

在Power中提取表單附件並在電子郵件中發送附件，使工作流自動化。
以下視頻說明了從提交的資料中建立附件所需的步驟。
>[!VIDEO](https://video.tv.adobe.com/v/3409017?quality=12&learn=on)

以下是在「分析JSON架構」步驟中需要使用的附件對象架構

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
