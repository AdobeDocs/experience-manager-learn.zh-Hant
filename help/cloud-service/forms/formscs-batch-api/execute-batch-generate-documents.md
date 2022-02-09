---
title: 執行批配置
description: 通過執行批來啟動文檔生成過程
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
kt: 9674
source-git-commit: 228da29e7ac0d61359c2b94131495b5b433a09dc
workflow-type: tm+mt
source-wordcount: '81'
ht-degree: 0%

---

# 執行批配置

要運行批處理，請向以下API發出POST請求

```xml
<baseURL>/confi/<configName>/execution
```

此API要求一個空json對象作為請求體中的參數。
此API返回由標識的響應標頭中的唯一URL **位置** 按鈕
對此唯一URL的GET請求將告訴您批執行的狀態

以下視頻演示了批配置的觸發

>[!VIDEO](https://video.tv.adobe.com/v/340242/?quality=12&learn=on)
