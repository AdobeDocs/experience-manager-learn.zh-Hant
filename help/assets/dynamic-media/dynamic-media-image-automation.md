---
title: 適用於透明與內容自動化批次處理的Dynamic Media
description: 瞭解如何在AEM中使用Dynamic Media來建立虛擬轉譯、管理透明度，以及自動化影像處理以重複使用可擴充的內容。
feature: Image Profiles, Viewer Presets
topic: Content Management
role: User
level: Beginner, Intermediate, Experienced
doc-type: Feature Video
duration: 560
last-substantial-update: 2025-05-28T00:00:00Z
jira: KT-18197
source-git-commit: a509b7c41e6adb05e6c3b791ac2e99f635973322
workflow-type: tm+mt
source-wordcount: '262'
ht-degree: 2%

---


# 適用於透明與內容自動化批次處理的Dynamic Media

瞭解如何在AEM中使用Dynamic Media來建立虛擬轉譯、管理透明度，以及自動化影像處理以重複使用可擴充的內容。

>[!VIDEO](https://video.tv.adobe.com/v/3463056/?learn=on&enablevpops&captions=chi_hant)


## Dynamic Media資產範例

以下是在影片中使用的Dynamic Media資產及其URL範例。

>[!BEGINTABS]

>[!TAB 影像透明度範例]

以下是影片中使用的Dynamic Media影像伺服器範例URL。

| 預覽 | 說明 | 動態媒體URL |
|-----------|------------------|---------|
| ![預設](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?bgc=255,255,255){width="250"} | 預設 | [連結](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?bgc=255,255,255) |
| ![與順暢的背景影像圖層複合](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&amp;layer=1&amp;src=backdrop5-Camera&amp;size=8500,8500&amp;layer=2&amp;src=AdobeStock_322150086%20trans){width="250"} | 與無縫的背景影像層結合 | [連結](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&amp;layer=1&amp;src=backdrop5-Camera&amp;size=8500,8500&amp;layer=2&amp;src=AdobeStock_322150086%20trans) |
| ![紅色背景](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&amp;layer=1&amp;color=200,50,50&amp;size=8500,8500&amp;layer=2&amp;src=AdobeStock_322150086%20trans){width="250"} | 紅色背景 | [連結](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&amp;layer=1&amp;color=200,50,50&amp;size=8500,8500&amp;layer=2&amp;src=AdobeStock_322150086%20trans) |
| ![已剪裁為橢圓形路徑](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&amp;bgc=255,255,255){width="250"} | 剪裁成橢圓形路徑 | [連結](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&amp;bgc=255,255,255) |


>[!TAB 影像路徑範例]

以下是影片中使用的Dynamic Media影像伺服器範例URL。

| 預覽 | 說明 | 動態媒體URL |
|-----------|------------------|---------|
| ![已標準化為80畫素寬（無透明度）](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?wid=800){width="250"} | 已標準化為80畫素寬（無透明度）{width="250"} | [連結](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?wid=800) |
| ![裁切至路徑](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?cropPathE=Path%201&amp;wid=800){width="250"} | 裁切至路徑 | [連結](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?cropPathE=Path%201&amp;wid=800) |
| ![剪裁至路徑](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&amp;wid=800){width="250"} | 剪裁至路徑 | [連結](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&amp;wid=800) |
| ![剪裁至路徑及裁切至路徑](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&amp;cropPathE=Path%201&amp;wid=800){width="250"} | 剪裁至路徑與裁切至路徑 | [連結](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&amp;cropPathE=Path%201&amp;wid=800) |
| ![剪裁至其他路徑](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&amp;wid=800){width="250"} | 剪裁至其他路徑 | [連結](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&amp;wid=800) |
| ![剪裁至其他路徑並製作紅色背景](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086fullpaths?cropPathE=round&amp;clipPathE=round&amp;bgc=200,50,50&amp;wid=800){width="250"} | 剪裁至其他路徑並製作紅色背景 | [連結](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086fullpaths?cropPathE=round&amp;clipPathE=round&amp;bgc=200,50,50&amp;wid=800) |

>[!ENDTABS]


## Dynamic Media影像伺服器API

* [Dynamic Media影像提供與轉譯API](https://experienceleague.adobe.com/zh-hant/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/http-protocol-reference/c-http-protocol-reference)
* [動態媒體快照集預覽](https://snapshot.scene7.com/)
