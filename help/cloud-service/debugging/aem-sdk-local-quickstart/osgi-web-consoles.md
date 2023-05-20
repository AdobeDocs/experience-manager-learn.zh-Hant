---
title: 使用AEMOSGi Web控制台調試SDK
description: SDKAEM的本地快速啟動具有OSGi Web控制台，該控制台在本地運行時提供各種資訊和介紹，這些資訊和介紹對於瞭解應用程式如何被識別以及其中的功AEM能非常有用。
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5265, 5366, 5267
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 0929bc1a-376c-4e16-a540-a276fd5af164
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '393'
ht-degree: 0%

---

# 使用AEMOSGi Web控制台調試SDK

SDKAEM的本地快速啟動具有OSGi Web控制台，該控制台在本地運行時提供各種資訊和介紹，這些資訊和介紹對於瞭解應用程式如何被識別以及其中的功AEM能非常有用。

提AEM供了許多OSGi控制台，每個控制台都提供了對不同方面的關鍵AEM見解，但是，以下內容通常在調試應用程式時最有用。

## 捆綁

>[!VIDEO](https://video.tv.adobe.com/v/34335?quality=12&learn=on)

Bundles控制台是OSGi捆綁包的目錄及其詳細資訊，AEM並具有啟動和停止這些捆綁包的臨時功能。

Bundles控制台位於：

+ 「工具」>「操作」>「Web控制台」>「OSGi」>「捆綁包」
+ 或直接在： [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)

按一下每個包，提供有助於調試應用程式的詳細資訊。

+ 驗證OSGi捆綁包存在
+ 驗證OSGi捆綁是否處於活動狀態
+ 確定OSGi捆綁是否具有未滿足的導入阻止其啟動

## 元件

>[!VIDEO](https://video.tv.adobe.com/v/34336?quality=12&learn=on)

「元件」控制台是部署到的所有OSGi元件的目錄AEM，並提供了有關它們的所有資訊，從它們定義的OSGi元件生命週期到它們可能引用的OSGi服務

元件控制台位於：

+ 「工具」>「操作」>「Web控制台」>「OSGi」>「元件」
+ 或直接在： [http://localhost:4502/system/console/components](http://localhost:4502/system/console/components)

幫助執行調試活動的關鍵方面：

+ 驗證OSGi捆綁包存在
+ 驗證OSGi捆綁是否處於活動狀態
+ 確定OSGi捆綁是否具有未滿足的導入阻止其啟動
+ 獲取元件的PID，以便以Git為元件建立OSGi配置
+ 標識綁定到活動OSGi配置的OSGi屬性值

## Sling 模型

>[!VIDEO](https://video.tv.adobe.com/v/34337?quality=12&learn=on)

Sling Models控制台位於：

+ 「工具」>「操作」>「Web控制台」>「狀態」>「Sling模型」
+ 或直接在： [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)

幫助執行調試活動的關鍵方面：

+ 驗證Sling模型已註冊到正確的資源類型
+ 驗證Sling模型可從正確的對象（Resource或SlingHttpRequestServlet）中調整
+ 驗證Sling模型導出程式已正確註冊
