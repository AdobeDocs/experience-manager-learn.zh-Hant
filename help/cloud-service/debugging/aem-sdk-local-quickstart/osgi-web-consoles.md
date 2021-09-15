---
title: 使用OSGi Web主控台除錯AEM SDK
description: AEM SDK的本機Quickstart有一個OSGi Web主控台，提供本機AEM執行階段的各種資訊和簡介，有助於了解應用程式如何辨識，以及在AEM中的功能。
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
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '393'
ht-degree: 0%

---

# 使用OSGi Web主控台除錯AEM SDK

AEM SDK的本機Quickstart有一個OSGi Web主控台，提供本機AEM執行階段的各種資訊和簡介，有助於了解應用程式如何辨識，以及在AEM中的功能。

AEM提供許多OSGi主控台，每個主控台都提供AEM不同層面的重要深入分析，但下列主控台在除錯應用程式時通常最有用。

## 套件組合

>[!VIDEO](https://video.tv.adobe.com/v/34335/?quality=12&learn=on)

套件組合主控台是OSGi套件組合的目錄，以及部署至AEM的詳細資料，以及啟動和停止這些套件的臨機功能。

套件組合控制台位於：

+ 「工具」>「操作」>「Web控制台」>「OSGi」>「套件組合」
+ 或直接在：[http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)

按一下每個套件，可提供有助於除錯應用程式的詳細資訊。

+ 驗證OSGi捆綁包
+ 驗證OSGi捆綁是否處於活動狀態
+ 確定OSGi套件組合是否具有未滿足的導入，阻止其啟動

## 元件

>[!VIDEO](https://video.tv.adobe.com/v/34336/?quality=12&learn=on)

元件控制台是部署到AEM的所有OSGi元件的目錄，它提供了有關這些元件的所有資訊，從其定義的OSGi元件生命週期，到它們可參考的OSGi服務

元件控制台位於：

+ 「工具」>「操作」>「Web控制台」>「OSGi」>「元件」
+ 或直接在：[http://localhost:4502/system/console/components](http://localhost:4502/system/console/components)

有助於偵錯活動的主要方面：

+ 驗證OSGi捆綁包
+ 驗證OSGi捆綁是否處於活動狀態
+ 確定OSGi套件組合是否具有未滿足的導入，阻止其啟動
+ 取得元件的PID，以便在Git中為元件建立OSGi設定
+ 識別綁定到活動OSGi配置的OSGi屬性值

## Sling 模型

>[!VIDEO](https://video.tv.adobe.com/v/34337/?quality=12&learn=on)

Sling模型控制台位於：

+ 「工具」>「操作」>「Web控制台」>「狀態」>「Sling模型」
+ 或直接在：[http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)

有助於偵錯活動的主要方面：

+ 驗證Sling模型會註冊為正確的資源類型
+ 驗證Sling模型可從正確的物件（Resource或SlingHttpRequestServlet）調整
+ 正確註冊驗證Sling模型匯出工具
