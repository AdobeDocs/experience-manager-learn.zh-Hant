---
title: 整合AEM Forms as a Cloud Service和Marketo （第2部分）
description: 瞭解如何使用AEM Forms表單資料模型整合AEM Forms和Marketo。
feature: Form Data Model,Integration
version: Experience Manager as a Cloud Service
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="整合" type="positive"
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
last-substantial-update: 2024-07-24T00:00:00Z
jira: KT-15876
exl-id: 75e589fa-f7fc-4d0b-98c8-ce4d603ef2f7
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '220'
ht-degree: 3%

---

# 建立資料Source

Marketo的REST API已透過雙腿OAuth 2.0驗證。我們可以使用上一步中下載的swagger檔案輕鬆建立資料來源

## 建立設定容器

* 登入AEM。
* 按一下[工具]功能表，然後按一下&#x200B;**設定瀏覽器**，如下所示

* ![工具功能表](assets/datasource3.png)

* 按一下&#x200B;**建立**&#x200B;並提供有意義的名稱，如下所示。 請務必選取雲端設定選項，如下所示

* ![設定容器](assets/datasource4.png)

## 建立雲端服務

* 導覽至工具功能表，然後按一下雲端服務 — >資料來源

* ![雲端服務](assets/datasource5.png)

* 選取在先前步驟中建立的設定容器，然後按一下&#x200B;**建立**&#x200B;以建立新的資料來源。請提供有意義的名稱，然後從[服務型別]下拉式清單中選取RESTful服務，然後按一下[下一步]&#x200B;**&#x200B;**
* ![新資料來源](assets/datasource6.png)

* 上傳Swagger檔案，並指定您Marketo執行個體專屬的授予型別、使用者端ID、使用者端密碼和存取權杖URL，如下方熒幕擷取畫面所示。

* 測試連線，如果連線成功，請確定您按一下藍色的&#x200B;**建立**&#x200B;按鈕，以完成建立資料來源的程式。

* ![資料來源設定](assets/datasource1.png)


## 後續步驟

[建立表單資料模型](./part3.md)
