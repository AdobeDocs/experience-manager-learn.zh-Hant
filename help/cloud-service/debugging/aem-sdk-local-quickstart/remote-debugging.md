---
title: 從遠端除錯AEM SDK
description: AEM SDK的本機Quickstart可讓您從IDE進行遠端Java除錯，讓您逐步執行AEM中的即時程式碼，以了解確切的執行流程。
kt: 5251
topic: Development
feature: Developer Tools
role: Developer
level: Beginner, Intermediate
thumbnail: 34338.jpeg
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '275'
ht-degree: 0%

---


# 從遠端除錯AEM SDK

>[!VIDEO](https://video.tv.adobe.com/v/34338/?quality=12&learn=on)

AEM SDK的本機Quickstart可讓您從IDE進行遠端Java除錯，讓您逐步執行AEM中的即時程式碼，以了解確切的執行流程。

要將遠程調試器連接到AEM,AEM SDK的本地快速入門必須使用特定參數(`-agentlib:...`)啟動，以便IDE連接到它。

```
$ java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005 -jar aem-author-p4502.jar   
```

+ `address` 指定AEM監聽遠程調試連接，並可更改為本地開發電腦上的任何可用埠。
+ 最後一個參數(例如 `aem-author-p4502.jar`)是AEM SKD Quickstart Jar。這可以是AEM製作服務(`aem-author-p4502.jar`)或AEM發佈服務(`aem-publish-p4503.jar`)。

## IDE設定說明

大多數Java IDE都支援對Java程式進行遠程調試，但每個IDE的確切設定步驟各不相同。 請查看IDE的遠程調試設定指示，了解確切步驟。 IDE配置通常需要：

+ 主機AEM SDK的本機快速入門正在監聽，即`localhost`。
+ 連接埠AEM SDK的本地快速入門正在監聽遠程調試連接，該連接是啟動AEM SDK的本地快速入門時由`address`參數指定的埠。
+ 有時，必須指定提供原始碼以進行遠端除錯的Maven專案；這是您的OSGi套件組合maven專案。

### 設定指示

+ [VS程式碼Java遠端除錯程式設定](https://code.visualstudio.com/docs/java/java-debugging)
+ [設定IntelliJ IDEA遠程調試器](https://www.jetbrains.com/help/idea/tutorial-remote-debug.html)
+ [Eclipse遠端除錯程式設定](https://javapapers.com/core-java/java-remote-debug-with-eclipse/)
