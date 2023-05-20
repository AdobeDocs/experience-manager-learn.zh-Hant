---
title: Dynamic Media Classic主工作流和預覽資產
description: 瞭解Dynamic Media Classic的主工作流，包括三個步驟：建立（和上載）、作者（和發佈）和交付。 然後學習如何預覽Dynamic Media Classic的資產。
feature: Dynamic Media Classic
doc-type: tutorial
topics: development, authoring, configuring, architecture, publishing
audience: all
activity: use
topic: Content Management
role: User
level: Beginner
exl-id: 04aacd81-bbb2-4742-9306-f0eabc665a41
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '2703'
ht-degree: 1%

---

# Dynamic Media Classic主工作流和預覽資產 {#main-workflow}

Dynamic Media支援建立（和上載）、作者（和發佈）和交付工作流流程。 您首先上傳資產，然後對這些資產執行一些操作，例如構建映像集，最後發佈以使其生存。 「生成」步驟對於某些工作流是可選的。 例如，如果您的目標是僅進行動態調整和縮放影像，或轉換和發佈視頻流，則沒有必要的生成步驟。

![影像](assets/main-workflow/create-author-deliver.jpg)

Dynamic Media Classic解決方案中的工作流包括三個主要步驟：

1. 建立（並上傳）SourceContent
2. 作者（和發佈）資產
3. 交付資產

## 步驟1:建立（和上載）

這是工作流的開始。 在此步驟中，您收集或建立適合您使用的工作流的源內容，並將其上載到Dynamic Media Classic。 系統支援影像、視頻和字型的多種檔案類型，也支援PDF、Adobe Illustrator和Adobe InDesign。

請參閱 [支援的檔案類型](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html#supported-asset-file-formats)。

您可以以多種不同方式上載源內容：

- 直接從案頭或本地網路。 [瞭解如何](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html#upload-files-using-sps-desktop-application)。
- 從Dynamic Media ClassicFTP伺服器。 [瞭解如何](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html#upload-files-using-via-ftp)。

預設模式為「從案頭」，在該模式下，您可以瀏覽本地網路上的檔案並啟動上載。

![影像](assets/main-workflow/upload.jpg)

>[!TIP]
>
>不要手動添加資料夾。 而是從FTP運行上載，然後使用 **包括子資料夾** 選項在Dynamic Media Classic內重新建立資料夾結構。

預設情況下啟用兩個最重要的上載選項： **標籤為發佈**&#x200B;我們之前討論過， **覆蓋**。 「覆蓋」(Overwrite)表示，如果正在上載的檔案與系統中已存在的檔案同名，則新檔案將替換現有版本。 如果取消選中此選項，則可能無法上載檔案。

### 上載映像時覆蓋選項

可以為整個公司設定「覆蓋映像」選項的四種變體，這些變體經常會被誤解。 簡而言之，您設定的規則是希望更頻繁地覆蓋具有相同名稱的資產，或希望更少地發生覆蓋（在這種情況下，新映像將以「–1」或「–2」副檔名更名）。

- **在當前資料夾中覆蓋，相同的基本映像名稱/副檔名**。
此選項是最嚴格的替換規則。 它要求您將替換映像上載到與原始映像相同的資料夾，並且替換映像的檔案副檔名與原始映像的副檔名相同。 如果未滿足這些要求，則會建立重複項。

- **在當前資料夾中覆蓋，無論副檔名如何，均使用相同的基本資產名稱**。
需要將替換映像上載到與原始映像相同的資料夾，但檔案副檔名可能與原始映像不同。 例如，chair.tif代替chair.jpg。

- **任何檔案夾內若有基本資產名稱/副檔名相同者，將予以覆寫**.
要求替換影像的檔案名副檔名與原始影像相同（例如，chair.jpg必須替換chair.jpg，而不是chair.tif）。 但是，您可以將替換影像上載到與原始資料夾不同的資料夾中。 更新後的影像駐留在新資料夾中；在檔案的原始位置中找不到該檔案。

- **任何檔案夾內若有基本資產名稱相同者 (無論副檔名為何)，將予以覆寫**.
此選項是最包容的替換規則。 您可以將替換影像上載到與原始檔案不同的資料夾中，上載具有不同檔案副檔名的檔案，並替換原始檔案。 如果原始檔案位於其他資料夾中，則替換映像將駐留在上載到的新資料夾中。

瞭解有關 [覆蓋影像選項](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html#using-the-overwrite-images-option)。

雖然不需要，但在使用上述兩種方法之一上載時，您可以為該特定上載指定作業選項 — 例如，計畫定期上載、在上載時設定裁剪選項以及許多其他。 這些對於某些工作流來說很有價值，因此，如果它們適合您的工作流，則值得考慮。

瞭解有關 [作業選項](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html#upload-options)。

上載是任何工作流中的第一個必要步驟，因為Dynamic Media Classic無法處理其系統中尚未包含的任何內容。 在上載期間，系統在後台將每個上載的資產註冊到集中的Dynamic Media Classic資料庫，分配一個ID，並將其複製到儲存中。 此外，系統將影像檔案轉換為允許動態調整大小和縮放的格式，並將視頻檔案轉換為MP4 Web友好格式。

### 概念：這是當你將影像上傳到Dynamic Media Classic

將任何類型的影像上載到Dynamic Media Classic時，它將轉換為稱為金字塔TIFF或PTIFF的主影像格式。 PTIFF與分層TIFF點陣圖影像的格式類似，但檔案包含同一影像的多個大小（解析度），而不是不同的圖層。

![影像](assets/main-workflow/pyramid-p-tiff.png)

在轉換影像時，Dynamic Media Classic會拍攝影像全部大小的「快照」，將其縮放一半並保存，再縮放一半並保存，等等，直到其填充原始大小的偶數倍。 例如，一個2000像素的PTIFF在同一檔案中具有1000-、500-、250 — 和125像素大小（和更小）。 PTIFF檔案是Dynamic Media Classic稱為「主映像」的格式。

當您請求某一大小的映像時，建立PTIFF可讓Dynamic Media Classic的映像伺服器快速找到下一個較大的大小並縮小大小。 例如，如果上載2000像素影像並請求100像素影像，Dynamic Media Classic會查找125像素版本並將其縮小到100像素，而不是從2000像素縮小到100像素。 這使得手術非常快。 此外，當縮放影像時，這使縮放查看器能夠僅請求縮放的影像的拼貼，而不請求整個全解析度影像。 這就是主影像格式(PTIFF檔案)支援動態調整大小和縮放的方式。

同樣，您可以將主源視頻上傳到Dynamic Media Classic，上傳Dynamic Media Classic後，可以自動調整其大小並將其轉換為MP4 Web友好格式。

### 確定上載映像的最佳大小的經驗規則

**以您需要的最大大小上載影像。**

- 如果需要縮放，請上載最長維度中1500-2500像素範圍的高解析度影像。 請考慮您要提供多少詳細資訊、源映像的質量以及所顯示產品的大小。 例如，上載一個1000像素的小環影像，但上載一個3000像素的整個房間場景影像。
- 如果不需要縮放，則以顯示的準確大小上載它。 例如，如果您的頁面上有要放置的徽標或閃屏/橫幅影像，請完全按1:1的大小上傳這些影像，然後完全按此大小調用它們。

**在上傳到Dynamic Media Classic之前，切勿對您的影像進行取樣或吹掃。** 例如，不要對較小的影像進行採樣，以使其成為2000像素的影像。 看起來不妙。 在上載前，使映像盡可能接近完美。

**縮放沒有最小大小，但預設情況下，查看器不會縮放到100%以上。** 如果影像太小，它不會放大，或只放大很小的量，以防止它看起來不好。

**雖然影像大小沒有最小值，但我們不建議上傳巨型影像。** 大影像可以視為4000個以上像素。 上傳此大小的影像可能顯示影像中的灰塵顆粒或毛髮等潛在缺陷。 此類映像在Dynamic Media Classic伺服器上佔用的空間更多，這可能導致您超出合同儲存限制。

瞭解有關 [正在上載檔案](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html#uploading-your-files)。

## 步驟2:作者（和發佈）

建立和上載內容後，您將通過執行一個或多個子工作流從上載的資產中建立新的富媒體資產。 這包括所有不同類型的集合 — 影像、色板、旋轉和混合媒體集以及模板。 還包括視頻。 我們稍後將詳細介紹每種類型的影像集合和視頻富媒體。 但是，在幾乎所有情況下，您都可以從選擇一個或多個資產（或未選擇任何資產）和選擇要構建的資產類型開始。 例如，您可以選擇一個主映像和該映像的幾個視圖，然後選擇構建一個「映像集」，即同一產品的一個替代視圖集合。

>[!IMPORTANT]
>
>確保所有資產都標籤為要發佈。 預設情況下，所有資產都自動標籤為在上載時發佈，但您上載的內容中所有新創作的資產也需要標籤為發佈。

構建新資產後，將運行發佈作業。 您可以手動執行此操作或計畫自動運行的發佈作業。 發佈將所有內容從私有、Dynamic Media Classic領域複製到公共領域，發佈伺服器領域等式。 Dynamic Media發佈作業的產品是每個已發佈資產的唯一URL。

您發佈到的伺服器取決於內容和工作流的類型。 例如，所有影像都轉到影像伺服器，並將視頻流傳輸到FMS伺服器。 為方便起見，我們將「發佈」作為單個事件來指向單個伺服器。

發佈發佈所有標籤為發佈的內容 — 不僅是您的內容。 單個管理員通常代表每個人發佈，而不是代表運行發佈的單個用戶。 管理員可以根據需要發佈或設定將自動發佈的日常、每週甚至每10分鐘一次的定期作業。 按對您的業務有意義的計畫發佈。

>[!TIP]
>
>自動執行發佈作業並安排「完整發佈」，以在每天凌晨12:00或深夜的任何時間運行。

### 概念：瞭解Dynamic Media ClassicURL

Dynamic Media Classic工作流的最終產品是指向資產（無論是影像集還是自適應視頻集）的URL。 這些URL非常可預測，並且遵循相同的模式。 在影像的情況下，從PTIFF主影像生成每個影像。

以下是影像URL的語法，其中包含幾個示例：

![影像](assets/main-workflow/dmc-url.jpg)

在URL中，問號左側的所有內容都是指向特定影像的虛擬路徑。 問號右側的一切都是Image Server修飾符，是有關如何處理影像的說明。 當您有多個修飾符時，它們會用和符號分隔。

在第一個示例中，映像&quot;Dackg_A&quot;的虛擬路徑是 `http://sample.scene7.com/is/image/s7train/BackpackA`。 「影像伺服器」修飾符將影像調整為寬度為250像素（從wid=250），並使用Lanczos插值算法對影像進行重新採樣，該算法在影像調整時會銳化（從resMode=sharp2）。

第二個示例將所謂的「影像預設」應用到$！所示的同一Dackp_A影像中。_template300$。 表達式兩側的$符號表示正在將影像預設（一組打包的影像修飾符）應用到影像。

一旦您瞭解了Dynamic Media ClassicURL的組合方式，您就會瞭解如何以寫程式方式更改它們，以及如何將它們更深地整合到站點和後端系統中。

### 概念：瞭解快取延遲

新上載和發佈的資產可立即查看，而更新的資產可能會延遲10小時快取。 預設情況下，所有已發佈資產在到期前至少有10小時。 我們說最小值，因為每次查看影像時，它都會啟動一個直到10小時之後才會過期的時鐘，而在10小時之內沒有人查看該影像。 此10小時時段是資產的「生存時間」。 一旦該資產的快取過期，可以交付更新的版本。

這通常不是問題，除非出現錯誤，並且映像/資產與以前發佈的版本具有相同的名稱，但映像有問題。 例如，您意外上載了低解析度版本或您的藝術指導未批准該影像。 在這種情況下，您要重新調用原始映像，並使用相同的資產ID將其替換為新版本。

瞭解如何 [手動清除需要更新的URL的快取](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/dynamicmedia/invalidate-cdn-cache-dynamic-media.html?lang=zh-Hant)。

>[!TIP]
>
>為避免快取延遲問題，請始終向前工作 — 一個晚上、一天、兩週等。 及時完成QA/接受，讓內部參與方在向公眾發佈之前證明您的工作。 甚至在工作前一晚，你也可以做出更改，然後在當晚重新發佈。 到早上，10小時已過，快取將使用正確的映像進行更新。

- 瞭解有關 [建立發佈作業](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/publishing-files.html#creating-a-publish-job)。
- 瞭解有關 [發佈](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/publishing-files.html)。

## 第3步：交付

請記住，Dynamic Media Classic工作流的最終產品是指向資產的URL。 URL可能指向單個影像、影像集、旋轉集或某些其他影像集集合或視頻。 您需要獲取該URL並對其執行某些操作，例如編輯HTML，以便 `<IMG>` 標籤指向Dynamic Media Classic影像，而不是指向當前站點中的影像。

在「交付」步驟中，您必須將這些URL整合到您的網站、移動應用、電子郵件活動或要在其上顯示資產的任何其他數字觸摸點中。

將影像的Dynamic Media ClassicURL整合到網站的示例：

![影像](assets/main-workflow/example-url-1.png)

紅色的URL是唯一特定於Dynamic Media Classic的元素。

您的IT團隊或整合合作夥伴可以牽頭編寫和更改代碼，以將Dynamic Media ClassicURL整合到您的站點中。 Adobe有一個咨詢團隊，可以通過提供技術、創意或一般性指導，在這方面提供幫助。

對於更複雜的解決方案，如縮放查看器或將縮放與替代視圖結合的查看器，通常URL指向由Dynamic Media Classic托管的查看器，並且該URL中也是對資產ID的引用。

將在新彈出窗口中在查看器中開啟影像集的連結示例（以紅色表示）:

![影像](assets/main-workflow/example-url-2.png)

>[!IMPORTANT]
>
>您需要將Dynamic Media ClassicURL整合到您的網站、移動應用、電子郵件和其他數字觸點中 — Dynamic Media Classic不能為您這樣做！

## 預覽資產

您可能希望預覽您上傳或正在建立或編輯的資產，以確保客戶在查看資產時按您希望的方式顯示資產。 通過按一下任何 **預覽** 按鈕 **瀏覽/生成面板**，或者 **「檔案」(File)>「預覽」(Preview)**。 在瀏覽器窗口中，它將預覽面板中當前的任何資產，無論是影像、視頻還是像「影像集」這樣構建的資產。

### 動態大小預覽（影像預設）

可以使用 **大小** 預覽。 這將載入可用影像預設的清單。 我們稍後將討論「影像預設」，但將其視為以指定大小載入影像的「配方」，具有特定數量的銳化和影像質量。

### 縮放預覽

您還可以使用 **縮放** 選項，以根據不同包含的縮放查看器在多個預構建的縮放預設中預覽影像。

瞭解有關 [預覽資產](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/managing-assets/previewing-asset.html)。
