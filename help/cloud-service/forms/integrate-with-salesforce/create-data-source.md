---
title: 建立雲端服務設定
description: 使用OAuth憑證建立資料來源以連線至Salesforce
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms, Integrations
jira: KT-7148
thumbnail: 331755.jpg
exl-id: e2d56e91-c13e-4787-a97f-255938b5d290
duration: 173
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '75'
ht-degree: 16%

---

# 建立資料Source

使用先前步驟中建立的Swagger檔案建立REST支援的資料來源。

>[!VIDEO](https://video.tv.adobe.com/v/3447276?quality=12&learn=on&captions=chi_hant)

| 設定 | 值 |
|---------------------|-----------------------------------------------------------------|
| oAuth URL | https://login.salesforce.com/services/oauth2/authorize |
| 授權範圍 | api chatter_api完整id openid refresh_token visualforce網頁 |
| 重新整理記號 URL | https://newfocus-dev-ed.my.salesforce.com/services/oauth2/token |
| 存取記號 URL | https://newfocus-dev-ed.my.salesforce.com/services/oauth2/token |


**必須變更重新整理與存取權杖URL的網域名稱，以符合您的Salesforce帳戶設定**