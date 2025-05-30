---
title: 使用IMS連線AEM Sites與標籤屬性
description: 瞭解如何使用AEM中的IMS設定連結AEM Sites與標籤屬性。
solution: Experience Manager, Data Collection, Experience Platform
jira: KT-5981
thumbnail: 38555.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="整合" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service、AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 92dbd185-bad4-4a4d-b979-0d8f5d47c54b
duration: 50
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 1%

---

# 使用IMS連線AEM Sites與標籤屬性{#connect-aem-and-tag-property-using-ims}

瞭解如何使用AEM中的IMS (Identity Management系統)設定來連結AEM與標籤屬性。 此設定使用標籤API驗證AEM，並允許AEM透過標籤API通訊以存取標籤屬性。

## 建立或重複使用IMS設定

需要使用Adobe Developer Console專案的IMS設定，才能將AEM與新建立的標籤屬性整合。 此設定允許AEM使用標籤API與標籤應用程式通訊，並且IMS會處理此整合的安全性方面。

每當為AEM as Cloud Service環境布建時，就會自動建立一些IMS設定，例如Asset Compute、Adobe Analytics和標籤。 若您使用AEM 6.X環境，可使用Adobe Experience Platform **IMS設定中自動建立的**&#x200B;標籤，或應建立新的IMS設定。

檢閱在Adobe Experience Platform **IMS設定中自動建立的**&#x200B;標籤（使用下列步驟）。

1. 在AEM Author中，開啟&#x200B;**工具**&#x200B;功能表
1. 在「安全性」區段中，選取「Adobe IMS設定」。
1. 選取&#x200B;**Adobe啟動**&#x200B;卡片，然後按一下&#x200B;**屬性**，檢閱&#x200B;**憑證**&#x200B;和&#x200B;**帳戶**&#x200B;標籤的詳細資料。 然後按一下[取消]返回，而不修改任何自動建立的詳細資料。**&#x200B;**
1. 選取&#x200B;**Adobe啟動**&#x200B;卡，這次按一下&#x200B;**檢查健康狀態**，您應該會看到如下的&#x200B;**成功**&#x200B;訊息。

   ![標籤狀況良好的IMS設定](assets/adobe-launch-healthy-ims-config.png)

## 後續步驟

[在AEM中建立標籤Cloud Service設定](create-aem-launch-cloud-service.md)
