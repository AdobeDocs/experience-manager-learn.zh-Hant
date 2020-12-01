---
title: 使用AEM體驗片段設定社交貼文
description: 「體驗片段」可讓行銷人員將在AEM中建立的體驗張貼至社交媒體平台。 以下影片詳細說明將體驗片段發佈至Facebook和Pinterest所需的設定和設定。
sub-product: 網站，內容服務
feature: experience-fragments
topics: integrations, content-delivery
audience: administrator, implementer, developer
doc-type: setup
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '168'
ht-degree: 0%

---


# 使用體驗片段設定社交貼文{#set-up-social-posting-with-experience-fragments}

「體驗片段」可讓行銷人員將在AEM中建立的體驗張貼至社交媒體平台。 以下影片詳細說明將體驗片段發佈至Facebook和Pinterest所需的設定和設定。

>[!VIDEO](https://video.tv.adobe.com/v/20592/?quality=9&learn=on)

*[體驗片段] -設定和設定社交貼文至Facebook和Pinterest*

## 設定要張貼至Facebook和Pinterest的體驗片段檢查清單

1. AEM Author Instance正在HTTPS上執行
2. Facebook帳戶+ Facebook開發人員應用程式
3. Pinterest帳戶+ Pinterest開發人員應用程式
4. [!UICONTROL AEM Cloud ] ServicesConfiguration - Facebook
5. [!UICONTROL AEM Cloud ] ServicesConfiguration - Pinterest
6. AEM Experience Fragment with Cloud Services for Facebook + Pinterest
7. 使用Facebook範本的體驗片段變異
8. 使用Pinterest範本的體驗片段變化

## 體驗片段重新導向URI

此URI會用於Facebook和Pinterest應用程式，作為Oauth流程的一部分。

```plain
 /* replace localhost:8443 with your aem host info */

 https://localhost:8443/libs/cq/experience-fragments/components/redirect
```

