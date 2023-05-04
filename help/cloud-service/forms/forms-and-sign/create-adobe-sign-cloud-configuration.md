---
title: 建立Acrobat Sign Cloud設定Cloud Service
description: 使用雲端服務設定建立AEM Forms和Acrobat Sign整合。
solution: Experience Manager,Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 7428
thumbnail: 332437.jpg
exl-id: a55773a5-0486-413f-ada6-bb589315f0b1
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: tm+mt
source-wordcount: '159'
ht-degree: 0%

---

# 建立Acrobat Sign雲端設定

AEM中的雲端服務設定可讓您建立AEM與其他雲端應用程式之間的整合。

以下影片將逐步引導您完成建立雲端服務設定以整合AEM與Acrobat Sign所需的步驟

>[!VIDEO](https://video.tv.adobe.com/v/332437?quality=12&learn=on)

## 疑難排解

如果您在設定Abobe Sign雲端復寫設定時發生錯誤，可執行下列步驟以疑難排解
* 請確定Acrobat Sign API應用程式中指定的重新導向URL為下列格式
&lt;your instance=&quot;&quot; name=&quot;&quot;>/libs/adobesign/cloudservices/adobesign/createcloudconfigwizard/cloudservices.html/conf/&lt;container>.
例如 — https://author-p24107-e32034.adobeaemcloud.com/libs/adobesign/cloudservices/adobesign/createcloudconfigwizard/cloudservices.html/conf/FormsCS。 FormsCS是要存放雲端設定之容器的名稱
* 請確定oAuth url正確
* 檢查用戶端ID和用戶端密碼
* 嘗試無痕窗口模式

