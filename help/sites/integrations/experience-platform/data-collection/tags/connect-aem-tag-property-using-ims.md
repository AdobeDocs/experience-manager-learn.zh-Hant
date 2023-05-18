---
title: 使用IMS連線AEM與標籤屬性
description: 了解如何在AEM中使用IMS設定，將AEM與標籤屬性連線。 此設定會使用Launch API驗證AEM，並允許AEM透過Launch API通訊以存取標籤屬性。
topics: integrations
audience: administrator
solution: Experience Manager, Data Collection, Experience Platform
doc-type: technical video
activity: setup
kt: 5981
thumbnail: 38555.jpg
topic: Integrations
role: Developer
level: Intermediate
exl-id: 92dbd185-bad4-4a4d-b979-0d8f5d47c54b
source-git-commit: 1d2daf53cd28fcd35cb2ea5c50e29b447790917a
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 0%

---

# 使用IMS連線AEM與標籤屬性{#connect-aem-and-tag-property-using-ims}

>[!NOTE]
>
>AEM產品UI、內容和檔案中已實作將Adobe Experience Platform Launch重新命名為一組資料收集技術的程式，因此此處仍使用Launch一詞。

了解如何使用AEM中的IMS(Identity Management系統)設定，將AEM與標籤屬性連線。 此設定會使用Launch API驗證AEM，並允許AEM透過Launch API通訊以存取標籤屬性。

## 建立或重複使用IMS設定

必須具備使用Adobe Developer Console專案的IMS設定，才能將AEM與新建立的標籤屬性整合。 此設定可讓AEM透過Launch API與標籤應用程式通訊，而IMS可處理這項整合的安全性方面。

每當布建AEM asCloud Service環境時，就會自動建立一些IMS設定，例如Asset compute、Adobe Analytics和Adobe啟動。 自動建立 **Adobe啟動** 如果您使用AEM 6.X環境，則可使用IMS設定，或建立新的IMS設定。

審核自動建立 **Adobe啟動** 使用下列步驟進行IMS設定。

1. 在AEM中開啟 **工具** 功能表

1. 在「安全性」區段中，選取「Adobe IMS設定」 。

1. 選取 **Adobe啟動** 卡片，按一下 **屬性**，請檢閱 **憑證** 和 **帳戶** 頁簽。 然後按一下 **取消** 返回而不修改任何自動建立的詳細資訊。

1. 選取 **Adobe啟動** 卡片，這次按一下 **檢查運行狀況**，您應該會看到 **成功** 訊息如下。

   ![Adobe啟動狀況良好的IMS設定](assets/adobe-launch-healthy-ims-config.png)


## 後續步驟

[在AEM中建立LaunchCloud Service設定](create-aem-launch-cloud-service.md)
