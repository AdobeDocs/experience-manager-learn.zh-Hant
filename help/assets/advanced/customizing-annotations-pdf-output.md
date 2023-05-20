---
title: 自定義AEM Assets注釋
description: AEM Assets格式和樣式，當開發人員可以配置輸出到PDFAEM時。
feature: Collaboration
version: 6.4, 6.5
topic: Collaboration
role: Developer
level: Intermediate
last-substantial-update: 2022-06-03T00:00:00Z
exl-id: 972737dd-8ca6-47b4-a4ec-b73355c09cec
source-git-commit: f37483f90f2a707c906e1e206795fdebb5f698e9
workflow-type: tm+mt
source-wordcount: '60'
ht-degree: 0%

---

# 自定義AEM Assets注釋{#using-annotations-in-aem-assets}

支AEM持將注釋的輸出自定義為PDF。

## PDF注釋sling:OsgiConfig定義

要自定義PDF批注，請建立 **sling:OsgiConfig** 項目下AEM的節點

`/apps/my-project/config.author/com.day.cq.dam.core.impl.annotation.pdf.AnnotationPdfConfig.xml` 並根據需要調整值：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
    jcr:primaryType="sling:OsgiConfig"

    cq.dam.config.annotation.pdf.document.padding.vertical="{Long}20"
    cq.dam.config.annotation.pdf.reviewStatus.color.changesRequested="fad269"
    cq.dam.config.annotation.pdf.document.height="{Long}792"
    cq.dam.config.annotation.pdf.document.width="{Long}612"
    cq.dam.config.annotation.pdf.reviewStatus.width="{Long}150"
    cq.dam.config.annotation.pdf.font.size="{Long}7"
    cq.dam.config.annotation.pdf.font.color="3B3B3B"
    cq.dam.config.annotation.pdf.font.light="A4A4A4"
    cq.dam.config.annotation.pdf.reviewStatus.color.rejected="fa7d73"
    cq.dam.config.annotation.pdf.minImageHeight="{Long}100"
    cq.dam.config.annotation.pdf.reviewStatus.color.approved="009933"
    cq.dam.config.annotation.pdf.marginTextImage="{Long}14"
    cq.dam.config.annotation.pdf.document.padding.horizontal="{Long}20"
    cq.dam.config.annotation.pdf.annotationMarker.width="{Long}5"
    />
```
