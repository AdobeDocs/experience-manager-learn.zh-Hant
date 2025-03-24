---
title: 執行批次設定
description: 執行批次以啟動檔案產生程式
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Output Service
topic: Development
jira: KT-9674
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: 17f91f81-96d8-49d6-b1e3-53d8899695ae
duration: 219
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '87'
ht-degree: 0%

---

# 執行批次設定

若要執行批次，請向以下API發出POST請求

```xml
<baseURL>/confi/<configName>/execution
```

此API預期空白json物件為請求內文中的引數。
此API在回應標頭中傳回唯一URL，由**位置**金鑰識別。
對此唯一URL的GET請求會告知您批次執行的狀態

以下影片示範批次設定的觸發

>[!VIDEO](https://video.tv.adobe.com/v/340242?quality=12&learn=on)
