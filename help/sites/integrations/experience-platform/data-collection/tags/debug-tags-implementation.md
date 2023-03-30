---
title: 為標籤實作除錯
description: 介紹調試「標籤」實施的一些常用工具和技術。 了解如何使用瀏覽器的開發人員主控台和Experience Platform偵錯工具擴充功能，識別及疑難排解標籤實作的重大問題。
topics: integrations
audience: administrator
solution: Experience Manager, Data Collection, Experience Platform
doc-type: technical video
activity: setup
version: Cloud Service
kt: 6047
thumbnail: 38567.jpg
topic: Integrations
role: Developer
level: Intermediate
exl-id: 647447ca-3c29-4efe-bb3a-d3f53a936a2a
source-git-commit: 18a72187290d26007cdc09c45a050df8f152833b
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# 為標籤實作除錯 {#debug-tags-implementation}

介紹用來除錯標籤實作的常用工具和技術。 了解如何使用瀏覽器的開發人員主控台和Experience Platform偵錯工具擴充功能，識別及疑難排解標籤實作的重大問題。

>[!VIDEO](https://video.tv.adobe.com/v/38567?quality=12&learn=on)

## 透過Satellite物件進行用戶端除錯

用戶端除錯有助於驗證標籤屬性規則載入或執行順序。 每當有標籤屬性新增至網站時， `_satellite` 瀏覽器中存在JavaScript物件，以方便用戶端事件和資料追蹤。

若要啟用用戶端除錯，請呼叫 `setDebug(true)` 方法 `_satellite` 物件。

1. 開啟瀏覽器主控台，然後執行以下命令。

   ```javascript
       _satellite.setDebug(true);
   ```

1. 重新載入AEM網站頁面，並驗證主控台記錄顯示 _引發規則_ 訊息如下。

   ![製作和發佈頁面上的標籤屬性](assets/satellite-object-debugging.png)

## 透過Adobe Experience Platform Debugger除錯

Adobe提供Adobe Experience Platform Debugger [Chrome擴充功能](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) 和 [Firefox附加元件](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/) 除錯、了解及深入了解整合。

1. 開啟Adobe Experience Platform Debugger擴充功能，並在Publish執行個體上開啟網站頁面

1. 在 **Adobe Experience Platform Debugger >摘要> Adobe Experience Platform標籤** 區段中，確認您的「標籤」屬性詳細資訊，例如名稱、版本、建置日期、環境和擴充功能。

   ![Adobe Experience Platform Debugger和標籤屬性詳細資訊](assets/tag-property-details.png)

## 其他資源 {#additional-resources}

+ [Adobe Experience Platform Debugger簡介](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)

+ [衛星對象參考](https://experienceleague.adobe.com/docs/experience-platform/tags/client-side/satellite-object.html)
