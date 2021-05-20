---
title: 為打印通道建立第一個互動式通信
seo-title: 為打印通道建立第一個互動式通信
description: 互動式通訊是AEM Forms 6.4的新功能。本檔案將引導您完成為列印管道建立互動式通訊所需的步驟。
seo-description: 互動式通訊是AEM Forms 6.4的新功能。本檔案將引導您完成為列印管道建立互動式通訊所需的步驟。
feature: 互動式通訊
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
topic: 開發
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 3%

---


# 為打印通道建立第一個互動式通信

互動式通訊是AEM Forms 6.4的新功能。本檔案將引導您完成為列印管道建立互動式通訊所需的步驟。

請造訪[AEM Forms範例](https://forms.enablementadobe.com/content/samples/samples.html?query=0)頁面，以取得此功能的即時示範連結。

## 必備條件 {#prerequistes}

[使用套件管理器，將本教學課程相關的資產下載並匯入AEM。](assets/gettingstartedassets.zip)此zip檔案包含作為資產套件一部分的影像、檔案片段、已觀看的資料夾設定和配置檔案(xdp)

[下載並解壓縮此檔案。](assets/warfileandswaggerfile.zip) 此檔案包含需要部署到Tomcat上的SampleRest.war檔案和需要用於配置資料源的Swagger檔案。

完成本教學課程後，您將學到下列內容：

* 建立資料來源
* 建立表單資料模型
* 建立檔案片段
* 配置表和圖表
* 使用「監看的資料夾」以批處理模式生成文檔

