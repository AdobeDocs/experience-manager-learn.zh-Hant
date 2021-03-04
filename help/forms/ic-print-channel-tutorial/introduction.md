---
title: 為列印頻道建立您的第一個互動式通訊
seo-title: 為列印頻道建立您的第一個互動式通訊
description: 互動式通訊是AEM Forms6.4的新手。本檔案將逐步引導您建立適用於列印頻道的互動式通訊。
seo-description: 互動式通訊是AEM Forms6.4的新手。本檔案將逐步引導您建立適用於列印頻道的互動式通訊。
feature: 交互通信
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '225'
ht-degree: 2%

---


# 為列印頻道建立您的第一個互動式通訊

互動式通訊是AEM Forms6.4的新手。本檔案將逐步引導您建立適用於列印頻道的互動式通訊。

請造訪[AEM Forms範例](https://forms.enablementadobe.com/content/samples/samples.html?query=0)頁面，以取得此功能的即時示範連結。

## 必備條件 {#prerequistes}

[使用套件管理器，下載並匯入與本教學課程AEM相關的資產。](assets/gettingstartedassets.zip)此zip檔案包含影像、檔案片段、監看的檔案夾設定和版面檔案(xdp)，做為資產套件的一部分

[下載並解壓縮此檔案。](assets/warfileandswaggerfile.zip) 此檔案包含需要部署至Tomcat的SampleRest.war檔案，以及需要用於設定資料來源的Swagger檔案。

完成本教學課程後，您將學到下列內容：

* 建立資料來源
* 建立表單資料模型
* 建立檔案片段
* 配置表和圖表
* 使用「監看的資料夾」，以批次模式產生檔案

