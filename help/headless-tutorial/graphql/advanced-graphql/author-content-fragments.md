---
title: 製作內容片段 — AEM無周邊的進階概念 — GraphQL
description: 在Adobe Experience Manager(AEM)無頭的進階概念的本章中，了解如何在內容片段中使用索引標籤、日期和時間、JSON物件和片段參考。 設定資料夾原則以限制可包含的內容片段模型。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: 998d3678-7aef-4872-bd62-0e6ea3ff7999
source-git-commit: a500c88091d87e34c12d4092c71241983b166af8
workflow-type: tm+mt
source-wordcount: '2911'
ht-degree: 1%

---

# 製作內容片段

在 [上一章](/help/headless-tutorial/graphql/advanced-graphql/create-content-fragment-models.md)，您已建立五個內容片段模型：人員、團隊、位置、地址和聯繫資訊。 本章將逐步引導您根據這些模型建立內容片段的步驟。 也會探討如何建立資料夾原則，以限制可在資料夾中使用的內容片段模型。

## 必備條件 {#prerequisites}

本檔案是多部分教學課程的一部分。 請確保 [上一章](create-content-fragment-models.md) 已完成，然後再繼續處理本章。

## 目標 {#objectives}

在本章中，了解如何：

* 使用資料夾策略建立資料夾並設定限制
* 直接從內容片段編輯器建立片段參考
* 使用標籤、日期和JSON物件資料類型
* 將內容和片段參考插入多行文字編輯器
* 新增多個片段參考
* 巢狀內嵌內容片段

## 安裝範例內容 {#sample-content}

安裝AEM套件，其中包含數個資料夾和用於加速本教學課程的範例影像。

1. 下載 [Advanced-GraphQL-Tutorial-Starter-Package-1.1.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Starter-Package-1.1.zip)
1. 在AEM中，導覽至 **工具** > **部署** > **套件** 存取 **封裝管理員**.
1. 上傳並安裝上一步中下載的套件（zip檔案）。

   ![透過封裝管理程式上傳的封裝](assets/author-content-fragments/install-starter-package.png)

## 使用資料夾策略建立資料夾並設定限制

從AEM首頁，選取 **資產** > **檔案** > **WKND共用** > **英文**. 您可以在此處查看各種內容片段類別，包括歷險和貢獻者。

### 建立檔案夾 {#create-folders}

導覽至 **冒險** 檔案夾。 您可以看到已建立「團隊」和「位置」的資料夾，以儲存「團隊」和「位置」內容片段。

根據人員內容片段模型，為講師內容片段建立資料夾。

1. 從歷險頁面中，選取 **建立** > **資料夾** 在右上角。

   ![建立資料夾](assets/author-content-fragments/create-folder.png)

1. 在出現的「建立資料夾」強制回應中，在 **標題** 欄位。 在結尾記下「s」。 包含許多片段的資料夾標題必須為複數。 選擇 **建立**。

   ![建立資料夾強制回應視窗](assets/author-content-fragments/create-folder-modal.png)

   您現在已建立資料夾來儲存「探險教師」。

### 使用資料夾策略設定限制

AEM可讓您定義內容片段資料夾的權限和原則。 使用權限時，您只能授予特定使用者（作者）或作者群組對特定資料夾的存取權。 透過使用資料夾原則，您可以限制內容片段模型作者可在這些資料夾中使用的內容。 在此示例中，將資料夾限制為「人員」和「聯繫人資訊」模型。 配置資料夾策略：

1. 選取 **講師** 建立的資料夾，然後選擇 **屬性** 的上界。

   ![屬性](assets/author-content-fragments/properties.png)

1. 選取 **原則** 標籤，然後取消選取 **繼承自/content/dam/wknd-shared**. 在 **允許的內容片段模型（依路徑）** 欄位中，選擇資料夾表徵圖。

   ![資料夾圖示](assets/author-content-fragments/folder-icon.png)

1. 在開啟的「選取路徑」對話方塊中，依循路徑 **conf** > **WKND共用**. 在上一章建立的「人員內容片段模型」包含「連絡資訊內容片段模型」的參考。 必須允許「講師」資料夾中的「人員」和「聯繫人資訊」模型，才能建立「講師內容片段」。 選擇 **人員** 和 **聯繫資訊**，然後按 **選擇** 以關閉對話方塊。

   ![選擇路徑](assets/author-content-fragments/select-path.png)

1. 選擇 **儲存並關閉** 選取 **確定** 在出現的成功對話方塊中。

1. 您現在已為Auditors資料夾配置了資料夾策略。 導覽至 **講師** 資料夾和選取 **建立** > **內容片段**. 您現在可以選取的模型只有 **人員** 和 **聯繫資訊**.

   ![資料夾原則](assets/author-content-fragments/folder-policies.png)

## 為講師製作內容片段

導覽至 **講師** 檔案夾。 在此，我們將建立一個嵌套資料夾以儲存講師的聯繫資訊。

請依照 [建立資料夾](#create-folders) 建立名為「聯繫人資訊」的資料夾。 嵌套資料夾繼承父資料夾的資料夾策略。 您可以配置更具體的策略，這樣新建立的資料夾才允許使用「聯繫資訊」模型。

### 建立講師內容片段

讓我們建立四個人員，可以加入冒險指導團隊。

1. 從「講師」資料夾，根據「人員內容片段模型」建立內容片段，並為其命名為「Jacob Wester」。

   新建立的內容片段看起來類似下列：

   ![人員內容片段](assets/author-content-fragments/person-content-fragment.png)

1. 在欄位中輸入下列內容：

   * **完整名稱**:雅各·韋斯特
   * **傳記**:雅各布·韋斯特已經當了十年徒步教練，每時每刻都很喜歡！ 雅各布是探險家，有攀岩和背包的天賦。 雅各布是登山比賽的冠軍，包括灣灣巨石競賽。 雅各目前住在加州。
   * **講師體驗等級**:專家
   * **技能**:攀岩、衝浪、背包
   * **管理員詳細資訊**:雅各布·韋斯特已經協調背包冒險了三年。

1. 在 **個人資料圖片** 欄位中，將內容參考新增至影像。 瀏覽至 **WKND共用** > **英文** > **貢獻者** > **jacob_wester.jpg** 來建立影像路徑。

### 從內容片段編輯器建立片段參考 {#fragment-reference-from-editor}

AEM可讓您直接從內容片段編輯器建立片段參考。 讓我們建立Jacob的聯繫資訊。

1. 選擇 **新內容片段** 在 **聯繫資訊** 欄位。

   ![新內容片段](assets/author-content-fragments/new-content-fragment.png)

1. 「新內容片段」強制回應隨即開啟。 在「選取目標」標籤下，依照路徑 **冒險** > **講師** 並選取 **聯繫資訊** 檔案夾。 選擇 **下一個** 以繼續前往「屬性」標籤。

   ![新內容片段強制回應視窗](assets/author-content-fragments/new-content-fragment-modal.png)

1. 在「屬性」頁簽下，在 **標題** 欄位。 選擇 **建立**，然後按 **開啟** 在出現的成功對話方塊中。

   ![「屬性」頁簽](assets/author-content-fragments/properties-tab.png)

   隨即出現新欄位，可讓您編輯「連絡資訊內容片段」。

   ![連絡資訊內容片段](assets/author-content-fragments/contact-info-content-fragment.png)

1. 在欄位中輸入下列內容：

   * **電話**:209-888-0000
   * **電子郵件**:jwester@wknd.com

   完成後，請選取 **儲存**. 您現在已建立連絡資訊內容片段。

1. 要導航回教師內容片段，請選擇 **雅各·韋斯特** 編輯器的左上角。

   ![導覽回原始內容片段](assets/author-content-fragments/back-to-jacob-wester.png)

   此 **聯繫資訊** 欄位現在包含參考之「連絡人資訊」片段的路徑。 這是巢狀片段參考。 已完成的講師內容片段如下所示：

   ![Jacob Wester內容片段](assets/author-content-fragments/jacob-wester-content-fragment.png)

1. 選擇 **儲存並關閉** 來儲存內容片段。 您現在有了新的「講師內容片段」。

### 建立其他片段

依照 [上一節](#fragment-reference-from-editor) 為這些講師建立三個講師內容片段和三個聯絡資訊內容片段。 在「講師」片段中新增下列內容：

**斯塔西·羅斯韋爾斯**

| 欄位 | 值 |
| --- | --- |
| 內容片段標題 | 斯塔西·羅斯韋爾斯 |
| 完整名稱 | 斯塔西·羅斯韋爾斯 |
| 聯絡資訊 | /content/dam/wknd shared/en/adventures/auditors/contact-info/stacey-roswells-contact-info |
| 個人資料圖片 | /content/dam/wknd-shared/en/contributors/stacey-roswells.jpg |
| 傳記 | 斯塔西·羅斯威爾斯是一位傑出的攀岩者和高山探險家。 斯塔西出生於馬里蘭州巴爾的摩，是六個孩子中的老幺。 斯塔西的父親是美國海軍的中校，母親是現代舞教練。 斯泰西的家人經常帶著父親的任務搬家，在父親駐泰時照了第一張照片。 這也是史黛西學會攀岩的地方。 |
| 講師體驗等級 | 進階 |
| 技能 | 攀岩 |滑雪 |回裝 |

**庫馬爾·塞爾瓦拉伊**

| 欄位 | 值 |
| --- | --- |
| 內容片段標題 | 庫馬爾·塞爾瓦拉伊 |
| 完整名稱 | 庫馬爾·塞爾瓦拉伊 |
| 聯絡資訊 | /content/dam/wknd shared/en/adventures/auditors/contact-info/kumar-selvaraj-contact-info |
| 個人資料圖片 | /content/dam/wknd-shared/en/contributors/kumar-selvaraj.jpg |
| 傳記 | Kumar Selvaraj是一名經驗豐富的AMGA認證專業講師，其主要目標是幫助學生提高攀岩和登山技能。 |
| 講師體驗等級 | 進階 |
| 技能 | 攀岩 |回裝 |

**奧貢辛德**

| 欄位 | 值 |
| --- | --- |
| 內容片段標題 | 奧貢辛德 |
| 完整名稱 | 奧貢辛德 |
| 聯絡資訊 | /content/dam/wknd-shared/en/adventures/aiditors/contact-info/ayo-ogunseinde-contact-info |
| 個人資料圖片 | /content/dam/wknd-shared/en/contributors/ayo-ogunseinde-237739.jpg |
| 傳記 | Ayo Ogunseinde是一位住在加州中部弗雷史諾的專業登山者和背包教練。 Ayo的目標是引導徒步者進行最史詩般的國家公園冒險。 |
| 講師體驗等級 | 進階 |
| 技能 | 攀岩 |騎腳踏車 |回裝 |

保留 **其他資訊** 欄位空白。

在「連絡資訊」片段中新增下列資訊：

| 內容片段標題 | 電話 | 電子郵件 |
| ------- | -------- | -------- |
| 斯塔西·羅斯韋爾斯聯繫資訊 | 209-888-0011 | sroswells@wknd.com |
| 庫馬爾·塞爾瓦拉伊聯繫資訊 | 209-888-0002 | kselvaraj@wknd.com |
| Ayo Ogunseinde聯繫資訊 | 209-888-0304 | aogunseinde@wknd.com |

您現在可以建立團隊了！

## 為位置製作內容片段

導覽至 **位置** 檔案夾。 在此，您會看到已建立的兩個巢狀資料夾：約塞米蒂國家公園和約塞米蒂谷旅店。

![位置資料夾](assets/author-content-fragments/locations-folder.png)

暫時忽略Yosemite Valley Lodge資料夾。 在本節稍後的部分，我們將建立一個位置，作為我們的講師團隊的首頁庫。

導覽至 **約塞米蒂國家公園** 檔案夾。 目前，它只包含約塞米蒂國家公園的照片。 讓我們使用「位置內容片段模型」建立內容片段，並將其命名為「Yosemite國家公園」。

### 標籤佔位符

AEM可讓您使用標籤預留位置來分組不同類型的內容，並讓您的內容片段更容易閱讀和管理。 在上一章中，您將索引標籤預留位置新增至「位置」模型。 因此，「位置內容片段」現在有兩個索引標籤區段： **位置詳細資訊** 和 **位置地址**.

![標籤預留位置](assets/author-content-fragments/tabs.png)

此 **位置詳細資訊** 標籤包含 **名稱**, **說明**, **聯繫資訊**, **位置影像**，和 **各季天氣** 欄位，而 **位置地址** 索引標籤包含「地址內容片段」的參考。 索引標籤可清楚顯示必須填入的內容類型，讓編寫內容更易於管理。

### JSON物件資料類型

此 **各季天氣** 欄位是JSON物件資料類型，表示接受JSON格式的資料。 此資料類型有彈性，可用於您要納入內容的任何資料。

您可以將游標移至欄位右側的資訊圖示上，以查看上一章中建立的欄位說明。

![JSON物件資訊圖示](assets/author-content-fragments/json-object-info.png)

在此情況下，我們需要提供該位置的平均天氣。 輸入下列資料：

```json
{
    "summer": "81 / 89°F",
    "fall": "56 / 83°F",
    "winter": "46 / 51°F",
    "spring": "57 / 71°F"
}
```

此 **各季天氣** 欄位現在應如下所示：

![JSON 物件](assets/author-content-fragments/json-object.png)

### 新增內容

我們將剩餘的內容新增至「位置內容片段」，以便在下一章中使用GraphQL查詢資訊。

1. 在 **位置詳細資訊** 頁簽，在欄位中輸入以下資訊：

   * **名稱**:約塞米蒂國家公園
   * **說明**:約塞米蒂國家公園位於加州的內華達山脈。 這裡以美麗的瀑布、巨大的紅杉樹和El Capitan和Half Dome懸崖的標誌性景致而聞名。 徒步旅行和野營是體驗約塞米蒂的最佳方式。 許多小徑提供無盡的冒險和探索機會。

1. 從 **聯繫資訊** 欄位中，根據「連絡資訊」模型建立內容片段，並將其標題為「Yosemite國家公園連絡資訊」。 遵循上一節中概述的相同程式： [從編輯器建立片段參考](#fragment-reference-from-editor) 並在欄位中輸入以下資料：

   * **電話**:209-999-0000
   * **電子郵件**:yosemite@wknd.com

1. 從 **位置影像** 欄位，瀏覽 **冒險** > **位置** > **約塞米蒂國家公園** > **yosemite-national-park.jpeg** 來建立影像路徑。

   請記住，在上一章中，您已設定影像驗證，因此「位置」影像的尺寸必須小於2560 x 1800，且其檔案大小必須小於3 MB。

1. 新增所有資訊後， **位置詳細資訊** 標籤現在看起來如下：

   ![「位置詳細資訊」頁簽已完成](assets/author-content-fragments/location-details-tab-completed.png)

1. 導覽至 **位置地址** 標籤。 從 **地址** 欄位中，使用您在上一章中建立的「位址內容片段模型」，建立標題為「Yosemite國家公園地址」的內容片段。 依照 [從編輯器建立片段參考](#fragment-reference-from-editor) 並在欄位中輸入以下資料：

   * **街道地址**:柯里村路9010號
   * **城市**:約塞米蒂谷
   * **狀態**:CA
   * **郵遞區號**:95389
   * **國家/地區**:美國

1. 已完成 **位置地址** 約塞米蒂國家公園碎片的標籤如下所示：

   ![「位置地址」頁簽已完成](assets/author-content-fragments/location-address-tab-completed.png)

1. 選擇 **儲存並關閉**.

### 建立另一個片段

1. 導覽至 **約塞米蒂谷旅館** 檔案夾。 使用「位置內容片段模型」建立內容片段，並將其命名為「Yosemite Valley Lodge」。

1. 在 **位置詳細資訊** 頁簽，在欄位中輸入以下資訊：

   * **名稱**:約塞米蒂谷旅館
   * **說明**:Yosemite Valley Lodge酒店是集體會議和各種活動的中心，如購物、餐飲、釣魚、徒步旅行等。

1. 從 **聯繫資訊** 欄位中，根據「連絡資訊」模型建立內容片段，並將其標題為「Yosemite Valley Lodge連絡資訊」。 依照 [從編輯器建立片段參考](#fragment-reference-from-editor) 並在新內容片段的欄位中輸入下列資料：

   * **電話**:209-992-0000
   * **電子郵件**:yosemitelodge@wknd.com

   儲存新建立的內容片段。

1. 導覽回 **約塞米蒂谷旅館** 然後前往 **位置地址** 標籤。 從 **地址** 欄位中，使用您在上一章中建立的「位址內容片段模型」，建立標題為「Yosemite Valley Lodge Address」的內容片段。 依照 [從編輯器建立片段參考](#fragment-reference-from-editor) 並在欄位中輸入以下資料：

   * **街道地址**:約塞米蒂旅店路9006號
   * **城市**:約塞米蒂國家公園
   * **狀態**:CA
   * **郵遞區號**:95389
   * **國家/地區**:美國

   儲存新建立的內容片段。

1. 導覽回 **約塞米蒂谷旅館**，然後選取 **儲存並關閉**. 此 **約塞米蒂谷旅館** 資料夾現在包含三個內容片段：約塞米蒂谷旅店、約塞米蒂谷旅店聯繫資訊和約塞米蒂谷旅店地址。

   ![約塞米蒂谷旅館資料夾](assets/author-content-fragments/yosemite-valley-lodge-folder.png)

## 製作團隊內容片段

將資料夾瀏覽到 **團隊** > **約塞米蒂隊**. 您可以看到Yosemite團隊資料夾目前僅包含團隊標誌。

![約塞米蒂團隊資料夾](assets/author-content-fragments/yosemite-team-folder.png)

讓我們使用團隊內容片段模型建立內容片段，並將其命名為「Yosemite團隊」。

### 多行文字編輯器中的內容和片段參考

AEM可讓您直接將內容和片段參考新增至多行文字編輯器，並稍後使用GraphQL查詢擷取這些參考。 將內容和片段參考新增至 **說明** 欄位。

1. 首先，將下列文字新增至 **說明** 欄位：「在約塞米蒂國家公園工作的專業冒險家和徒步旅行教官團隊。」

1. 若要新增內容參考，請選取 **插入資產** 圖示。

   ![插入資產圖示](assets/author-content-fragments/insert-asset-icon.png)

1. 在出現的強制回應視窗中，選取 **team-yosemite-logo.png** 按 **選擇**.

   ![選取影像](assets/author-content-fragments/select-image.png)

   內容參考現在已新增至 **說明** 欄位。

請記住，在上一章中，您允許將片段參考新增至 **說明** 欄位。 我們在這裡加一個。

1. 選取 **插入內容片段** 圖示。

   ![插入內容片段圖示](assets/author-content-fragments/insert-content-fragment-icon.png)

1. 瀏覽至 **WKND共用** > **英文** > **冒險** > **位置** > **約塞米蒂谷旅館** > **約塞米蒂谷旅館**. Press **選擇** 來插入內容片段。

   ![插入內容片段強制回應視窗](assets/author-content-fragments/insert-content-fragment-modal.png)

   此 **說明** 欄位現在如下所示：

   ![說明欄位](assets/author-content-fragments/description-field.png)

您現在已將內容和片段參考直接新增至多行文字編輯器。

### 日期和時間資料類型

讓我們查看日期和時間資料類型。 選取 **日曆** 表徵圖 **團隊建立日期** 欄位以開啟日曆檢視。

![日期日曆檢視](assets/author-content-fragments/date-calendar-view.png)

可以使用當月兩側的向前和向後箭頭來設定過去或未來日期。 假設約塞米蒂隊成立於2016年5月24日，因此我們會設定日期。

### 新增多個片段參考

將「講師」新增至「團隊成員」片段參考。

1. 選擇 **新增** 在 **團隊成員** 欄位。

   ![添加按鈕](assets/author-content-fragments/add-button.png)

1. 在出現的新欄位中，選取資料夾圖示以開啟「選取路徑」強制回應。 瀏覽資料夾以 **WKND共用** > **英文** > **冒險** > **講師**，然後選取旁邊的核取方塊 **雅各布·韋斯特**. Press **選擇** 來儲存路徑。

   ![片段參考路徑](assets/author-content-fragments/fragment-reference-path.png)

1. 選取 **新增** 按三次。 使用新欄位將其餘的三名講師添加到團隊中。 此 **團隊成員** 欄位現在看起來像這樣：

   ![團隊成員欄位](assets/author-content-fragments/team-members-field.png)

1. 選擇 **儲存並關閉** 儲存團隊內容片段。

### 將片段參考新增至冒險內容片段

最後，將新建立的內容片段新增至冒險。

1. 導覽至 **冒險** > **約塞米蒂背包** 並開啟Yosemite回填內容片段。 在表單底部，您可以看到在上一章中建立的三個欄位： **位置**, **講師團**，和 **管理員**.

1. 在 **位置** 欄位。 位置路徑應參考您建立的Yosemite國家公園內容片段： `/content/dam/wknd-shared/en/adventures/locations/yosemite-national-park/yosemite-national-park`.

1. 在 **講師團** 欄位。 團隊路徑應參考您建立的Yosemite團隊內容片段： `/content/dam/wknd-shared/en/adventures/teams/yosemite-team/yosemite-team`. 這是巢狀片段參考。 團隊內容片段包含對參考聯繫資訊和地址模型的人員模型的引用。 因此，您將巢狀內容片段向下三個層級。

1. 在 **管理員** 欄位。 假設Jacob Wester是Yosemite背包探險的管理員。 路徑應會導向Jacob網站內容片段，如下所示： `/content/dam/wknd-shared/en/adventures/instructors/jacob-wester`.

1. 您現在已將三個片段參考新增至冒險內容片段。 欄位如下所示：

   ![探險片段參考](assets/author-content-fragments/adventure-fragment-references.png)

1. 選擇 **儲存並關閉** 來儲存您的內容。

## 恭喜！

恭喜！您現在已根據上一章中建立的進階內容片段模型，建立內容片段。 您也已建立資料夾原則，以限制可在資料夾中選取的內容片段模型。

## 後續步驟

在 [下一章](/help/headless-tutorial/graphql/advanced-graphql/explore-graphql-api.md)，您將了解如何使用GraphiQL整合開發環境(IDE)來傳送進階GraphQL查詢。 這些查詢可讓我們查看本章中建立的資料，並最終將這些查詢添加到WKND應用。
