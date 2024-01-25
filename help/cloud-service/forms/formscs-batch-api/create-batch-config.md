---
title: 設定批次資料設定
description: 設定批次資料設定
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
jira: KT-9673
exl-id: db25e5a2-e1a8-40ad-af97-35604d515450
duration: 237
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 0%

---

# 建立批次設定

若要使用批次API，請建立批次設定，並根據該設定執行執行。 以下影片示範如何使用API建立批次組態

>[!NOTE]
>請確定AEM使用者屬於 ```forms-users``` 群組進行API呼叫。


>[!VIDEO](https://video.tv.adobe.com/v/340241?quality=12&learn=on)

## 建立批次設定

以下是建立批次設定的POST端點

```xml
<baseURL>/config
```

以下是建立批次組態時需要指定的最小組態。 這必須在HTTP要求內文中以JSON物件形式傳遞

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

## 驗證批次設定

若要驗證已成功建立批次設定，您可以對以下端點進行GET請求呼叫


```xml
<baseURL>/config/monthlystatements
```

您只需在HTTP請求內文中傳遞空白JSON物件
