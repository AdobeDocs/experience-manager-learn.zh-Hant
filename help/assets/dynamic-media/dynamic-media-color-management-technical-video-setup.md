---
title: 理解色彩管AEM理Dynamic Media
description: 在此視頻中，我們將探討Dynamic Media色彩管理，以及如何使用它為AEM Assets提供色彩校正預覽功能。
sub-product: dynamic-media
feature: Image Profiles, Video Profiles
version: 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
exl-id: a733532b-db64-43f6-bc43-f7d422d5071a
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '319'
ht-degree: 17%

---

# 理解色彩管AEM理Dynamic Media{#understanding-color-management-with-aem-dynamic-media}

在此視頻中，我們將探討Dynamic Media色彩管理，以及如何使用它為AEM Assets提供色彩校正預覽功能。

>[!VIDEO](https://video.tv.adobe.com/v/16792/?quality=9&learn=on)

>[!NOTE]
>
>[啟用Dynamic Media](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html) 來AEM使用此功能。

此功能作為功AEM能包可用於6.1和6.2版。

## 顏色管理配置節點的XML模板 {#xml-template-for-the-color-management-configuration-node}

以下是「顏色管理」配置節點的XML模板。 此XML模板可複製到開發項AEM目中，並使用適合項目的配置進行配置。

```xml
<?xml version="1.0" encoding="UTF-8"?>

<!--
    XML Node definition for: /etc/dam/imageserver/configuration/jcr:content/settings

 Adobe Docs

 * Image Server Configuration: https://docs.adobe.com/docs/en/aem/6-2/administer/content/dynamic-media/config-dynamic.html#Configuring%20Dynamic%20Media%20Image%20Settings

* Default Color Profile Configuration: https://docs.adobe.com/docs/en/aem/6-1/administer/content/dynamic-media/config-dynamic.html#Configuring%20the%20default%20color%20profiles

    iccprofileXXX values:
        Node name of color profile found at: /etc/dam/imageserver/profiles

    iccblackpointcompensation values:
        true | false

    iccdither values:
        true | false

    iccrenderintent values:
        0 for perceptual
        1 for relative colorimetric
        2 for saturation
        3 for absolute colorimetric

-->

<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
    xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
    jcr:primaryType="nt:unstructured"

        bkgcolor="FFFFFF"
        defaultpix="300,300"
        defaultthumbpix="100,100"
        expiration="{Long}36000000"
        jpegquality="80"
        maxpix="2000,2000"
        resmode="SHARP2"
        resolution="72"
        thumbnailtime="[1%,11%,21%,31%,41%,51%,61%,71%,81%,91%]"
        iccprofilergb=""
        iccprofilecmyk=""
        iccprofilegray=""
        iccprofilesrcrgb=""
        iccprofilesrccmyk=""
        iccprofilesrcgray=""
        iccblackpointcompensation="{Boolean}true"
        iccdither="{Boolean}false"
        iccrenderintent="{Long}0"
/>
```

### 下面列出了預設Adobe顏色配置檔案清單 {#list-of-default-adobe-color-profiles-are-listed-below}

| 名稱 | 色彩空間 | 說明 |
| ------------------- | ---------- | ------------------------------------- |
| AdobeRGB | RGB | Adobe RGB(1998) |
| AppleRGB | RGB | AppleRGB |
| CIERGB | RGB | CIERGB |
| CobedFogra27 | CMYK | 塗層FOGRA27(ISO 12647-2:2004) |
| CobedFogra39 | CMYK | 塗層FOGRA39(ISO 12647-2:2004) |
| 塗層格拉科 | CMYK | 塗層GRACoL 2006(ISO 12647-2:2004) |
| 顏色匹配RGB | RGB | 顏色匹配RGB |
| 歐洲ISOC | CMYK | 歐洲ISO塗層FOGRA27 |
| 塗層歐洲 | CMYK | V2型歐洲級 |
| 未塗層歐洲比例 | CMYK | V2無塗層歐洲級 |
| 日本彩色塗層 | CMYK | 2001年日本彩色 |
| 日本彩色報 | CMYK | 《2002年日本彩色報》 |
| 日本顏色未塗層 | CMYK | 2001年日本彩色無塗層 |
| 日本彩色網路塗層 | CMYK | 日本彩色2003網路塗層 |
| 日本網路塗層 | CMYK | 日本Web Cobed（廣告） |
| 新聞紙SNAP2007 | CMYK | 美國新聞紙(SNAP 2007) |
| NTSC | RGB | NTSC(1953) |
| PAL | RGB | PAL/SECAM |
| ProPhoto | RGB | ProPhotoRGB |
| PS4預設 | CMYK | Photoshop4預設CMYK |
| PS5預設 | CMYK | Photoshop5預設CMYK |
| 已塗布的紙張 | CMYK | 美國單張紙塗布v2 |
| 單張已取消塗布 | CMYK | 美國單張未塗層v2 |
| SMPTE | RGB | SMPTE-C |
| sRGB | RGBsRGB | IEC61966-2.1 |
| 未塗層Fogra29 | CMYK | 未塗層FOGRA29(ISO 12647-2:2004) |
| 塗層網 | CMYK | 美國網路塗層(SWOP)v2 |
| WebCobedFogra28 | CMYK | Web Cobided FOGRA28(ISO 12647-2:2004) |
| WebCobdGrade3 | CMYK | SWOP 2006三級紙 |
| WebCobidGrade5 | CMYK | SWOP 2006五級紙 |
| 未塗層網 | CMYK | 美國網路未塗層v2 |
| 寬色域RGB | RGB | 寬色域RGB |

## 其他資源{#additional-resources}

* [配置Dynamic Media顏色管理](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dynamic.html#ConfiguringDynamicMediaColorManagement)
