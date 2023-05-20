---
title: 建立標籤屬性
description: 瞭解如何建立具有要與整合的裸機最小配置的Tag屬AEM性。 用戶將介紹到標籤UI，並瞭解擴展、規則和發佈工作流。
topics: integrations
audience: administrator
solution: Experience Manager, Data Collection, Experience Platform
doc-type: technical video
activity: setup
kt: 5980
thumbnail: 38553.jpg
topic: Integrations
role: Developer
level: Intermediate
exl-id: d5de62ef-a2aa-4283-b500-e1f7cb5dec3b
source-git-commit: 1d2daf53cd28fcd35cb2ea5c50e29b447790917a
workflow-type: tm+mt
source-wordcount: '598'
ht-degree: 1%

---

# 建立標籤屬性 {#create-tag-property}

瞭解如何建立具有最小裸配置的Tag屬性以與Adobe Experience Manager整合。 用戶將介紹到標籤UI，並瞭解擴展、規則和發佈工作流。

>[!VIDEO](https://video.tv.adobe.com/v/38553?quality=12&learn=on)

## 標籤屬性建立

要建立Tag屬性，請完成以下步驟。

1. 在瀏覽器中，導航到 [Adobe Experience Cloud家](https://experience.adobe.com/) 頁面和登錄，Adobe ID。

1. 按一下 **資料收集** 應用程式 _快速訪問_ 的子菜單。

1. 按一下 **標籤** 菜單項，然後按一下 **新建屬性** 從右上角。

1. 使用 **名稱** 欄位。 在「域」欄位中，輸入域名，或者如果使用AEMas a Cloud Service環境，請輸入 `adobeaemcloud.com` 按一下 **保存**。

   ![標籤屬性](assets/tag-properties.png)

## 建立新規則

通過按一下新建立的「標籤」屬性的名稱，開啟 **標籤屬性** 的子菜單。 也在 _我最近的活動_ 標題中，您應看到核心擴展已添加到其中。 核心標籤擴展是預設擴展，它提供了基本事件類型，如頁面載入、瀏覽器、表單和其他事件類型，請參閱 [核心擴展概述](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/core/overview.html) 的子菜單。

規則允許您指定訪問者與站點交互時應發生AEM的事件。 為了保持簡單，讓我們將兩條消息記錄到瀏覽器控制台，以演示資料收集標籤整合如何在不更新項目代碼的情況下將JavaScriptAEM代碼注AEM入您的站點。

要建立規則，請完成以下步驟。

1. 按一下 **規則** 從 _創作_ ，然後按一下 **建立新規則**

1. 使用 **名稱** 欄位。

1. 按一下 **添加** 從 _事件_ 的 _事件配置_ 的子菜單。 **事件類型** 下拉清單選擇 _已載入庫（頁面頂部）_ 選項 **保留更改**。

1. 按一下 **添加** 從 _操作_ 的 _操作配置_ 的子菜單。 **操作類型** 下拉清單選擇 _自定義代碼_ 選項 **開啟編輯器**。

1. 在 _編輯代碼_ 模式，輸入以下JavaScript代碼段，然後按一下 **保存**，最後按一下 **保留更改**。

   ```javascript
   console.log('Tags Property loaded, all set for...');
   console.log('capabilities such as capturing data, conversion tracking and delivering unique and personalized experiences');
   ```

1. 按一下 **保存** 完成規則建立過程。

   ![新規則](assets/new-rule.png)

## 添加庫並發佈

Tag屬性 _規則_ 使用庫激活，請將庫視為包含JavaScript代碼的包。 按照步驟激活新建立的規則。

1. 按一下 **發佈流** 從 _發佈_ ，然後按一下 **添加庫**

1. 使用 **名稱** 選擇 _發展（發展）_ 選項 **環境** 下拉清單。

1. 要選擇自「標籤」屬性建立以來所有更改的資源，請按一下 **+添加所有更改的資源**。 此操作會將新建立的規則和核心擴展資源添加到庫中。 最後按一下 **保存並構建到開發**。

1. 一旦為 **開發** 泳道，使用 _橢圓_ 選擇 **提交以供審批**

1. 在 **已提交** 泳道 _橢圓_ 選擇 **批准發佈**&#x200B;同樣 **生成並發佈到生產** 的 **已批准** 游泳道。

![已發佈庫](assets/published-library.png)


上述步驟完成簡單標籤屬性的建立，該屬性具有在載入頁面時將消息記錄到瀏覽器控制台的規則。 此外，通過建立庫來發佈規則和核心擴展。

## 後續步驟

[使用AEMIMS與標籤屬性連接](connect-aem-tag-property-using-ims.md)


## 其他資源 {#additional-resources}

* [建立標籤屬性](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/create-a-property.html)
