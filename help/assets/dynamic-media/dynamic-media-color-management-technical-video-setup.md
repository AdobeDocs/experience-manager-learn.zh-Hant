---
title: 瞭解色彩管理與AEMDynamic Media
description: 在此影片中，我們探索「Dynamic Media色彩管理」，以及如何使用它來提供AEM Assets的色彩校正預覽功能。
sub-product: 動態媒體
feature: 影像描述檔、視訊描述檔
version: 6.3, 6.4, 6.5
topic: 內容管理
role: 開發人員
level: 中級
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '330'
ht-degree: 14%

---


# 瞭解使用AEMDynamic Media的色彩管理{#understanding-color-management-with-aem-dynamic-media}

在此影片中，我們探索「Dynamic Media色彩管理」，以及如何使用它來提供AEM Assets的色彩校正預覽功能。

>[!VIDEO](https://video.tv.adobe.com/v/16792/?quality=9&learn=on)

>[!NOTE]
>
>[啟用Dynamic ](https://docs.adobe.com/docs/en/aem/6-0/administer/integration/dynamic-media/enabling-dynamic-media.html) MediainAEM以使用此功能。

此功能適用於6.AEM1和6.2版的功能套件。

## 色彩管理配置節點{#xml-template-for-the-color-management-configuration-node}的XML模板

以下是「色彩管理」設定節點的XML範本。 此XML範本可複製至開發專AEM案，並以專案適當的組態來設定。

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

### 預設Adobe色彩描述檔清單列於{#list-of-default-adobe-color-profiles-are-listed-below}下方

| 名稱 | 色彩空間 | 說明 |
| ------------------- | ---------- | ------------------------------------- |
| AdobeRGB | RGB | Adobe RGB(1998) |
| AppleRGB | RGB | Apple RGB |
| CIERGB | RGB | CIE RGB |
| CobatedFogra27 | CMYK | 塗覆FOGRA27(ISO 12647-2:2004) |
| CobatedFogra39 | CMYK | 塗覆FOGRA39(ISO 12647-2:2004) |
| CobatedGraCol | CMYK | 塗層GRACoL 2006(ISO 12647-2:2004) |
| ColorMatchRGB | RGB | ColorMatch RGB |
| EuropeISOCoated | CMYK | 歐洲ISO塗層FOGRA27 |
| EuroscaleCoobed | CMYK | Euroscale Coopted v2 |
| EuroscaleUncoated | CMYK | Euroscale Uncoupted v2 |
| JapanColorCooverd | CMYK | Japan Color 2001 Coupted |
| JapanColorSeppare | CMYK | 《日本彩色2002報》 |
| JapanColorUncoated | CMYK | 2001年日本彩色無塗層 |
| JapanColorWebCooved | CMYK | 日本Color 2003 Web Copted |
| JapanWebCoapted | CMYK | Japan Web Coopted(Ad) |
| NewsprintSNAP2007 | CMYK | 美國新聞用紙(SNAP 2007) |
| NTSC | RGB | NTSC(1953) |
| PAL | RGB | PAL/SECAM |
| ProPhoto | RGB | ProPhoto RGB |
| PS4Default | CMYK | Photoshop4預設CMYK |
| PS5Default | CMYK | Photoshop5預設CMYK |
| 張紙塗布 | CMYK | 美國張紙塗布v2 |
| 張紙未塗覆 | CMYK | 美國張紙未塗覆v2 |
| SMPTE | RGB | SMPTE-C |
| sRGB | RGB sRGB | IEC61966-2.1 |
| UncovedFogra29 | CMYK | 未塗覆的FOGRA29(ISO 12647-2:2004) |
| WebCoated | CMYK | 美國網衣(SWOP)v2 |
| WebCoatedFogra28 | CMYK | 網頁塗層FOGRA28(ISO 12647-2:2004) |
| WebCoatedGrade3 | CMYK | 塗網SWOP 2006三級紙 |
| WebCobatedGrade5 | CMYK | 塗網SWOP 2006五級紙 |
| WebUncoated | CMYK | U.S. Web Uncoved v2 |
| 寬色域RGB | RGB | 寬色域RGB |

## 其他資源{#additional-resources}

* [配置Dynamic Media色彩管理](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dynamic.html#ConfiguringDynamicMediaColorManagement)
