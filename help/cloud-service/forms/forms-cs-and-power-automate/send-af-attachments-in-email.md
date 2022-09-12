---
title: 在電子郵件中發送表單附件
description: 使用強大的自動化工作流程，在電子郵件中擷取並傳送已提交的表單附件
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 11077
source-git-commit: aea43a705b3959f8be26238d32b816b3953e153e
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# 從提交的表單資料擷取表單附件

提取表單附件，並以電子郵件傳送附件，讓工作流程自動化。
以下影片說明從提交的資料建立附件所需的步驟。
>[!VIDEO](https://video.tv.adobe.com/v/3409017/?quality=12&learn=on)

以下是您需要在剖析JSON結構步驟中使用的附件物件結構

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
