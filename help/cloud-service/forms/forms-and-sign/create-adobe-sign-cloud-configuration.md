---
title: 建立Acrobat Sign雲端設定Cloud Service
description: 使用雲端服務設定建立AEM Forms與Acrobat Sign整合。
solution: Experience Manager,Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-7428
thumbnail: 332437.jpg
badgeIntegration: label="整合" type="positive"
badgeVersions: label="AEM Formsas a Cloud Service" before-title="false"
exl-id: a55773a5-0486-413f-ada6-bb589315f0b1
duration: 229
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '155'
ht-degree: 1%

---

# 建立Acrobat Sign雲端設定

AEM中的雲端服務設定可讓您建立AEM與其他雲端應用程式之間的整合。

以下影片將逐步說明建立雲端服務設定以將AEM與Acrobat Sign整合所需的步驟

>[!VIDEO](https://video.tv.adobe.com/v/332437?quality=12&learn=on)

## 疑難排解

如果您在設定Abobe Sign雲端設定時發生錯誤，可採取下列步驟進行疑難排解
* 請確定Acrobat Sign API應用程式中指定的重新導向URL格式如下
&lt;your instance=&quot;&quot; name=&quot;&quot;>/libs/adobesign/cloudservices/adobesign/createcloudconfigwizard/cloudservices.html/conf/&lt;container>.
例如 — https://author-p24107-e32034.adobeaemcloud.com/libs/adobesign/cloudservices/adobesign/createcloudconfigwizard/cloudservices.html/conf/FormsCS。 FormsCS是要保留雲端設定的容器名稱
* 請確定oAuth URL正確
* 檢查您的使用者端ID和使用者端密碼
* 嘗試無痕視窗模式

