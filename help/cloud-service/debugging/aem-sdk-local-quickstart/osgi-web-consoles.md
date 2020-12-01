---
title: 使用OSGi網頁主控台除錯AEM SDK
description: AEM SDK的本機快速入門有一個OSGi網頁主控台，可提供本機AEM執行階段中的各種資訊和介紹，這些資訊和介紹對於瞭解您的應用程式如何被AEM識別及在AEM中的功能十分有用。
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5265, 5366, 5267
translation-type: tm+mt
source-git-commit: a3d3612713decefb5c7e1cf5b2d4d21afff0a2f5
workflow-type: tm+mt
source-wordcount: '393'
ht-degree: 0%

---


# 使用OSGi網頁主控台除錯AEM SDK

AEM SDK的本機快速入門有一個OSGi網頁主控台，可提供本機AEM執行階段中的各種資訊和介紹，這些資訊和介紹對於瞭解您的應用程式如何被AEM識別及在AEM中的功能十分有用。

AEM提供許多OSGi主控台，每個主控台都提供AEM不同層面的重要深入資訊，但是下列在除錯應用程式時通常最有用。

## 組合

>[!VIDEO](https://video.tv.adobe.com/v/34335/?quality=12&learn=on)

Bundles主控台是OSGi Bundles的目錄，以及部署至AEM的詳細資訊，以及啟動和停止OSGi Bundles的臨機功能。

Bundles控制台位於：

+ 「工具>作業> Web Console > OSGi >組合」
+ 或直接在：[http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)

按一下每個套件，提供有助於除錯應用程式的詳細資訊。

+ 驗證OSGi捆綁包存在
+ 驗證OSGi捆綁包是否處於活動狀態
+ 確定OSGi捆綁包是否具有未滿足的導入，阻止其啟動

## 元件

>[!VIDEO](https://video.tv.adobe.com/v/34336/?quality=12&learn=on)

Components主控台是部署至AEM的所有OSGi元件的目錄，並提供有關這些元件的所有資訊，從其定義的OSGi元件生命週期，到其可參考的OSGi服務

元件控制台位於：

+ 「工具>作業> Web Console > OSGi >元件」
+ 或直接在：[http://localhost:4502/system/console/components](http://localhost:4502/system/console/components)

協助除錯活動的主要方面：

+ 驗證OSGi捆綁包存在
+ 驗證OSGi捆綁包是否處於活動狀態
+ 確定OSGi捆綁包是否具有未滿足的導入，阻止其啟動
+ 取得元件的PID，以便以Git建立OSGi組態
+ 識別綁定到活動OSGi配置的OSGi屬性值

## Sling Models

>[!VIDEO](https://video.tv.adobe.com/v/34337/?quality=12&learn=on)

Sling Models主控台位於：

+ 「工具>作業> Web Console >狀態> Sling Models」
+ 或直接在：[http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)

協助除錯活動的主要方面：

+ 驗證Sling Models會註冊至正確的資源類型
+ 驗證Sling Models可從正確的物件（Resource或SlingHttpRequestServlet）調整
+ 驗證Sling Model Exportorer正確註冊
