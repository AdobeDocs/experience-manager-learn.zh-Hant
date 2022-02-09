---
title: 配置批資料配置
description: 配置批資料配置
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
kt: 9673
source-git-commit: 228da29e7ac0d61359c2b94131495b5b433a09dc
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 0%

---

# 建立批配置

要使用批處理API，請建立批配置並基於該配置執行運行。 以下視頻顯示了使用API建立批配置的演示

>[!NOTE]
>請確保用AEM戶屬於 ```forms-users``` 進行API調用的組。


>[!VIDEO](https://video.tv.adobe.com/v/340241/?quality=12&learn=on)

## 建立批配置

以下是用於建立批配置的POST終結點

```xml
<baseURL>/config
```

以下是建立批處理配置時需要指定的最小配置。 這需要作為JSON對象在HTTP請求正文中傳遞

```
{
	"configName": "monthlystatements",
	"dataSourceConfigUri": "/conf/batchapi/settings/forms/usc/batch/batchapitutorial",
	"outputTypes": [
		"PDF"
	],
	"template": "crx:///content/dam/formsanddocuments/formtemplates/custom_fonts.xdp"

}
```

## 驗證批配置

要驗證批配置的建立是否成功，可以對以下終結點進行GET請求調用


```xml
<baseURL>/config/monthlystatements
```

您只需在HTTP請求正文中傳遞空JSON對象

