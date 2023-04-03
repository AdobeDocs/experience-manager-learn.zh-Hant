---
title: 執行批配置
description: 通過執行批處理來啟動文檔生成過程
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
kt: 9674
exl-id: 17f91f81-96d8-49d6-b1e3-53d8899695ae
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '81'
ht-degree: 0%

---

# 執行批配置

若要執行批次，請向下列API提出POST要求

```xml
<baseURL>/confi/<configName>/execution
```

此API需要空的json物件作為要求內文中的參數。
此API會在回應標題中傳回唯一URL，識別為 **位置** 鍵。
對此唯一URL的GET請求將告知您批次執行的狀態

以下影片示範批次設定的觸發方式

>[!VIDEO](https://video.tv.adobe.com/v/340242?quality=12&learn=on)
