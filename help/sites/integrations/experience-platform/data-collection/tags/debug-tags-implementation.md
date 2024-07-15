---
title: 對標籤實作除錯
description: 介紹對標籤實作進行偵錯的一些常用工具和技術。 瞭解如何使用瀏覽器的開發人員主控台和Experience Platform Debugger擴充功能，識別及疑難排解Tags實作的重大問題。
solution: Experience Manager, Data Collection, Experience Platform
jira: KT-6047
thumbnail: 38567.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="整合" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service、AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 647447ca-3c29-4efe-bb3a-d3f53a936a2a
duration: 259
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 1%

---

# 對標籤實作除錯 {#debug-tags-implementation}

介紹對標籤實作進行偵錯時常用的工具和技術。 瞭解如何使用瀏覽器的開發人員主控台和Experience Platform Debugger擴充功能，識別及疑難排解Tags實作的重大問題。

>[!VIDEO](https://video.tv.adobe.com/v/38567?quality=12&learn=on)

## 透過Satellite物件進行使用者端除錯

使用者端除錯有助於驗證Tag屬性規則載入或執行順序。 每當將Tag屬性新增至網站時，瀏覽器中都會顯示`_satellite` JavaScript物件，以利使用者端事件和資料追蹤。

若要啟用使用者端偵錯，請呼叫`_satellite`物件上的`setDebug(true)`方法。

1. 開啟瀏覽器主控台，然後執行下列命令。

   ```javascript
       _satellite.setDebug(true);
   ```

1. 重新載入AEM網站頁面，並確認主控台記錄檔顯示如下所示的&#x200B;_已引發規則_&#x200B;訊息。

   作者和Publish頁面上的![標籤屬性](assets/satellite-object-debugging.png)

## 透過Adobe Experience Platform Debugger除錯

Adobe提供Adobe Experience Platform Debugger[Chrome擴充功能](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)以偵錯、瞭解及深入分析整合。

1. 開啟Adobe Experience Platform Debugger擴充功能，並在Publish例項上開啟網站頁面

2. 在&#x200B;**Adobe Experience Platform Debugger>摘要> Adobe Experience Platform Tags**&#x200B;區段中，驗證Tag屬性詳細資訊，例如，名稱、版本、建置日期、環境和擴充功能。

   ![Adobe Experience Platform Debugger和標籤屬性詳細資料](assets/tag-property-details.png)

## 其他資源 {#additional-resources}

+ [Adobe Experience Platform Debugger簡介](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)

+ [Satellite物件參考](https://experienceleague.adobe.com/docs/experience-platform/tags/client-side/satellite-object.html)
