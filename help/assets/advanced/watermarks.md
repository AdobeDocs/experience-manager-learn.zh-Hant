---
title: AEM Assets中的水印
description: AEM as aCloud Service的浮水印功能允許使用任何PNG影像對自訂影像轉譯加上水印。
feature: asset compute微服務
version: cloud-service
kt: 6357
thumbnail: 41536.jpg
topic: 內容管理
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '63'
ht-degree: 0%

---


# 水印

AEM as aCloud Service的浮水印功能允許使用任何PNG影像對自訂影像轉譯加上水印。

>[!VIDEO](https://video.tv.adobe.com/v/41536/?quality=12&learn=on)

## OSGi配置

可以更新以下OSGi配置存根，並將其添加到AEM項目的`ui.config`子項目中。

`/apps/example/osgiconfig/config.author/com.adobe.cq.assetcompute.impl.profile.WatermarkingProfileServiceImpl.cfg.json`

```json
{
    "watermark": "/content/dam/path/to/watermark.png",
     "scale": 1.0
}
```
