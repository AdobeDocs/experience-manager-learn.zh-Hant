---
title: 建立內容片段模型 — 無頭的高級AEM概念 — GraphQL
description: 在Adobe Experience Manager(AEM)無頭的高級概念的本章中，通過添加頁籤佔位符、日期和時間、JSON對象、片段引用和內容引用，瞭解如何編輯內容片段模型。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: 2122ab13-f9df-4f36-9c7e-8980033c3b10
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '1989'
ht-degree: 1%

---

# 建立內容片段模型 {#create-content-fragment-models}

本章介紹建立五個內容片段模型的步驟：

* **聯絡資訊**
* **地址**
* **人員**
* **位置**
* **團隊**

內容片段模型允許定義內容類型之間的關係並將此類關係保留為模式。 使用嵌套片段引用、各種內容資料類型和可視內容組織的頁籤類型。 更高級的資料類型，如制表符佔位符、片段引用、JSON對象以及日期和時間資料類型。

本章還介紹如何增強內容引用的驗證規則。

## 必備條件 {#prerequisites}

這是高級教程。 在繼續本章之前，請確保您已完成 [快速設定](../quick-setup/cloud-service.md)。 確保您還閱讀了上一頁 [概述](../overview.md) 一章，瞭解有關高級教程的設定的詳細資訊。

## 目標 {#objectives}

* 建立內容片段模型.
* 添加制表符佔位符、日期和時間、JSON對象、片段引用和對模型的內容引用。
* 將驗證添加到內容引用。

## 內容片段模型概述 {#content-fragment-model-overview}

以下視頻簡要介紹了內容片段模型以及在本教程中如何使用這些模型。

>[!VIDEO](https://video.tv.adobe.com/v/340037?quality=12&learn=on)

## 建立內容片段模型 {#create-models}

讓我們為WKND應用建立一些內容片段模型。 如果您需要建立內容片段模型的基本介紹，請參閱 [基本教程](../multi-step/content-fragment-models.md)。

1. 導航到 **工具** > **常規** > **內容片段模型**。

   ![內容片段模型路徑](assets/define-content-fragment-models/content-fragment-models-path.png)

1. 選擇 **WKND共用** 查看網站的現有內容片段模型清單。

### 聯繫資訊模型 {#contact-info-model}

接下來，建立包含人員或地點聯繫資訊的模型。

1. 選擇 **建立** 在右上角。

1. 為模型提供「聯繫資訊」標題，然後選擇 **建立**。 在顯示的成功模式中，選擇 **開啟** 的子菜單。

1. 從拖動 **單行文本** 的子菜單。 給它 **欄位標籤** 在 **屬性** 頁籤。 屬性名稱自動填寫為 `phone`。 選中複選框以建立欄位 **必需**。

1. 導航到 **資料類型** 頁籤，然後添加另一個 **單行文本** 的子菜單。 給它 **欄位標籤** 並將其設定為 **必需**。

Adobe Experience Manager提供了一些內置的驗證方法。 這些驗證方法允許您將治理規則添加到內容片段模型中的特定欄位。 在這種情況下，讓我們添加一個驗證規則，以確保用戶在填寫此欄位時只能輸入有效的電子郵件地址。 在 **驗證類型** 下拉清單，選擇 **電子郵件**。

已完成的內容片段模型應如下所示：

![聯繫資訊模型路徑](assets/define-content-fragment-models/contact-info-model.png)

完成後，選擇 **保存** 確認更改並關閉內容片段模型編輯器。

### 地址模型 {#address-model}

接下來，為地址建立模型。

1. 從 **WKND共用**&#x200B;選中 **建立** 從右上角。

1. 輸入「地址」標題，然後選擇 **建立**。 在顯示的成功模式中，選擇 **開啟** 的子菜單。

1. 拖放 **單行文本** 在模型上給它 **欄位標籤** 街道地址。 然後，屬性名稱將填充為 `streetAddress`。 選擇 **必需** 複選框。

1. 重複上述步驟，並向模型添加四個「單行文本」欄位。 使用以下標籤：

   * 城市
   * 狀態
   * 郵遞區號
   * 國家/地區

1. 選擇 **保存** 以保存對地址模型的更改。

   已完成的「地址」片段模型應如下所示：
   ![地址模型](assets/define-content-fragment-models/address-model.png)

### 人員模型 {#person-model}

接下來，建立包含有關人員資訊的模型。

1. 在右上角，選擇 **建立**。

1. 為模型指定「人員」標題，然後選擇 **建立**。 在顯示的成功模式中，選擇 **開啟** 的子菜單。

1. 從拖動 **單行文本** 的子菜單。 給它 **欄位標籤** 全名。 屬性名稱自動填寫為 `fullName`。 選中複選框以建立欄位 **必需**。

   ![全名選項](assets/define-content-fragment-models/full-name.png)

1. 內容片段模型可以在其他模型中引用。 導航到 **資料類型** 頁籤，然後拖放 **片段引用** 的子菜單。

1. 在 **屬性** 頁籤 **允許的內容片段模型** 的子菜單。 **聯繫資訊** 早先建立的片段模型。

1. 添加 **內容引用** 並給它 **欄位標籤** 「配置檔案圖片」。 選擇下面的資料夾表徵圖 **根路徑** 的子菜單。 通過選擇 **內容** > **資產**，然後選中的複選框 **WKND共用**。 使用 **選擇** 的子菜單。 最終文本路徑應為 `/content/dam/wknd-shared`。

   ![內容引用根路徑](assets/define-content-fragment-models/content-reference-root-path.png)

1. 下 **僅接受指定的內容類型**，選擇「影像」。

   ![配置檔案圖片選項](assets/define-content-fragment-models/profile-picture.png)

1. 要限制影像檔案大小和尺寸，讓我們查看內容引用欄位的一些驗證選項。

   下 **僅接受指定的檔案大小**，選擇「小於或等於」，下面將顯示其他欄位。
   ![僅接受指定的檔案大小](assets/define-content-fragment-models/accept-specified-file-size.png)

1. 對於 **最大**，輸入&quot;5&quot;和 **選擇設備**，選擇「兆位元組(MB)」。 此驗證僅允許選擇具有指定大小的影像。

1. 下 **僅接受指定的影像寬度**，選擇「最大寬度」。 在 **最大（像素）** 欄位中，輸入「10000」。 為 **僅接受指定的影像高度**。

   這些驗證可確保添加的映像不會超過指定的值。 驗證規則現在應如下所示：

   ![內容引用驗證規則](assets/define-content-fragment-models/content-reference-validation.png)

1. 添加 **多行文本** 並給它 **欄位標籤** 《傳記》 離開 **預設類型** 下拉清單，作為預設的「Rich Text」選項。

   ![傳記選項](assets/define-content-fragment-models/biography.png)

1. 導航到 **資料類型** 頁籤，然後拖動 **枚舉** 在&quot;傳記&quot;下。 而不是預設 **呈現為** 選項，選擇 **下拉清單** 然後把它 **欄位標籤** 「教師經驗級別」。 輸入教師經驗級別選項的選擇，如 _專家、高級、中級_。

1. 接下來，拖動另一個 **枚舉** 「教師體驗級別」下的欄位，並選擇「複選框」 **呈現為** 的雙曲餘切值。 給它 **欄位標籤** &quot;技能&quot; 進入不同的技能，如攀岩、衝浪、騎腳踏車、滑雪和背包。 選項標籤和選項值應匹配如下：

   ![技能枚舉](assets/define-content-fragment-models/skills-enum.png)

1. 最後，使用 **多行文本** 的子菜單。

選擇 **保存** 確認更改並關閉內容片段模型編輯器。

### 位置模型 {#location-model}

下一個內容片段模型描述物理位置。 此模型使用制表符佔位符。 制表符佔位符通過對內容進行分類，幫助分別在模型編輯器和片段編輯器中組織資料類型。 每個佔位符都在「內容片段」編輯器中建立一個頁籤，類似於Internet瀏覽器中的頁籤。 「位置」(Location)模型應具有兩個頁籤：位置詳細資訊和位置地址。

1. 如前所述，選擇 **建立** 建立其他內容片段模型。 在「模型標題」中，輸入「位置」。 選擇 **建立** 後跟 **開啟** 的子菜單。

1. 添加 **頁籤佔位符** 欄位，並將其標籤為「位置詳細資訊」。

1. 拖放 **單行文本** 並標上&quot;名&quot; 在此欄位標籤下，添加 **多行文本** 欄位並將其標籤為「說明」。

1. 下一步，添加 **片段引用** 欄位，並將其標籤為「聯繫資訊」。 在「屬性」頁籤中，在 **允許的內容片段模型**，選擇 **資料夾表徵圖** 並選擇先前建立的「聯繫資訊」片段模型。

1. 添加 **內容引用** 欄位。 將其標籤為「位置映像」。 的 **根路徑** 應該 `/content/dam/wknd-shared.` 下 **僅接受指定的內容類型**，選擇「影像」。

1. 我們也添加 **JSON對象** 的子菜單。 由於此資料類型靈活，因此可用於顯示要包括在內容中的任何資料。 在這種情況下，JSON對象用於顯示有關天氣的資訊。 將JSON對象標籤為「Weather by Season」。 在 **屬性** 頁籤，添加 **說明** 因此，用戶可以清楚地知道應在此處輸入哪些資料：&quot;有關按季節（春、夏、秋、冬）的活動地點天氣的JSON資料。&quot;

   ![JSON對象選項](assets/define-content-fragment-models/json-object.png)

1. 要建立「位置地址」頁籤，請添加 **頁籤佔位符** 欄位，並將其標籤為「位置地址」。

1. 拖放 **片段引用** 欄位和屬性頁籤中，將其標籤為「地址」，並在 **允許的內容片段模型**，選擇 **地址** 模型。

1. 選擇 **保存** 確認更改並關閉內容片段模型編輯器。 已完成的位置模型應顯示如下：

   ![內容引用選項](assets/define-content-fragment-models/location-model.png)

### 團隊模型 {#team-model}

最後，建立描述一組人員的模型。

1. 從 **WKND共用** ，選擇 **建立** 建立另一個內容片段模型。 在「模型標題」中，輸入「團隊」。 如前所述，選擇 **建立** 後跟 **開啟** 的子菜單。

1. 添加 **多行文本** 的子菜單。 下 **欄位標籤**，輸入「說明」。

1. 添加 **日期和時間** 欄位，並將其標籤為「團隊建立日期」。 在這種情況下，保留預設 **類型** 設定為「日期」，但請注意，也可以使用「日期和時間」或「時間」。

   ![日期和時間選項](assets/define-content-fragment-models/date-and-time.png)

1. 導航到 **資料類型** 頁籤。 在「團隊建立日期」下，添加 **片段引用**。 在 **呈現為** 下拉清單，選擇「multifield」。 對於 **欄位標籤**，輸入「團隊成員」。 此欄位連結到 _人員_ 先前建立的模型。 由於資料類型是多欄位，因此可以添加多個人員片段，從而能夠建立人員團隊。

   ![片段引用選項](assets/define-content-fragment-models/fragment-reference.png)

1. 下 **允許的內容片段模型**，使用資料夾表徵圖開啟「選擇路徑」模式，然後選擇 **人員** 模型。 使用 **選擇** 按鈕。

   ![選擇人員模型](assets/define-content-fragment-models/select-person-model.png)

1. 選擇 **保存** 確認更改並關閉內容片段模型編輯器。

## 將片段引用添加到冒險模型 {#fragment-references}

與團隊模型對人員模型的片段引用的方式類似，必須從Adventure模型中引用團隊和位置模型，才能在WKND應用中顯示這些新模型。

1. 從 **WKND共用** ，選擇 **冒險** 模型，然後選擇 **編輯** 的上界。

   ![冒險編輯路徑](assets/define-content-fragment-models/adventure-edit-path.png)

1. 在表格底部，在「What to Bring」下，添加 **片段引用** 的子菜單。 輸入 **欄位標籤** 「位置」。 下 **允許的內容片段模型**，選擇 **位置** 模型。

   ![位置片段引用選項](assets/define-content-fragment-models/location-fragment-reference.png)

1. 再添加一個 **片段引用** 欄位，並將其標籤為「教師團隊」。 下 **允許的內容片段模型**，選擇 **團隊** 模型。

   ![團隊片段引用選項](assets/define-content-fragment-models/team-fragment-reference.png)

1. 添加其他 **片段引用** 欄位，並將其標籤為「管理員」。

   ![管理員片段引用選項](assets/define-content-fragment-models/administrator-fragment-reference.png)

1. 選擇 **保存** 確認更改並關閉內容片段模型編輯器。

## 最佳做法 {#best-practices}

有一些與建立內容片段模型相關的最佳做法：

* 建立映射到UX元件的模型。 例如，WKND應用具有用於冒險、文章和位置的內容片段模型。 您還可以添加標題、促銷或免責聲明。 每個示例都構成一個特定的UX元件。

* 建立盡可能少的模型。 通過限制模型數量，您可以最大限度地提高重用性並簡化內容管理。

* 根據需要將內容片段模型嵌套得越深越好，但只能根據需要嵌套。 回想一下，嵌套是使用片段引用或內容引用完成的。 考慮最多五個嵌套級別。

## 恭喜！ {#congratulations}

恭喜！您現在已添加了頁籤，使用了日期和時間和JSON對象資料類型，並瞭解了有關片段和內容引用的更多資訊。 您還添加了內容引用驗證規則。

## 後續步驟 {#next-steps}

本系列的下一章將介紹 [創作內容片段](/help/headless-tutorial/graphql/advanced-graphql/author-content-fragments.md) 從本章中建立的模型中。 瞭解如何使用本章中介紹的資料類型並建立資料夾策略，以限制可以在資產資料夾中建立的內容片段模型。

雖然本教程是可選的，但請確保在真實生產環境中發佈所有內容。 有關中的「作者」和「發佈」環境的AEM回顧，請參閱
[無AEM頭GraphQL系列](/help/headless-tutorial/graphql/video-series/author-publish-architecture.md)。
