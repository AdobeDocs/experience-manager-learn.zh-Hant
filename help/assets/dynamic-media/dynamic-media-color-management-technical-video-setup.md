---
title: 了解AEM Dynamic Media的色彩管理
description: 本影片將介紹Dynamic Media色彩管理，以及如何使用它來提供AEM Assets適用的色彩校正預覽功能。
feature: Image Profiles, Video Profiles
version: 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
exl-id: a733532b-db64-43f6-bc43-f7d422d5071a
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '319'
ht-degree: 17%

---

# 了解AEM Dynamic Media的色彩管理{#understanding-color-management-with-aem-dynamic-media}

本影片將介紹Dynamic Media色彩管理，以及如何使用它來提供AEM Assets適用的色彩校正預覽功能。

>[!VIDEO](https://video.tv.adobe.com/v/16792?quality=12&learn=on)

>[!NOTE]
>
>[啟用Dynamic Media](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html) 在AEM中使用此功能。

AEM 6.1和6.2版本為Feature Pack提供此功能。

## 色彩管理配置節點的XML模板 {#xml-template-for-the-color-management-configuration-node}

以下是「顏色管理」配置節點的XML模板。 此XML範本可複製至AEM開發專案，並使用適合專案的設定進行設定。

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

### 以下列出預設Adobe顏色配置檔案清單 {#list-of-default-adobe-color-profiles-are-listed-below}

| 名稱 | 色彩空間 | 說明 |
| ------------------- | ---------- | ------------------------------------- |
| AdobeRGB | RGB | Adobe RGB市（1998年） |
| AppleRGB | RGB | AppleRGB |
| CIERGB | RGB | CIERGB |
| CoatedFogra27 | CMYK | 塗層FOGRA27(ISO 12647-2:2004) |
| CoatedFogra39 | CMYK | 塗層FOGRA39(ISO 12647-2:2004) |
| CobatedGraCol | CMYK | 塗層GRACoL 2006(ISO 12647-2:2004) |
| ColorMatchRGB | RGB | ColorMatchRGB |
| 歐洲ISOC已處理 | CMYK | 歐洲ISO塗層FOGRA27 |
| EuroscaleCobated | CMYK | 塗有V2的歐洲標準 |
| EuroscaleUncobated | CMYK | 歐洲標準無塗層v2 |
| JapanColorCobated | CMYK | 2001年日本塗料 |
| JapanColorSpeable | CMYK | 《2002年日本彩色報》 |
| JapanColorUncobated | CMYK | 2001年日本顏色無塗層 |
| JapanColorWebCobated | CMYK | 日本彩色2003網版 |
| JapanWebCobated | CMYK | Japan Web Cobated（廣告） |
| NewsprintSNAP2007 | CMYK | 美國新聞紙(SNAP 2007) |
| NTSC | RGB | NTSC（1953年） |
| PAL | RGB | PAL/SECAM |
| ProPhoto | RGB | ProPhotoRGB |
| PS4Default | CMYK | Photoshop 4預設CMYK |
| PS5Default | CMYK | Photoshop 5預設CMYK |
| 張紙塗層 | CMYK | 美國鈑金件塗層v2 |
| 鈑金件未塗覆 | CMYK | 美國鈑金未塗層v2 |
| SMPTE | RGB | SMPTE-C |
| sRGB | RGBsRGB | IEC61966-2.1 |
| UncoatedFogra29 | CMYK | 無塗層FOGRA29(ISO 12647-2:2004) |
| WebCobated | CMYK | 美國塗層網板(SWOP)v2 |
| WebCoatedFogra28 | CMYK | Web Cobated FOGRA28(ISO 12647-2:2004) |
| WebCoatedGrade3 | CMYK | SWOP 2006三級紙 |
| WebCoatedGrade5 | CMYK | SWOP 2006五級紙 |
| WebUncobated | CMYK | 美國網路未塗層v2 |
| 寬色域RGB | RGB | 寬色域RGB |

## 其他資源{#additional-resources}

* [設定Dynamic Media色彩管理](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dynamic.html#ConfiguringDynamicMediaColorManagement)
