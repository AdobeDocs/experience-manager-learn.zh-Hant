---
title: 調試標籤實現
description: 介紹調試Tags實現的一些常用工具和技術。 瞭解如何使用瀏覽器的開發人員控制台和Experience Platform調試器擴展來識別和排除標籤實現的關鍵方面。
topics: integrations
audience: administrator
solution: Experience Manager, Data Collection, Experience Platform
doc-type: technical video
activity: setup
kt: 6047
thumbnail: 38567.jpg
topic: Integrations
role: Developer
level: Intermediate
exl-id: 647447ca-3c29-4efe-bb3a-d3f53a936a2a
source-git-commit: 1d2daf53cd28fcd35cb2ea5c50e29b447790917a
workflow-type: tm+mt
source-wordcount: '282'
ht-degree: 0%

---

# 調試標籤實現 {#debug-tags-implementation}

介紹用於調試標籤實現的常用工具和技術。 瞭解如何使用瀏覽器的開發人員控制台和Experience Platform調試器擴展來識別和排除標籤實現的關鍵方面。

>[!VIDEO](https://video.tv.adobe.com/v/38567?quality=12&learn=on)

## 通過Satellite對象進行客戶端調試

客戶端調試有助於驗證Tag屬性規則載入或執行順序。 只要將Tag屬性添加到網站， `_satellite` JavaScript對象存在於瀏覽器中，以便於客戶端事件和資料跟蹤。

要啟用客戶端調試，請調用 `setDebug(true)` 方法 `_satellite` 的雙曲餘切值。

1. 開啟瀏覽器控制台，然後運行以下命令。

   ```javascript
       _satellite.setDebug(true);
   ```

1. 重新加AEM載站點頁，並驗證控制台日誌是否顯示 _已觸發_ 下面的消息。

   ![「作者」和「發佈」頁上的標籤屬性](assets/satellite-object-debugging.png)

## 通過Adobe Experience Platform調試器調試

Adobe提供Adobe Experience Platform調試器 [鉻延伸](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) 和 [Firefox載入項](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/) 調試、瞭解和深入瞭解整合。

1. 開啟Adobe Experience Platform調試器擴展，並開啟發佈實例上的網站頁

1. 在 **Adobe Experience Platform調試器>摘要>Adobe Experience Platform標籤** 部分，驗證標籤屬性詳細資訊，如名稱、版本、生成日期、環境和擴展。

   ![Adobe Experience Platform調試器和標籤屬性詳細資訊](assets/tag-property-details.png)

## 其他資源 {#additional-resources}

+ [Adobe Experience Platform調試器簡介](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)

+ [衛星對象參考](https://experienceleague.adobe.com/docs/experience-platform/tags/client-side/satellite-object.html)
