---
title: 使用IMS連線AEM Sites與標籤屬性
description: 瞭解如何使用AEM中的IMS設定連結AEM Sites與標籤屬性。 此設定可使用Launch API驗證AEM，並允許AEM透過Launch API通訊以存取標籤屬性。
topics: integrations
audience: administrator
solution: Experience Manager, Data Collection, Experience Platform
doc-type: technical video
activity: setup
kt: 5981
thumbnail: 38555.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="整合" type="positive"
badgeVersions: label="AEM Sitesas a Cloud Service、AEM Sites 6.5" before-title="false"
exl-id: 92dbd185-bad4-4a4d-b979-0d8f5d47c54b
source-git-commit: b044c9982fc9309fb73509dd3117f5467903bd6a
workflow-type: tm+mt
source-wordcount: '313'
ht-degree: 0%

---

# 使用IMS連線AEM Sites與標籤屬性{#connect-aem-and-tag-property-using-ims}

>[!NOTE]
>
>AEM產品UI、內容和檔案正在實施將Adobe Experience Platform Launch重新命名為一組資料收集技術的程式，因此此處仍使用Launch一詞。

瞭解如何使用AEM中的IMS (Identity Management系統)設定來連結AEM與標籤屬性。 此設定可使用Launch API驗證AEM，並允許AEM透過Launch API通訊以存取標籤屬性。

## 建立或重新使用IMS設定

需要使用Adobe Developer控制檯專案的IMS設定，才能將AEM與新建立的標籤屬性整合。 此設定可讓AEM使用Launch API與Tags應用程式通訊，而IMS會處理此整合的安全性方面。

每當布建AEM as Cloud Service環境時，就會自動建立一些IMS設定，例如Asset compute、Adobe Analytics和Adobe Launch。 已自動建立 **Adobe啟動** 如果您使用AEM 6.X環境，則可以使用IMS設定或應建立新的IMS設定。

評論自動建立 **Adobe啟動** 使用以下步驟進行IMS設定。

1. 在AEM中開啟 **工具** 功能表

1. 在「安全性」區段中，選取「Adobe IMS設定」。

1. 選取 **Adobe啟動** 卡片並按一下 **屬性**，檢閱以下專案的詳細資料： **憑證** 和 **帳戶** 索引標籤。 然後按一下 **取消** 返回，而不修改任何自動建立的詳細資訊。

1. 選取 **Adobe啟動** 卡片，這次按一下 **檢查健康狀態**，您應該會看到 **成功** 訊息如下。

   ![Adobe啟動狀況良好的IMS設定](assets/adobe-launch-healthy-ims-config.png)


## 後續步驟

[在AEM中建立LaunchCloud Service設定](create-aem-launch-cloud-service.md)
