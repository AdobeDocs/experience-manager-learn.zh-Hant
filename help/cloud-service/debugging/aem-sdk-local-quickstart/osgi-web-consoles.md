---
title: 使用OSGi Web主控台除錯AEM SDK
description: AEM SDK的本機Quickstart有一個OSGi Web主控台，提供各種本機AEM執行階段的資訊和說明，有助於瞭解AEM如何辨識您的應用程式和函式。
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
kt: 5265, 5366, 5267
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 0929bc1a-376c-4e16-a540-a276fd5af164
duration: 486
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '369'
ht-degree: 0%

---

# 使用OSGi Web主控台除錯AEM SDK

AEM SDK的本機Quickstart有一個OSGi Web主控台，提供各種本機AEM執行階段的資訊和說明，有助於瞭解AEM如何辨識您的應用程式和函式。

AEM提供許多OSGi主控台，每個主控台都提供AEM不同層面的關鍵深入分析，不過下列通常是除錯應用程式時最有用的地方。

## 組合

>[!VIDEO](https://video.tv.adobe.com/v/34335?quality=12&learn=on)

此套件組合控制檯是OSGi套件組合的目錄，其詳細資訊已部署到AEM，且具有啟動和停止這些套件組合的隨選功能。

套件組合主控台位於：

+ 「工具>作業> Web主控台> OSGi >套裝」
+ 或直接在： [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)

按一下每個套件組合，可提供協助您偵錯應用程式的詳細資訊。

+ 驗證OSGi套件組合是否存在
+ 驗證OSGi套件組合是否作用中
+ 判斷OSGi套件組合是否有未滿足的匯入導致其無法啟動

## 元件

>[!VIDEO](https://video.tv.adobe.com/v/34336?quality=12&learn=on)

「元件」主控台是部署至AEM之所有OSGi元件的目錄，並提供其相關的所有資訊，從已定義的OSGi元件生命週期，到他們可以參考的OSGi服務

元件主控台位於：

+ 工具>作業> Web主控台> OSGi >元件
+ 或直接在： [http://localhost:4502/system/console/components](http://localhost:4502/system/console/components)

協助偵錯活動的主要面向：

+ 驗證OSGi套件組合是否存在
+ 驗證OSGi套件組合是否作用中
+ 判斷OSGi套件組合是否有未滿足的匯入導致其無法啟動
+ 取得元件的PID，以便在Git中為其建立OSGi設定
+ 識別繫結至使用中OSGi設定的OSGi屬性值

## Sling 模型

>[!VIDEO](https://video.tv.adobe.com/v/34337?quality=12&learn=on)

Sling模型控制檯位於：

+ 工具>作業> Web主控台>狀態> Sling模型
+ 或直接在： [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)

協助偵錯活動的主要面向：

+ 驗證Sling模型已註冊到適當的資源型別
+ 驗證Sling模型是否可從正確的物件（資源或SlingHttpRequestServlet）改寫
+ 驗證Sling模型匯出工具已正確註冊
