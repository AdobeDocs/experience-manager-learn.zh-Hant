---
title: 從遠端偵錯AEM SDK
description: AEM SDK的本機Quickstart允許從IDE進行遠端Java偵錯，讓您在AEM中逐步執行即時程式碼，以瞭解確切的執行流程。
jira: KT-5251
topic: Development
feature: Developer Tools
role: Developer
level: Beginner, Intermediate
thumbnail: 34338.jpeg
exl-id: beac60c6-11ae-4d0c-a055-cd3d05aeb126
duration: 428
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 0%

---

# 從遠端偵錯AEM SDK

>[!VIDEO](https://video.tv.adobe.com/v/34338?quality=12&learn=on)

AEM SDK的本機Quickstart允許從IDE進行遠端Java偵錯，讓您在AEM中逐步執行即時程式碼，以瞭解確切的執行流程。

若要將遠端偵錯工具連線至AEM，必須使用特定引數啟動AEM SDK的本機Quickstart (`-agentlib:...`)，允許IDE連線到它。

```
$ java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=*:5005 -jar aem-author-p4502.jar   
```

+ AEM SDK僅支援Java 11
+ `address` 指定AEM接聽遠端偵錯連線的連線埠，並可變更為本機開發電腦上任何可用的連線埠。
+ 最後一個引數(例如 `aem-author-p4502.jar`)是AEM SKD Quickstart Jar。 這可以是AEM Author服務(`aem-author-p4502.jar`)或AEM Publish服務(`aem-publish-p4503.jar`)。


## IDE設定指示

大多數Java IDE都提供對Java程式進行遠端偵錯的支援，但每個IDE的具體設定步驟有所不同。 請檢閱IDE的遠端偵錯設定指示，以瞭解確切步驟。 通常IDE配置需要：

+ 主機AEM SDK的本機Quickstart正在接聽，也就是 `localhost`.
+ AEM SDK的本機Quickstart正在接聽連線埠以進行遠端偵錯連線，該連線埠是由 `address` 啟動AEM SDK的本機Quickstart時的引數。
+ 有時候，必須指定提供原始程式碼給遠端偵錯的Maven專案；這是您的OSGi套件組合maven專案專案。

### 設定指示

+ [VS程式碼Java遠端偵錯工具設定](https://code.visualstudio.com/docs/java/java-debugging)
+ [IntelliJ IDEA遠端偵錯工具設定](https://www.jetbrains.com/help/idea/tutorial-remote-debug.html)
+ [Eclipse遠端偵錯工具設定](https://javapapers.com/core-java/java-remote-debug-with-eclipse/)
