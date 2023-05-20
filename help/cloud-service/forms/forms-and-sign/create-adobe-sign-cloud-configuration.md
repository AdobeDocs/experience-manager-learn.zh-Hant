---
title: 建立Acrobat Sign雲配置Cloud Service
description: 使用雲服務配置建立AEM Forms和Acrobat Sign整合。
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

# 建立Acrobat Sign雲配置

中的雲服務配AEM置允許您建立與其他雲應用AEM程式之間的整合。

以下視頻將引導您完成建立雲服務配置以與Acrobat Sign整合所需AEM的步驟

>[!VIDEO](https://video.tv.adobe.com/v/332437?quality=12&learn=on)

## 疑難排解

如果您在配置Abobe Sign雲克隆配置時遇到錯誤，可以採取以下步驟來進行錯誤拍攝
* 確保在Acrobat SignAPI應用程式中指定的重定向URL的格式如下
&lt;your instance=&quot;&quot; name=&quot;&quot;>/libs/adobesign/cloudservices/adobesign/createcloudconfigwizard/cloudservices.html/conf/&lt;container>。
例如 — https://author-p24107-e32034.adobeaemcloud.com/libs/adobesign/cloudservices/adobesign/createcloudconfigwizard/cloudservices.html/conf/FormsCS。 FormsCS是要保存雲配置的容器的名稱
* 確保oAuth URL正確
* 檢查客戶端ID和客戶端密碼
* 嘗試Incognito窗口模式

