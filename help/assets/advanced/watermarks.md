---
title: AEM Assets水印
description: 由於AEMCloud Service的浮水印功能允許使用任何PNG影像對自訂影像轉譯加上浮水印。
feature: Asset Compute Microservices
version: cloud-service
kt: 6357
thumbnail: 41536.jpg
topic: Content Management
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '65'
ht-degree: 1%

---


# 浮水印

由於AEMCloud Service的浮水印功能允許使用任何PNG影像對自訂影像轉譯加上浮水印。

>[!VIDEO](https://video.tv.adobe.com/v/41536/?quality=12&learn=on)

## OSGi配置

可以更新下列OSGi配置存根，並將其添AEM加到項目的`ui.config`子項目中。

`/apps/example/osgiconfig/config.author/com.adobe.cq.assetcompute.impl.profile.WatermarkingProfileServiceImpl.cfg.json`

```json
{
    "watermark": "/content/dam/path/to/watermark.png",
     "scale": 1.0
}
```
