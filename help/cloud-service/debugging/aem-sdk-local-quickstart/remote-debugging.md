---
title: 遠程調試AEMSDK
description: SDKAEM的本地快速啟動允許從IDE進行遠程Java調試，使您可以在中逐步執行即時代碼，以了AEM解確切的執行流。
kt: 5251
topic: Development
feature: Developer Tools
role: Developer
level: Beginner, Intermediate
thumbnail: 34338.jpeg
exl-id: beac60c6-11ae-4d0c-a055-cd3d05aeb126
source-git-commit: 45e7c58efd1d89537752fe7f890c0e80f7be7d67
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 0%

---

# 遠程調試AEMSDK

>[!VIDEO](https://video.tv.adobe.com/v/34338?quality=12&learn=on)

SDKAEM的本地快速啟動允許從IDE進行遠程Java調試，使您可以在中逐步執行即時代碼，以了AEM解確切的執行流。

要將遠程調試器連AEM接到，AEM必須使用特定參數啟動SDK的本地快速啟動(`-agentlib:...`)允許IDE連接到它。

```
$ java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=*:5005 -jar aem-author-p4502.jar   
```

+ AEM SDK僅支援Java 11
+ `address` 指定監聽AEM遠程調試連接的埠，並可以更改為本地開發電腦上的任何可用埠。
+ 最後一個參數(例如 `aem-author-p4502.jar`)是AEMSKD快速啟動Jar。 這可以是AEM作者服務(`aem-author-p4502.jar`)或AEM發佈服務(`aem-publish-p4503.jar`)。


## IDE設定說明

大多數Java IDE都支援遠程調試Java程式，但每個IDE的確切設定步驟各不相同。 請查看IDE的遠程調試設定說明以瞭解具體步驟。 通常，IDE配置需要：

+ 主機AEMSDK的本地快速啟動正在偵聽， `localhost`。
+ 埠AEM SDK的本地快速啟動正在偵聽遠程調試連接，該連接由 `address` 參數AEM。
+ 有時，必須指定為遠程調試提供原始碼的Maven項目；這是您的OSGi捆綁銷售項目。

### 設定說明

+ [VS代碼Java遠程調試器設定](https://code.visualstudio.com/docs/java/java-debugging)
+ [設定IntelliJ IDEA遠程調試器](https://www.jetbrains.com/help/idea/tutorial-remote-debug.html)
+ [設定Eclipse遠程調試器](https://javapapers.com/core-java/java-remote-debug-with-eclipse/)
