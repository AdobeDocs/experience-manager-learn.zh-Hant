---
title: 無頭的高級概AEM念 — GraphQL
description: 一個端到端教程，演示Adobe Experience Manager(AEM)GraphQL API的高級概念。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
source-git-commit: 83e16ea87847182139982ea2378d8ff9f079c968
workflow-type: tm+mt
source-wordcount: '926'
ht-degree: 0%

---

# 無頭的高級概AEM念

本端到端教程繼續 [基本教程](../multi-step/overview.md) 涵蓋了Adobe Experience Manager(AEM)Headless和GraphQL的基礎知識。 高級教程說明了使用內容片段模型、內容片段和AEMGraphQL API的深入方面，包括在客戶端應AEM用程式中使用GraphQL。

## 必備條件

完成 [快速設定AEMas a Cloud Service](../quick-setup/cloud-service.md) 配置環境。

強烈建議您完成上一個 [基本教程](../multi-step/overview.md) 和 [視頻系列](../video-series/modeling-basics.md) 教程，然後繼續本高級教程。 雖然您可以使用本地環境完成本教AEM程，但本教程僅涵蓋as a Cloud Service的工作AEM流。

## 目標

本教程介紹以下主題：

* 使用驗證規則和更高級的資料類型（如制表符佔位符、嵌套片段引用、JSON對象以及日期和時間資料類型）建立內容片段模型。
* 使用嵌套內容和片段引用時建立內容片段，並為內容片段創作管理配置資料夾策略。
* 使用AEM帶變數和指令的GraphQL查詢瀏覽GraphQL API功能。
* 具有中參數的永續GraphQL查AEM詢，並瞭解如何將快取控制參數用於永續查詢。
* 使用無頭JavaScript SDK將永續查詢請求整合到示例WKND GraphQL React應AEM用中。

## 無頭概AEM述的高級概念

以下視頻概括介紹了本教程中介紹的概念。 本教程包括定義具有更高級資料類型的內容片段模型、嵌套內容片段以及在中保留GraphQL查AEM詢。

>[!VIDEO](https://video.tv.adobe.com/v/340035/?quality=12&learn=on)

## 項目設定

WKND站點項目具有所有必要的配置，因此您可以在完成以下任務後立即啟動本教程 [快速設定](../quick-setup/cloud-service.md)。 本節僅重點介紹建立自己的無頭項目時可使用的AEM一些重要步驟。

### 建立配置

在中啟動任何新項目的第一步AEM是建立其配置（作為工作區）和建立GraphQL API端點。 要查看或建立配置，請導航至 **工具** > **常規** > **配置瀏覽器**。

![導航到配置瀏覽器](assets/overview/create-configuration.png)

請注意，已為本教程建立了WKND站點配置。 要為您自己的項目建立配置，請選擇 **建立** 在右上角，並在出現的「建立配置」(Create Configuration)模式中填寫表單。

### 建立GraphQL API終結點

接下來，必須配置API終結點以將GraphQL查詢發送到。 要查看現有終結點或建立終結點，請導航至 **工具** > **資產** > **圖形QL**。

![配置終結點](assets/overview/endpoints.png)

注意已建立全局端點和WKND端點。 要為項目建立終結點，請選擇 **建立** 按照工作流。

>[!NOTE]
>
> 保存終結點後，您將看到有關訪問安全控制台的模式，該模式允許您在要配置對終結點的訪問時調整安全設定。 但是，安全權限本身不在本教程的範圍之內。 有關詳細資訊，請參閱 [AEM文](https://experienceleague.adobe.com/docs/experience-manager-64/administering/security/security.html)。

### 為項目建立語言根資料夾

語言根資料夾是一個資料夾，其名稱為ISO語言代碼，如EN或FR。 翻譯AEM管理系統使用這些資料夾來定義內容的主要語言和內容翻譯語言。

轉到 **導航** > **資產** > **檔案**。

![導航到檔案](assets/overview/files.png)

導航到 **WKND站點** 的子菜單。 觀察標題為「English」和名稱為「EN」的資料夾。 此資料夾是WKND站點項目的語言根資料夾。

![英文資料夾](assets/overview/english.png)

對於您自己的項目，在配置內建立一個語言根資料夾。 請參閱 [建立資料夾](/help/headless-tutorial/graphql/advanced-graphql/author-content-fragments.md#create-folders) 的子菜單。

### 為嵌套資料夾分配配置

最後，必須將項目的配置分配給語言根資料夾。 此分配允許根據項目配置中定義的內容片段模型建立內容片段。

要將語言根資料夾分配給配置，請選擇該資料夾，然後選擇 **屬性** 的子菜單。

![選擇屬性](assets/overview/properties.png)

接下來，導航到 **Cloud Services** 的子菜單。 **雲配置** 的子菜單。

![雲端設定](assets/overview/cloud-conf.png)

在顯示的模式中，選擇先前建立的配置以為其分配語言根資料夾。

### 最佳做法

以下是在中建立您自己的項目時的最佳做AEM法：

* 資料夾層次結構應考慮本地化和翻譯。 換句話說，語言資料夾應嵌套在配置資料夾中，這樣可以輕鬆翻譯這些配置資料夾中的內容。
* 資料夾層次結構應保持平坦和簡單。 避免以後移動或更名資料夾和片段，尤其是在發佈以用於實際使用時，因為它會更改可能影響片段引用和GraphQL查詢的路徑。

## 入門和解決方案包

2AEM **軟體包** 可用，可通過 [包管理器](/help/headless-tutorial/graphql/advanced-graphql/author-content-fragments.md#sample-content)

* [Advanced-GraphQL-Tutorial-Starter-Package-1.0.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Starter-Package-1.0.zip) 在本教程的後面部分使用，並包含示例影像和資料夾。
* [高級 — GraphQL — 教程 — 解決方案 — 軟體包–1.1.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Solution-Package-1.1.zip) 包含第1至4章的已完成解決方案，包括新的內容片段模型、內容片段和永續GraphQL查詢。 對於想直接跳入 [客戶端應用程式整合](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md) 一章。

兩個React JS項目可用於對查詢進行實驗 [從無頭客戶端應用程式](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md)。

* [aem-guides-wknd-headless-start-tutorial.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-start-tutorial.zip)  — 在中完成的啟動客戶端應用程式 [第5章 — 客戶端應用程式整合](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md)。
* [aem-guides-wknd-headless-solution-tutorial.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-solution-tutorial.zip)  — 已完成的客戶端應用程式 **持久** 的下界。


## 入門

要開始使用本高級教程，請執行以下步驟：

1. 使用 [AEMas a Cloud Service](../quick-setup/cloud-service.md)。
1. 啟動教程章，內容為 [建立內容片段模型](/help/headless-tutorial/graphql/advanced-graphql/create-content-fragment-models.md)。
