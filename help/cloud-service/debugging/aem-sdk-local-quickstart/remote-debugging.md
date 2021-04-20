---
title: 遠端除AEM錯SDK
description: SDKAEM的本機快速入門可讓您從IDE進行遠端Java除錯，讓您逐步執行即時程式碼，以了AEM解確切的執行流程。
feature: Developer Tools
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5251
topic: Development
role: Developer
level: Beginner, Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 1%

---


# 遠端除AEM錯SDK

>[!VIDEO](https://video.tv.adobe.com/v/34338/?quality=12&learn=on)

SDKAEM的本機快速入門可讓您從IDE進行遠端Java除錯，讓您逐步執行即時程式碼，以了AEM解確切的執行流程。

若要將遠端除錯程式連AEM接至AEM,SDK的本機快速入門必須以特定參數(`-agentlib:...`)啟動，讓IDE可連接至它。

```
$ java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005 -jar aem-author-p4502.jar   
```

+ `address` 指定遠AEM程調試連接的埠偵聽，並可更改為本地開發電腦上的任何可用埠。
+ 最後一個參數(例如 `aem-author-p4502.jar`)是SKD快AEM速入門。這可以是AEM Author服務(`aem-author-p4502.jar`)或AEM Publish服務(`aem-publish-p4503.jar`)。

## IDE設定說明

大多數Java IDE都支援對Java程式進行遠程調試，但每個IDE的確切設定步驟各不相同。 請查看IDE的遠程調試設定說明，以瞭解具體步驟。 通常，IDE配置需要：

+ 主機AEMSDK的本機快速入門正在監聽，即`localhost`。
+ 埠AEMSDK的本機快速啟動正在監聽遠端除錯連線，此為啟動AEMSDK本機快速啟動時由`address`參數指定的埠。
+ 有時，必須指定為遠程調試提供原始碼的Maven項目；這是您的OSGi捆綁銷售專案。

### 設定指示

+ [VS代碼Java遠程調試程式設定](https://code.visualstudio.com/docs/java/java-debugging)
+ [IntelliJ IDEA Remote除錯程式設定](https://www.jetbrains.com/help/idea/run-debug-configuration-remote-debug.html)
+ [Eclipse Remote除錯程式設定](https://javapapers.com/core-java/java-remote-debug-with-eclipse/)
