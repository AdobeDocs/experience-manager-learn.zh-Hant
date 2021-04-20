---
title: 使用AEMOSGi網頁主控台除錯SDK
description: AEMSDK的本端快速入門有OSGi網頁主控台，可提供本端執行階段中的多種資訊和介紹，以協助您瞭解應用程式的識別方式，以及其中的功AEM能。
feature: Developer Tools
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5265, 5366, 5267
topic: Development
role: Developer
level: Beginner, Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '399'
ht-degree: 1%

---


# 使用AEMOSGi網頁主控台除錯SDK

AEMSDK的本端快速入門有OSGi網頁主控台，可提供本端執行階段中的多種資訊和介紹，以協助您瞭解應用程式的識別方式，以及其中的功AEM能。

提AEM供許多OSGi控制台，每個控制台都提供不同方面的關鍵見解AEM，但是，在調試應用程式時，以下通常最有用。

## 組合

>[!VIDEO](https://video.tv.adobe.com/v/34335/?quality=12&learn=on)

Bundles控制台是OSGi捆綁包的目錄，其詳細資訊已部署AEM到，並具有啟動和停止這些捆綁包的臨機功能。

Bundles控制台位於：

+ 「工具>作業> Web Console > OSGi >組合」
+ 或直接在：[http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)

按一下每個套件，提供有助於除錯應用程式的詳細資訊。

+ 驗證OSGi捆綁包存在
+ 驗證OSGi捆綁包是否處於活動狀態
+ 確定OSGi捆綁包是否具有未滿足的導入，阻止其啟動

## 元件

>[!VIDEO](https://video.tv.adobe.com/v/34336/?quality=12&learn=on)

元件控制台是部署到的所有OSGi元件的目錄AEM，並提供了有關它們的所有資訊，從其定義的OSGi元件生命週期到它們可參考的OSGi服務

元件控制台位於：

+ 「工具>作業> Web Console > OSGi >元件」
+ 或直接在：[http://localhost:4502/system/console/components](http://localhost:4502/system/console/components)

協助除錯活動的主要方面：

+ 驗證OSGi捆綁包存在
+ 驗證OSGi捆綁包是否處於活動狀態
+ 確定OSGi捆綁包是否具有未滿足的導入，阻止其啟動
+ 取得元件的PID，以便以Git建立OSGi組態
+ 識別綁定到活動OSGi配置的OSGi屬性值

## Sling 模型

>[!VIDEO](https://video.tv.adobe.com/v/34337/?quality=12&learn=on)

Sling Models主控台位於：

+ 「工具>作業> Web Console >狀態> Sling Models」
+ 或直接在：[http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)

協助除錯活動的主要方面：

+ 驗證Sling Models會註冊至正確的資源類型
+ 驗證Sling Models可從正確的物件（Resource或SlingHttpRequestServlet）調整
+ 驗證Sling Model Exportorer正確註冊
