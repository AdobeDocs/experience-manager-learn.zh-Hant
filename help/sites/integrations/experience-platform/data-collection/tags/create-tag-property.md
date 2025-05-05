---
title: 建立標籤屬性
description: 瞭解如何使用最低設定建立Tag屬性，以便與AEM整合。 使用者可瞭解標籤UI，並瞭解擴充功能、規則和發佈工作流程。
solution: Experience Manager, Data Collection, Experience Platform
jira: KT-5980
thumbnail: 38553.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="整合" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service、AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: d5de62ef-a2aa-4283-b500-e1f7cb5dec3b
duration: 606
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '593'
ht-degree: 0%

---

# 建立標籤屬性 {#create-tag-property}

瞭解如何使用最低設定建立Tag屬性，以便與Adobe Experience Manager整合。 使用者可瞭解標籤UI，並瞭解擴充功能、規則和發佈工作流程。

>[!VIDEO](https://video.tv.adobe.com/v/38553?quality=12&learn=on)

## 標籤屬性建立

若要建立Tag屬性，請完成以下步驟。

1. 在瀏覽器中，導覽至[Adobe Experience Cloud首頁](https://experience.adobe.com/)頁面，並使用您的Adobe ID登入。

1. 從Adobe Experience Cloud首頁的&#x200B;_快速存取_&#x200B;區段，按一下&#x200B;**資料彙集**&#x200B;應用程式。

1. 從左側導覽按一下&#x200B;**標籤**&#x200B;功能表專案，然後從右上角按一下&#x200B;**新增屬性**。

1. 使用&#x200B;**Name**&#x200B;必要欄位為您的Tag屬性命名。 在[網域]欄位中，輸入您的網域名稱，或如果使用AEM as a Cloud Service環境，請輸入`adobeaemcloud.com`並按一下[儲存]。**&#x200B;**

   ![標籤屬性](assets/tag-properties.png)

## 建立新規則

在&#x200B;**標籤屬性**&#x200B;檢視中按一下新建立的標籤屬性，以開啟該屬性。 在「_我最近的活動_」標題下，您應該會看到核心擴充功能已新增至其中。 核心標籤擴充功能是預設的擴充功能，並提供頁面載入、瀏覽器、表單和其他事件型別等基本事件型別。如需詳細資訊，請參閱[核心擴充功能概觀](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/core/overview.html?lang=zh-Hant)。

規則可讓您指定訪客與您的AEM網站互動時應該發生的事。 為了簡單起見，我們將兩則訊息記錄到瀏覽器主控台，以示範資料收集標籤整合如何將JavaScript程式碼插入您的AEM網站，而不需要更新AEM專案程式碼。

若要建立規則，請完成下列步驟。

1. 從左側導覽的&#x200B;_AUTHORING_&#x200B;區段中按一下&#x200B;**規則**，然後按一下&#x200B;**建立新規則**

1. 使用&#x200B;**Name**&#x200B;必要欄位為規則命名。

1. 從&#x200B;_EVENTS_&#x200B;區段按一下&#x200B;**新增**，然後在&#x200B;_事件組態_&#x200B;表單的&#x200B;**事件型別**&#x200B;下拉式清單中，選取&#x200B;_載入的程式庫（頁面頂端）_&#x200B;選項，然後按一下&#x200B;**保留變更**。

1. 從&#x200B;_動作_&#x200B;區段中按一下&#x200B;**新增**，然後在&#x200B;_動作組態_&#x200B;表單的&#x200B;**動作型別**&#x200B;下拉式清單中，選取&#x200B;_自訂程式碼_&#x200B;選項，然後按一下&#x200B;**開啟編輯器**。

1. 在&#x200B;_編輯代碼_&#x200B;強制回應視窗中，輸入下列JavaScript代碼片段，然後按一下&#x200B;**儲存**，最後按一下&#x200B;**保留變更**。

   ```javascript
   console.log('Tags Property loaded, all set for...');
   console.log('capabilities such as capturing data, conversion tracking and delivering unique and personalized experiences');
   ```

1. 按一下&#x200B;**儲存**&#x200B;以完成規則建立程式。

   ![新規則](assets/new-rule.png)

## 新增程式庫並發佈

Tag屬性&#x200B;_規則_&#x200B;是使用程式庫啟動，請將程式庫視為包含JavaScript程式碼的套件。 按照以下步驟啟動新建立的規則。

1. 從左側導覽的&#x200B;_PUBLISHING_&#x200B;區段中，按一下&#x200B;**發佈流程**，然後按一下&#x200B;**新增資料庫**

1. 使用&#x200B;**名稱**&#x200B;欄位為資料庫命名，並為&#x200B;**環境**&#x200B;下拉式清單選取&#x200B;_開發（開發）_&#x200B;選項。

1. 若要選取自標籤屬性建立後所有變更的資源，請按一下[新增所有變更的資源] **。** 此動作會將新建立的規則和核心擴充功能資源新增至程式庫。 最後按一下&#x200B;**儲存並建置至開發**。

1. 一旦為&#x200B;**開發**&#x200B;泳道建置資料庫，使用&#x200B;_點_&#x200B;選取&#x200B;**提交核准**

1. 然後在&#x200B;**已提交**&#x200B;使用省略符號的泳道&#x200B;_中_&#x200B;選取&#x200B;**核准發佈**，同樣地，在&#x200B;**已核准**&#x200B;泳道中&#x200B;**建置和Publish至生產環境**。

![已發佈的資料庫](assets/published-library.png)


上述步驟會完成簡單的標籤屬性建立，接著會有規則在頁面載入時將訊息記錄至瀏覽器主控台。 此外，規則和核心擴充功能也會透過建立程式庫發佈。

## 後續步驟

[使用IMS連線AEM與標籤屬性](connect-aem-tag-property-using-ims.md)


## 其他資源 {#additional-resources}

* [建立標籤屬性](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/create-a-property.html?lang=zh-Hant)
