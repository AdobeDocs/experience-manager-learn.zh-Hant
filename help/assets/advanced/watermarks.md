---
title: AEM Assets中的浮水印
description: AEM做為Cloud Service的浮水印功能可讓自訂影像轉譯使用任何PNG影像加上浮水印。
feature: watermark
topics: images
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6357
thumbnail: 41536.jpg
translation-type: tm+mt
source-git-commit: 10784dce34443adfa1fc6dc324242b1c021d2a17
workflow-type: tm+mt
source-wordcount: '58'
ht-degree: 0%

---


# 浮水印

AEM做為Cloud Service的浮水印功能可讓自訂影像轉譯使用任何PNG影像加上浮水印。

>[!VIDEO](https://video.tv.adobe.com/v/41536/?quality=12&learn=on)

## OSGi配置

您可以更新下列OSGi組態存根，並將其新增至AEM專案的`ui.config`子專案。

`/apps/example/osgiconfig/config.author/com.adobe.cq.assetcompute.impl.profile.WatermarkingProfileServiceImpl.cfg.json`

```json
{
    "watermark": "/content/dam/path/to/watermark.png",
     "scale": 1.0
}
```
