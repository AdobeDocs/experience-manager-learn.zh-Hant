---
title: 日誌記錄和AEM活動
description: 瞭解如何從日誌中擷取初始的AEM事件集，並探索每個事件的詳細資訊。
version: Experience Manager as a Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 280
last-substantial-update: 2023-01-29T00:00:00Z
jira: KT-14734
thumbnail: KT-14734.jpeg
exl-id: 33eb0757-f0ed-4c2d-b8b9-fa6648e87640
source-git-commit: e01eb7ff050321a70d84f8a613705799017dbf5d
workflow-type: tm+mt
source-wordcount: '579'
ht-degree: 1%

---

# 日誌記錄和AEM活動

瞭解如何從日誌中擷取初始的AEM事件集，並探索每個事件的詳細資訊。

>[!VIDEO](https://video.tv.adobe.com/v/3427052?quality=12&learn=on)

日誌是一種用於消耗AEM事件的提取方法，而日誌是一種排序的事件清單。 您可以使用Adobe I/O Events Journaling API從日誌擷取AEM事件，並在應用程式中處理。 此方法可讓您根據指定的步調管理事件，並有效率地大量處理這些事件。 請參閱[日誌](https://developer.adobe.com/events/docs/guides/journaling_intro/)以取得深入分析，包括保留期、分頁等基本考量。

在Adobe Developer Console專案中，每個事件註冊都會自動啟用日誌，以啟用順暢整合。

>[!IMPORTANT]
>
>此教學課程中的即時示範端點先前託管於[問題](https://glitch.com/)。 自2025年7月起，Glitch已終止託管服務，且無法再存取端點。
>>我們正在積極努力將示範移轉至其他平台。 教學課程內容會維持正確狀態，而且我們很快會提供更新的連結。
>>感謝您的理解和耐心。

使用您自己的應用程式，直到即時示範端點再次可用為止。

## 先決條件

若要完成本教學課程，您需要：

- 已啟用[AEM事件的AEM as a Cloud Service環境](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment)。

- 已針對Adobe Developer Console事件設定[AEM專案](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#how-to-subscribe-to-aem-events-in-the-adobe-developer-console)。

## 存取網頁應用程式

若要存取Adobe提供的Web應用程式，請遵循下列步驟：

- 確認您可以在新的瀏覽器分頁中存取[Glitch — 託管的Web應用程式](https://indigo-speckle-antler.glitch.me/)。

  ![問題 — 託管的Web應用程式](../assets/examples/journaling/glitch-hosted-web-application.png)

## 收集Adobe Developer Console專案詳細資料

若要從日誌擷取AEM事件，需要&#x200B;_IMS組織ID_、_使用者端ID_&#x200B;和&#x200B;_存取權杖_&#x200B;等認證。 若要收集這些認證，請依照下列步驟進行：

- 在[Adobe Developer Console](https://developer.adobe.com)中，導覽至您的專案並按一下以開啟專案。

- 在&#x200B;**認證**&#x200B;區段下，按一下&#x200B;**OAuth伺服器對伺服器**&#x200B;連結以開啟&#x200B;**認證詳細資料**&#x200B;標籤。

- 按一下&#x200B;**產生存取權杖**&#x200B;按鈕，以產生存取權杖。

  ![Adobe Developer Console專案產生存取權杖](../assets/examples/journaling/adobe-developer-console-project-generate-access-token.png)

- 複製&#x200B;**產生的存取權杖**、**使用者端識別碼**&#x200B;和&#x200B;**組織識別碼**。 在本教學課程的後半部分，您會需要用到這些資訊。

  ![Adobe Developer Console專案複製認證](../assets/examples/journaling/adobe-developer-console-project-copy-credentials.png)

- 每個事件註冊都會自動啟用日誌。 若要取得事件註冊的&#x200B;_唯一日誌API端點_，請按一下訂閱AEM事件的事件卡。 從&#x200B;**註冊詳細資料**&#x200B;索引標籤，複製&#x200B;**JOURNALING UNIQUE API端點**。

  ![Adobe Developer Console專案活動卡](../assets/examples/journaling/adobe-developer-console-project-events-card.png)

## 載入 AEM 事件日誌

為了簡單起見，此託管Web應用程式只會從日誌擷取第一批AEM事件。 這些是日誌中最舊可用的事件。 如需詳細資訊，請參閱[第一批事件](https://developer.adobe.com/events/docs/guides/api/journaling_api/#fetching-your-first-batch-of-events-from-the-journal)。

- 在[問題 — 託管的Web應用程式](https://indigo-speckle-antler.glitch.me/)中，輸入您先前從Adobe Developer Console專案複製的&#x200B;**IMS組織ID**、**使用者端ID**&#x200B;和&#x200B;**存取權杖**，然後按一下&#x200B;**提交**。

- 成功後，表格元件會顯示AEM事件日誌資料。

  ![AEM事件日誌資料](../assets/examples/journaling/load-journal.png)

- 若要檢視完整的事件裝載，請連按兩下該列。 您可以看到AEM事件詳細資料具有在webhook中處理事件所需的所有必要資訊。 例如，事件型別(`type`)、事件來源(`source`)、事件識別碼(`event_id`)、事件時間(`time`)和事件資料(`data`)。

  ![完成AEM事件承載](../assets/examples/journaling/complete-journal-data.png)

## 其他資源

- [Adobe I/O Events Journaling API](https://developer.adobe.com/events/docs/guides/api/journaling_api/)提供該API的詳細資訊，例如第一個、下一個和最後一個批次事件、分頁等等。
