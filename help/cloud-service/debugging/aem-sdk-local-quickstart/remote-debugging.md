---
title: 遠端除錯AEM SDK
description: AEM SDK的本機快速入門可讓您從IDE進行遠端Java除錯，讓您逐步執行AEM中的即時程式碼執行，以瞭解確切的執行流程。
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5251
translation-type: tm+mt
source-git-commit: a3d3612713decefb5c7e1cf5b2d4d21afff0a2f5
workflow-type: tm+mt
source-wordcount: '275'
ht-degree: 0%

---


# 遠端除錯AEM SDK

>[!VIDEO](https://video.tv.adobe.com/v/34338/?quality=12&learn=on)

AEM SDK的本機快速入門可讓您從IDE進行遠端Java除錯，讓您逐步執行AEM中的即時程式碼執行，以瞭解確切的執行流程。

若要將遠端除錯程式連接至AEM,AEM SDK的本機快速入門必須以特定參數(`-agentlib:...`)啟動，讓IDE能連線至它。

```
$ java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005 -jar aem-author-p4502.jar   
```

+ `address` 指定AEM監聽遠端除錯連線的埠，並可變更為本機開發機器上的任何可用埠。
+ 最後一個參數(例如 `aem-author-p4502.jar`)是AEM SKD Quickstart Jar。這可以是AEM Author服務(`aem-author-p4502.jar`)或AEM Publish服務(`aem-publish-p4503.jar`)。

## IDE設定說明

大多數Java IDE都支援對Java程式進行遠程調試，但每個IDE的確切設定步驟各不相同。 請查看IDE的遠程調試設定說明，以瞭解具體步驟。 通常，IDE配置需要：

+ 主機AEM SDK的本機快速入門正在監聽，即`localhost`。
+ 埠AEM SDK的本機快速入門正在監聽遠端除錯連線，此為啟動AEM SDK的本機快速入門時由`address`參數指定的埠。
+ 有時，必須指定為遠程調試提供原始碼的Maven項目；這是您的OSGi捆綁銷售專案。

### 設定指示

+ [VS代碼Java遠程調試程式設定](https://code.visualstudio.com/docs/java/java-debugging)
+ [IntelliJ IDEA Remote除錯程式設定](https://www.jetbrains.com/help/idea/run-debug-configuration-remote-debug.html)
+ [Eclipse Remote除錯程式設定](https://javapapers.com/core-java/java-remote-debug-with-eclipse/)
