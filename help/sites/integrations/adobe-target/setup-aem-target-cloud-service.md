---
title: 建立Adobe Target Cloud服務帳戶
description: 逐步逐步逐步逐步說明如何使用雲端服務和Adobe IMS驗證將Adobe Experience Manager整合為雲端服務與Adobe Target
feature: cloud-services
topics: integrations, administration, development
audience: administrator, developer
doc-type: technical video
activity: setup
version: cloud-service
kt: 6044
thumbnail: 41244.jpg
translation-type: tm+mt
source-git-commit: 25ca90f641aaeb93fc9319692f3b099d6b528dd1
workflow-type: tm+mt
source-wordcount: '193'
ht-degree: 0%

---


# 建立Adobe Target Cloud服務帳戶 {#adobe-target-cloud-service}

逐步逐步逐步說明如何使用雲端服務和Adobe IMS驗證將Adobe Experience Manager整合為雲端服務與Adobe Target。

>[!VIDEO](https://video.tv.adobe.com/v/41244?quality=12&learn=on)

>[!CAUTION]
>
>視訊中顯示的Adobe Target Cloud Services設定有已知問題。 建議您改為依照視訊中的相同步驟，但使用舊 [版Adobe Target Cloud服務設定](https://docs.adobe.com/content/help/en/experience-manager-learn/aem-target-tutorial/aem-target-implementation/using-aem-cloud-services.html)。

## 常見問題

將體驗片段匯出至Adobe Target時，如果您的管理控制台中的Target整合沒有正確的權限，您可能會收到如下所示的錯誤。

**UI錯誤**![目標API UI錯誤](assets/error-target-offer.png)

**記錄錯誤**![目標API控制台錯誤](assets/target-console-error.png)


**解決方案**

1. 導覽至管 [理控制台](https://adminconsole.adobe.com/)
2. 選擇產品> Adobe Target >產品設定檔
3. 在「整合」標籤下，選取您的整合（Adobe I/O專案）
4. 指派編輯者或核准者角色。

![目標API錯誤](assets/target-permissions.png)

將適當權限新增至Target整合應有助於解決上述問題。