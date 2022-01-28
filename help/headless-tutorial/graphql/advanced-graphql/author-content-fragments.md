---
title: 作者內容片段 — 無頭的高級概AEM念 — GraphQL
description: 在Adobe Experience Manager(AEM)無頭的高級概念的本章中，學習在內容片段中使用制表符、日期和時間、JSON對象和片段引用。 設定資料夾策略以限制可包括的內容片段模型。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
source-git-commit: 83e16ea87847182139982ea2378d8ff9f079c968
workflow-type: tm+mt
source-wordcount: '3015'
ht-degree: 1%

---

# 作者內容片段

在 [上一章](/help/headless-tutorial/graphql/advanced-graphql/create-content-fragment-models.md)，您建立了五個內容片段模型：人員、團隊、位置、地址和聯繫資訊。 本章將介紹根據這些模型建立內容片段的步驟。 還探討了如何建立資料夾策略以限制在資料夾中可以使用的內容片段模型。

## 必備條件 {#prerequisites}

本文檔是多部分教程的一部分。 在繼續本章之前，請確保已完成前幾章。

## 目標 {#objectives}

在本章中，瞭解如何：

* 使用資料夾策略建立資料夾和設定限制
* 直接從內容片段編輯器建立片段引用
* 使用頁籤、日期和JSON對象資料類型
* 將內容和片段引用插入多行文本編輯器
* 添加多個片段引用
* 嵌套內容片段

## 安裝示例內容 {#sample-content}

安裝包AEM含幾個資料夾和用於加速本教程的示例映像的軟體包。

1. 下載 [Advanced-GraphQL-Tutorial-Starter-Package-1.0.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Starter-Package-1.0.zip)
1. 在AEM中，導航到 **工具** > **部署** > **包** 訪問 **包管理器**。
1. 上載並安裝上一步中下載的包（zip檔案）。

   ![通過包管理器上載的包](assets/author-content-fragments/install-starter-package.png)

## 使用資料夾策略建立資料夾和設定限制

從主AEM頁選擇 **資產** > **檔案** > **WKND站點** > **英語**。 在這裡，您可以看到各種內容片段類別，包括在上一部分探索過的冒險和貢獻者 [多步GraphQL教程](../multi-step/overview.md)。

### 建立資料夾 {#create-folders}

導航到 **冒險** 的子菜單。 您可以看到，已建立「團隊」和「位置」資料夾來儲存「團隊」和「位置」內容片段。

為基於人員內容片段模型的教師內容片段建立資料夾。

1. 從「冒險」頁面中，選擇 **建立** > **資料夾** 在右上角。

   ![建立資料夾](assets/author-content-fragments/create-folder.png)

1. 在顯示的「建立資料夾」模式中，在 **標題** 的子菜單。 在末章節附註意&#39;s。 包含多個片段的資料夾的標題必須是複數。 選擇 **建立**。

   ![建立資料夾模式](assets/author-content-fragments/create-folder-modal.png)

   您現在已建立一個資料夾來儲存「冒險教師」。

### 使用資料夾策略設定限制

允AEM許您定義內容片段資料夾的權限和策略。 通過使用權限，您只能授予某些用戶（作者）或作者組對某些資料夾的訪問權限。 通過使用資料夾策略，可以限制內容片段模型作者在這些資料夾中可以使用的內容。 在本示例中，我們將資料夾限制為「人員」和「聯繫人資訊」模型。 配置資料夾策略：

1. 選擇 **指導員** 建立的資料夾，然後選擇 **屬性** 按鈕。

   ![屬性](assets/author-content-fragments/properties.png)

1. 選擇 **策略** ，然後取消選擇 **繼承自/content/dam/wknd**。 在 **允許的內容片段模型（按路徑）** 的子菜單。

   ![資料夾表徵圖](assets/author-content-fragments/folder-icon.png)

1. 在開啟的「選擇路徑」對話框中，沿路徑 **會議** > **WKND站點**。 在上一章中建立的「人員內容片段模型」包含對「聯繫資訊內容片段模型」的引用。 必須允許「教師」資料夾中的「人員」和「聯繫人資訊」模型，才能建立「教師內容」部分。 選擇 **人員** 和 **聯繫資訊**&#x200B;按 **選擇** 按鈕

   ![選擇路徑](assets/author-content-fragments/select-path.png)

1. 選擇 **保存並關閉** 選擇 **確定** 的子菜單。

1. 您現在已為「教師」資料夾配置了資料夾策略。 導航到 **指導員** 資料夾和 **建立** > **內容片段**。 現在可以選擇的模型只有 **人員** 和 **聯繫資訊**。

   ![資料夾策略](assets/author-content-fragments/folder-policies.png)

## 教師的作者內容片段

導航到 **指導員** 的子菜單。 在此處，我們建立一個嵌套資料夾來儲存教師的聯繫資訊。

按照上一節中概述的步驟 [建立資料夾](#create-folders) 建立標題為「聯繫人資訊」的資料夾。 請注意，嵌套資料夾繼承父資料夾的資料夾策略。 您可以隨意配置更具體的策略，以便新建立的資料夾僅允許使用「聯繫資訊」模型。

### 建立教師內容片段

讓我們建立四個可以添加到冒險指導團隊的人員。 重用在上次建立的參與者內容片段的影像和名稱 [多步GraphQL教程](../multi-step/author-content-fragments.md)。 雖然上一教程概述了如何建立基本內容片段，但本教程側重於更高級的功能。

1. 從「指導者」資料夾中，根據「人員內容片段模型」建立新內容片段，並為其指定「Jacob Wester」的標題。

   新建立的內容片段如下所示：

   ![人員內容片段](assets/author-content-fragments/person-content-fragment.png)

1. 在欄位中輸入以下內容：

   * **全名**:雅各布·韋斯特
   * **傳記**:雅各布·韋斯特是徒步教練已經十年了，每分每秒都很喜歡！ 他是個探險家，有攀岩和背包的天賦。 雅各布是攀岩比賽的贏家，包括海灣巨石競賽。 他目前住在加利福尼亞。
   * **教師經驗級別**:專家
   * **技能**:攀岩、衝浪、背包
   * **管理員詳細資訊**:雅各布·韋斯特已經協調背包冒險了3年。

1. 在 **配置檔案圖片** 欄位中，將內容引用添加到影像。 瀏覽到 **WKND站點** > **英語** > **撰稿人** > **雅各布·韋斯特·jpg** 來修改標籤元素的屬性。

### 從內容片段編輯器建立新片段引用 {#fragment-reference-from-editor}

允許AEM您直接從內容片段編輯器建立片段引用。 我們建立Jacob的聯繫資訊引用。

1. 選擇 **新內容片段** 下方 **聯繫資訊** 的子菜單。

   ![新內容片段](assets/author-content-fragments/new-content-fragment.png)

1. 將開啟「新建內容片段」模式。 在「選擇目標」頁籤下，沿路徑 **冒險** > **指導員** 並選中 **聯繫資訊** 的子菜單。 選擇 **下一個** 轉到「屬性」頁籤。

   ![新內容片段模式](assets/author-content-fragments/new-content-fragment-modal.png)

1. 在「屬性」頁籤下，在 **標題** 的子菜單。 選擇 **建立**&#x200B;按 **開啟** 的子菜單。

   ![「屬性」頁籤](assets/author-content-fragments/properties-tab.png)

   此時將出現新欄位，允許您編輯「聯繫資訊內容」片段。

   ![聯繫資訊內容片段](assets/author-content-fragments/contact-info-content-fragment.png)

1. 在欄位中輸入以下內容：

   * **電話**:209-888-0000
   * **電子郵件**:jwester@wknd.com

   完成後，選擇 **保存**。 您現在已建立新的「聯繫資訊」內容片段。

1. 要導航回教師內容片段，請選擇 **雅各布·韋斯特** 的下界。

   ![導航回原始內容片段](assets/author-content-fragments/back-to-jacob-wester.png)

   的 **聯繫資訊** 欄位現在包含引用的「聯繫人資訊」段的路徑。 這是嵌套片段引用。 完成的教師內容片段如下所示：

   ![雅各布·韋斯特內容片段](assets/author-content-fragments/jacob-wester-content-fragment.png)

1. 選擇 **保存並關閉** 的子菜單。 您現在有了新的教師內容片段。

### 建立其他片段

按照中概述的流程執行 [上一節](#fragment-reference-from-editor) 為這些指導員建立三個指導員內容片段和三個聯繫資訊內容片段。 在「指導者」片段中添加以下內容：

**史黛絲·羅斯威爾斯**

| 欄位 | 值 |
| --- | --- |
| 內容片段標題 | 史黛絲·羅斯威爾斯 |
| 完整名稱 | 史黛絲·羅斯威爾斯 |
| 聯絡資訊 | /content/dam/wknd/en/adventures/autintors/contact-info/stacey-roswells-contact-info |
| 配置檔案圖片 | /content/dam/wknd/en/contributors/stacey-roswells.jpg |
| 傳記 | 史黛西·羅斯威爾斯是一位非常出色的攀岩者和阿爾卑斯山探險家。 史黛絲出生在馬里蘭州的巴爾的摩，是六個孩子中最小的。 她的父親是美國海軍的一名中校，母親是現代舞指導員。 她的家人經常隨著父親的任務搬家，她在泰國駐紮時就拍了她的第一張照片。 史黛絲也是在這裡學會攀岩的。 |
| 教師經驗級別 | 進階 |
| 技能 | 攀岩 |滑雪 |回裝 |

**庫馬爾·塞爾瓦拉伊**

| 欄位 | 值 |
| --- | --- |
| 內容片段標題 | 庫馬爾·塞爾瓦拉伊 |
| 完整名稱 | 庫馬爾·塞爾瓦拉伊 |
| 聯絡資訊 | /content/dam/wknd/en/adventures/autintors/contact-info/kumar-selvaraj-contact-info |
| 配置檔案圖片 | /content/dam/wknd/en/contributors/Kumar_Selvaraj.JPG |
| 傳記 | Kumar Selvaraj是一位經驗豐富的AMGA認證專業教師，其主要目標是幫助學生提高攀岩和徒步旅行技能。 |
| 教師經驗級別 | 進階 |
| 技能 | 攀岩 |回裝 |

**奧貢森德**

| 欄位 | 值 |
| --- | --- |
| 內容片段標題 | 奧貢森德 |
| 完整名稱 | 奧貢森德 |
| 聯絡資訊 | /content/dam/wknd/en/adventures/attorints/contact-info/ayo-ogunseinde contact-info |
| 配置檔案圖片 | /content/dam/wknd/en/contributors/ayo-ogunseinde-237739.jpg |
| 傳記 | Ayo Ogunseinde是一位職業登山者和背包教練，住在加州中部的弗雷史諾。 她的目標是引導徒步者進行他們最史詩般的國家公園冒險。 |
| 教師經驗級別 | 進階 |
| 技能 | 攀岩 |循環 |回裝 |

離開 **其他資訊** 欄位為空。

在「聯繫人資訊」片段中添加以下資訊：

| 內容片段標題 | 電話 | 電子郵件 |
| ------- | -------- | -------- |
| Stacey Roswells聯繫資訊 | 209-888-0011 | sroswells@wknd.com |
| 庫馬爾·塞爾瓦拉伊聯繫資訊 | 209-888-0002 | kselvaraj@wknd.com |
| Ayo Ogunseinde聯繫資訊 | 209-888-0304 | aogunseinde@wknd.com |

您現在已準備好建立團隊！

## 位置的作者內容片段

導航到 **位置** 的子菜單。 這裡，您會看到兩個已建立的嵌套資料夾：約塞米蒂國家公園和約塞米蒂谷旅館。

![位置資料夾](assets/author-content-fragments/locations-folder.png)

暫時忽略Yosemite Valley Lodge資料夾。 我們稍後將在本節中重新介紹它，屆時我們將建立一個新位置，作為我們的教師團隊的主基地。

導航到 **約塞米蒂國家公園** 的子菜單。 目前，它只包含約塞米蒂國家公園的照片。 讓我們使用位置內容片段模型建立新內容片段，並將其命名為「Yosemite National Park」。

### 頁籤佔位符

允許AEM您使用頁籤佔位符對不同類型的內容進行分組，並使內容片段更易於讀取和管理。 在上一章中，將頁籤佔位符添加到「位置」模型。 因此，「位置內容片段」現在有兩個頁籤部分： **位置詳細資訊** 和 **位置地址**。

![頁籤佔位符](assets/author-content-fragments/tabs.png)

的 **位置詳細資訊** 頁籤包含 **名稱**。 **說明**。 **聯繫資訊**。 **位置影像**, **各季天氣** ，而 **位置地址** 頁籤包含對地址內容片段的引用。 這些頁籤可以清楚地說明必須填寫哪些類型的內容，因此創作內容更易於管理。

### JSON對象資料類型

的 **各季天氣** 欄位是JSON對象資料類型，這意味著它接受JSON格式的資料。 此資料類型靈活，可用於任何要包括在內容中的資料。

通過懸停在欄位右側的資訊表徵圖上，可以查看在上一章中建立的欄位說明。

![JSON對象資訊表徵圖](assets/author-content-fragments/json-object-info.png)

在這種情況下，我們需要提供當地的平均天氣。 輸入以下資料：

```json
{
    "summer": "81 / 89°F",
    "fall": "56 / 83°F",
    "winter": "46 / 51°F",
    "spring": "57 / 71°F"
}
```

的 **各季天氣** 欄位現在應顯示為如下：

![JSON 物件](assets/author-content-fragments/json-object.png)

### 添加內容

讓我們將其餘內容添加到位置內容片段，以便在下一章中使用GraphQL查詢資訊。

1. 在 **位置詳細資訊** 的子菜單。

   * **名稱**:約塞米蒂國家公園
   * **說明**:約塞米蒂國家公園位於加州內華達山脈。 酒店以其華麗的瀑布、巨大的紅杉樹以及El Capitan和Half Dome懸崖的標誌性景致而聞名。 徒步旅行和露營是客人體驗約塞米蒂的最佳方式。 眾多的步道提供無盡的探險和探索機會。

1. 從 **聯繫資訊** 欄位中，根據「聯繫資訊」模型建立新內容片段，並將其標題為「Yosemite National Park Contact Info」。 遵循上一節中概述的相同流程。 [從編輯器建立新片段引用](#fragment-reference-from-editor) 並在欄位中輸入以下資料：

   * **電話**:209-999-0000
   * **電子郵件**:yosemite@wknd.com

1. 從 **位置影像** 欄位，瀏覽 **冒險** > **位置** > **約塞米蒂國家公園** > **約塞米蒂國家公園.jpeg** 來修改標籤元素的屬性。

   請記住，在上一章中您配置了映像驗證，因此「位置」映像的尺寸必須小於2560 x 1800，其檔案大小必須小於3 MB。

1. 添加所有資訊後， **位置詳細資訊** 頁籤現在如下所示：

   ![「位置詳細資訊」頁籤已完成](assets/author-content-fragments/location-details-tab-completed.png)

1. 導航到 **位置地址** 頁籤。 從 **地址** 欄位中，使用您在上一章中建立的「地址內容片段模型」建立標題為「Yosemite National Park Address」的新內容片段。 按照上一節中概述的相同流程 [從編輯器建立新片段引用](#fragment-reference-from-editor) 並在欄位中輸入以下資料：

   * **街道地址**:柯里村街9010號
   * **城市**:約塞米蒂谷
   * **州**:CA
   * **郵遞區號**:95389
   * **國家/地區**:美國

1. 已完成 **位置地址** Yosemite國家公園碎片的頁籤如下所示：

   ![「位置地址」頁籤已完成](assets/author-content-fragments/location-address-tab-completed.png)

1. 選擇 **保存並關閉**。

### 建立附加片段

1. 導航到 **約塞米蒂谷旅館** 的子菜單。 使用「位置內容片段模型」建立新內容片段，並將其命名為「Yosemite Valley Lodge」。

1. 在 **位置詳細資訊** 的子菜單。

   * **名稱**:約塞米蒂谷旅館
   * **說明**:Yosemite Valley Lodge酒店是集團會議和各種活動的中心，如購物、餐飲、釣魚、遠足等。

1. 從 **聯繫資訊** 欄位中，根據「聯繫資訊」模型建立新內容片段，並將其標題為「Yosemite Valley Lodge Contact Info」。 按照上一節中概述的相同流程 [從編輯器建立新片段引用](#fragment-reference-from-editor) 並在新內容片段的欄位中輸入以下資料：

   * **電話**:209-992-0000
   * **電子郵件**:yosemitelodge@wknd.com

   保存新建立的內容片段。

1. 導航到 **約塞米蒂谷旅館** 去 **位置地址** 頁籤。 從 **地址** 欄位中，使用您在上一章中建立的「地址內容片段模型」建立標題為「Yosemite Valley Lodge Address」的新內容片段。 按照上一節中概述的相同流程 [從編輯器建立新片段引用](#fragment-reference-from-editor) 並在欄位中輸入以下資料：

   * **街道地址**:9006優勝美地旅館
   * **城市**:約塞米蒂國家公園
   * **州**:CA
   * **郵遞區號**:95389
   * **國家/地區**:美國

   保存新建立的內容片段。

1. 導航到 **約塞米蒂谷旅館**，然後選擇 **保存並關閉**。 的 **約塞米蒂谷旅館** 資料夾現在包含三個內容片段：Yosemite Valley Lodge山谷旅館、Yosemite Valley Lodge聯繫資訊和Yosemite Valley Lodge地址。

   ![約塞米蒂谷小屋資料夾](assets/author-content-fragments/yosemite-valley-lodge-folder.png)

## 編寫團隊內容片段

瀏覽資料夾到 **團隊** > **約塞米蒂隊**。 您可以看到，Yosemite Team資料夾當前僅包含團隊徽標。

![Yosemite團隊資料夾](assets/author-content-fragments/yosemite-team-folder.png)

讓我們使用團隊內容片段模型建立新內容片段，並將其命名為「Yosemite團隊」。

### 多行文本編輯器中的內容和片段引用

允AEM許您將內容和片段引用直接添加到多行文本編輯器中，並稍後使用GraphQL查詢檢索它們。 讓我們將內容和片段引用添加到 **說明** 的子菜單。

1. 首先，將以下文本添加到 **說明** 欄位：「在約塞米蒂國家公園工作的專業冒險家和徒步教練團隊。」

1. 要添加內容引用，請選擇 **插入資產** 表徵圖。

   ![插入資產表徵圖](assets/author-content-fragments/insert-asset-icon.png)

1. 在出現的模式中，選擇 **team-yosemite徽標.png** 按 **選擇**。

   ![選擇影像](assets/author-content-fragments/select-image.png)

   內容引用現已添加到 **說明** 的子菜單。

請記住，在上一章中，您允許將片段引用添加到 **說明** 的子菜單。 我們再加一個。

1. 選擇 **插入內容片段** 表徵圖。

   ![插入內容片段表徵圖](assets/author-content-fragments/insert-content-fragment-icon.png)

1. 瀏覽到 **WKND站點** > **英語** > **冒險** > **位置** > **約塞米蒂谷旅館** > **約塞米蒂谷旅館**。 按 **選擇** 的子菜單。

   ![插入內容片段模式](assets/author-content-fragments/insert-content-fragment-modal.png)

   的 **說明** 欄位現在如下所示：

   ![說明欄位](assets/author-content-fragments/description-field.png)

您現在已將內容和片段引用直接添加到多行文本編輯器中。

### 日期和時間資料類型

讓我們看看日期和時間資料類型。 選擇 **日曆** 表徵圖 **團隊建立日期** 的子菜單。

![日期日曆視圖](assets/author-content-fragments/date-calendar-view.png)

可以使用月的兩側的前箭頭和後箭頭設定過去日期或將來日期。 假設約塞米蒂隊是2016年5月24日成立的，那麼我們將為此確定日期。

### 添加多個片段引用

讓我們將「教師」(Aditons)添加到「團隊成員」(Team Members)片段引用。

1. 選擇 **添加** 的 **團隊成員** 的子菜單。

   ![「添加」按鈕](assets/author-content-fragments/add-button.png)

1. 在顯示的新欄位中，選擇資料夾表徵圖以開啟「選擇路徑」模式。 瀏覽資料夾到 **WKND站點** > **英語** > **冒險** > **指導員**，然後選擇旁邊的複選框 **雅各·韋斯特**。 按 **選擇** 的子菜單。

   ![片段引用路徑](assets/author-content-fragments/fragment-reference-path.png)

1. 選擇 **添加** 按鈕。 使用新欄位將剩餘的三名教師添加到團隊。 的 **團隊成員** 欄位現在如下所示：

   ![團隊成員欄位](assets/author-content-fragments/team-members-field.png)

1. 選擇 **保存並關閉** 的子菜單。

### 向冒險內容片段添加片段引用

最後，讓我們將新建立的內容片段添加到冒險中。

1. 導航到 **冒險** > **約塞米蒂背包** 開啟Yosemite背包內容片段。 在窗體底部，您可以看到在上一章中建立的三個欄位： **位置**。 **教師團隊**, **管理員**。

1. 在中添加片段引用 **位置** 的子菜單。 位置路徑應引用您建立的Yosemite National Park內容片段： `/content/dam/wknd/en/adventures/locations/yosemite-national-park/yosemite-national-park`。

1. 在中添加片段引用 **教師團隊** 的子菜單。 團隊路徑應引用您建立的Yosemite團隊內容片段： `/content/dam/wknd/en/adventures/teams/yosemite-team/yosemite-team`。 這是嵌套片段引用。 團隊內容片段包含引用聯繫資訊和地址模型的人員模型的引用。 因此，您將嵌套內容分段向下分為三個級別。

1. 在中添加片段引用 **管理員** 的子菜單。 假設雅各布·韋斯特是優勝美地背包探險的管理員。 路徑應指向Jacob Wester內容片段，如下所示： `/content/dam/wknd/en/adventures/instructors/jacob-wester`。

1. 您現在已將三個片段引用添加到「冒險內容片段」。 欄位如下所示：

   ![冒險片段引用](assets/author-content-fragments/adventure-fragment-references.png)

1. 選擇 **保存並關閉** 保存內容。

## 恭喜！

恭喜！ 現在，您已基於在上一章中建立的高級內容片段模型建立了內容片段。 您還建立了資料夾策略，以限制在資料夾中可以選擇的內容片段模型。

## 後續步驟

在 [下一章](/help/headless-tutorial/graphql/advanced-graphql/explore-graphql-api.md)，您將瞭解如何使用GraphiQL整合開發環境(IDE)發送高級GraphQL查詢。 這些查詢將允許我們查看本章中建立的資料，並最終將這些查詢添加到WKND應用。

雖然本教程是可選的，但請確保在真實生產環境中發佈所有內容。 有關「作者」和「發佈」環境的詳細資訊，請參閱 [無頭視頻](/help/headless-tutorial/graphql/video-series/author-publish-architecture.md)
