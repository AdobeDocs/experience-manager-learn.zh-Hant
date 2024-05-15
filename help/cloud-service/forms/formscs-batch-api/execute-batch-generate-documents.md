---
title: 執行批次設定
description: 執行批次以啟動檔案產生程式
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
jira: KT-9674
exl-id: 17f91f81-96d8-49d6-b1e3-53d8899695ae
duration: 219
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '81'
ht-degree: 0%

---

# 執行批次設定

若要執行批次，請向以下API發出POST請求

```xml
<baseURL>/confi/<configName>/execution
```

此API預期空白json物件為請求內文中的引數。
此API會傳回回應標題中的唯一URL，識別方式為 **位置** 機碼。
這個唯一URL的GET要求會告訴您批次執行的狀態

以下影片示範批次設定的觸發

>[!VIDEO](https://video.tv.adobe.com/v/340242?quality=12&learn=on)
