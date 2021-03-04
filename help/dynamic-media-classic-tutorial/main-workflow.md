---
title: Dynamic Media經典主工作流程與資產預覽
description: 瞭解Dynamic Media經典版的主要工作流程，其中包括三個步驟：建立（和上傳）、作者（和發佈）和傳送。 然後瞭解如何在Dynamic Media經典中預覽資產。
sub-product: 動態媒體
feature: Dynamic Media經典
doc-type: tutorial
topics: development, authoring, configuring, architecture, publishing
audience: all
activity: use
topic: 內容管理
role: 業務從業人員
level: 初學者
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2737'
ht-degree: 0%

---


# Dynamic Media傳統主工作流和預覽資產{#main-workflow}

Dynamic Media支援建立（和上傳）、作者（和發佈）和傳送工作流程程式。 您先上傳資產，然後對這些資產執行一些動作，例如建立影像集，最後發佈以讓它們上線。 「建立」步驟對於某些工作流程是選用的。 例如，如果您的目標是只對影像進行動態調整大小和縮放，或轉換並發佈視訊以進行串流，則不需要建立步驟。

![影像](assets/main-workflow/create-author-deliver.jpg)

Dynamic Media經典解決方案的工作流程包括三個主要步驟：

1. 建立（及上傳）SourceContent
2. 製作（和發佈）資產
3. 交付資產

## 步驟1:建立（和上傳）

這是工作流程的開始。 在此步驟中，您會收集或建立符合您所使用工作流程的來源內容，並將它上傳至Dynamic Media經典。 此系統支援多種檔案類型的影像、視訊和字型，也支援PDF、Adobe Illustrator和Adobe InDesign。

請參閱[受支援檔案類型](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/upload-publish/uploading-files.html#supported-asset-file-formats)的完整清單。

您可以以幾種不同的方式上傳來源內容：

- 直接從您的案頭或本機網路。 [瞭解如何](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/upload-publish/uploading-files.html#upload-files-using-sps-desktop-application)。
- 從Dynamic Media經典FTP伺服器。 [瞭解如何](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/upload-publish/uploading-files.html#upload-files-using-via-ftp)。

預設模式為「從案頭」，您可在此瀏覽本地網路上的檔案並啟動上載。

![影像](assets/main-workflow/upload.jpg)

>[!TIP]
>
>不要手動添加資料夾。 請改為從FTP執行上傳，然後使用&#x200B;**Include Subfolders**&#x200B;選項，在Dynamic Media經典檔案中重新建立檔案夾結構。

兩個最重要的上傳選項預設會啟用： **標示為Publish**（我們先前已討論過）和&#x200B;**覆寫**。 覆寫表示，如果正在上載的檔案與系統中已有的檔案名稱相同，則新檔案將取代現有版本。 如果您取消勾選此選項，則可能無法上傳檔案。

### 上傳影像時覆寫選項

「覆寫影像」選項有四種變化，可針對您的整個公司設定，而且常常會有誤解。 簡而言之，您可以設定規則，讓想要更頻繁地覆寫同名的資產，或希望更少地發生覆寫（在這種情況下，新影像會以「-1」或「-2」副檔名重新命名）。

- **覆寫目前檔案夾中的基本影像名稱／副檔名相同**。此選項是最嚴格的替換規則。 它要求您將取代影像上傳至與原始影像相同的檔案夾，而取代影像的副檔名與原始影像的副檔名相同。 如果未滿足這些要求，則會建立重複項。

- **覆寫目前資料夾中的基本資產名稱，不論副檔名為何**。需要將取代影像上傳至與原始影像相同的檔案夾，但副檔名可能與原始影像不同。 例如，chair.tif會取代chair.jpg。

- **在任何資料夾中覆寫相同的基本資產名稱／副檔名**。要求取代影像的副檔名與原始影像相同（例如，chair.jpg必須取代chair.jpg，而非chair.tif）。 不過，您可以將取代影像上傳至原始檔案夾以外的其他檔案夾。 更新後的影像位於新資料夾中；在檔案的原始位置中無法再找到該檔案。

- **在任何資料夾中覆寫相同的基本資產名稱，不論副檔名為何**。此選項是最包含的取代規則。 您可以將取代影像上傳至原始檔案夾以外的其他檔案夾、以不同副檔名上傳檔案，並取代原始檔案。 如果原始檔案位於不同的檔案夾中，則取代影像會位於上傳檔案的新檔案夾中。

進一步瞭解[覆寫影像選項](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/setup/application-setup.html#using-the-overwrite-images-option)。

雖然不需要，但使用上述兩種方法之一上傳時，您可以指定該特定上傳的「工作選項」— 例如，若要排程循環上傳、在上傳時設定裁切選項，以及其他許多選項。 這些功能對於某些工作流程而言很有價值，因此，如果適合您的工作流程，就值得考慮。

進一步瞭解[工作選項](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/upload-publish/uploading-files.html#upload-options)。

上傳是任何工作流程中的第一個必要步驟，因為Dynamic Media經典無法處理任何尚未在其系統中的內容。 在上傳期間的幕後，系統會將每個上傳的資產註冊到集中的Dynamic Media經典資料庫，指派ID並將其複製至儲存。 此外，系統會將影像檔轉換為可動態調整大小和縮放的格式，並將視訊檔轉換為MP4適用於網頁的格式。

### 概念：以下是您將影像上傳至Dynamic Media經典影像時的情況

當您將任何類型的影像上傳至Dynamic Media經典影像時，它會轉換為稱為金字塔TIFF或P-TIFF的主影像格式。 P-TIFF與圖層TIFF點陣圖影像的格式類似，但檔案包含相同影像的多種大小（解析度），而不是不同的圖層。

![影像](assets/main-workflow/pyramid-p-tiff.png)

在轉換影像時，Dynamic Media·Classic會拍攝影像完整大小的「快照」，將影像縮放一半並儲存，再縮放一半並儲存，等等，直到填滿原始大小的倍數。 例如，2000像素的P-TIFF在相同檔案中會有1000、500、250和125像素大小（及更小）。 P-TIFF檔案是Dynamic Media經典影像中的「主影像」格式。

當您要求特定大小的影像時，建立P-TIFF可讓Image Server forDynamic Media經典影像伺服器快速找到下一個較大的影像，並縮小它。 例如，如果您上傳2000像素的影像並要求100像素的影像，Dynamic Media經典會尋找125像素版本，並將它縮小至100像素，而非從2000像素縮放至100像素。 這使得操作非常快。 此外，在放大影像時，這可讓縮放檢視器僅要求縮放的影像的拼貼，而非整個完整解析度影像。 主影像格式P-TIFF檔案就是這樣支援動態調整大小和縮放的。

同樣地，您也可以將主要來源視訊上傳至Dynamic Media經典，上傳Dynamic Media經典可自動調整其大小，並將它轉換為MP4適用於網路的格式。

### 判斷您上傳影像最佳大小的經驗法則

**以您所需的最大尺寸上傳影像。**

- 如果您需要縮放，請上傳最長維度中1500-2500像素範圍的高解析度影像。 請考慮您要提供多少細節、來源影像的品質，以及所顯示產品的大小。 例如，上傳1000像素的影像以取得小環，而上傳3000像素的影像則取得整個房間的效果。
- 如果您不需要縮放，則上傳大小會完全相同。 例如，如果您的頁面上要放置標誌或啟動顯示／橫幅影像，請完全以1:1大小上傳這些影像，然後完全以該大小呼叫這些影像。

**上傳至Dynamic Media經典之前，切勿對影像進行取樣或爆破。** 例如，不要對小影像進行上樣，使其變成2000像素的影像。看起來不妙。 在上傳前，讓您的影像盡可能接近完美。

**縮放沒有最小大小，但依預設，檢視器不會放大超過100%。** 如果您的影像太小，它不會縮放，或只會縮放很小的量，以防止它看起來不好。

**雖然影像大小沒有最低限制，但我們不建議上傳巨型影像。** 巨型影像可以視為4000像素以上。上傳此大小的影像可能會顯示影像中的灰塵粒子或毛髮等潛在瑕疵。 此類影像在Dynamic MediaClassic伺服器上也會佔用更多空間，這可能會讓您超過合約的儲存空間限制。

進一步瞭解[上傳檔案](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/upload-publish/uploading-files.html#uploading-your-files)。

## 步驟2:作者（和發佈）

在建立和上傳您的內容後，您將會執行一或多個子工作流程，從上傳的資產製作新的多媒體資產。 這包括所有不同類型的集合集合— 影像、色票、回轉和混合媒體集，以及範本。 它還包含視訊。 我們稍後將詳細說明每種影像收集集和視訊多媒體類型。 不過，在幾乎所有情況下，您都可以先選取一或多個資產（或未選取任何資產），然後選擇您要建立的資產類型。 例如，您可以選取主影像和該影像的幾個檢視，然後選擇建立影像集，這是相同產品的替代檢視集合。

>[!IMPORTANT]
>
>請確定您的所有資產都已標示為發佈。 雖然預設會自動將所有資產標示為上傳時發佈，但您上傳內容中任何新製作的資產也需要標示為發佈。

建立新資產後，您將執行發佈工作。 您可以手動執行此動作，或排程自動執行的發佈工作。 發佈會將所有內容從私人的Dynamic Media經典球體複製到公共的，發佈伺服器球體。 「Dynamic Media發佈」工作的產品是每個已發佈資產的唯一URL。

您發佈至的伺服器取決於內容和工作流程的類型。 例如，所有影像都會移至影像伺服器，並串流視訊至FMS伺服器。 為方便起見，我們會將「發佈」稱為單一事件，傳至單一伺服器。

發佈發佈所有標示為發佈的內容— 不只是您的內容。 單一管理員通常代表每個人發佈內容，而非執行發佈的個別使用者。 管理員可視需要發佈，或設定每日、每週或甚至每10分鐘自動發佈的循環工作。 以符合您企業意義的排程發佈。

>[!TIP]
>
>將您的發佈工作自動化，並排程「完整發佈」，以便每天在上午12:00或晚上任何時間晚上執行。

### 概念：瞭解Dynamic Media經典URL

Dynamic Media經典工作流程的最終產品是指向資產的URL（不論是影像集或最適化視訊集）。 這些URL可以很容易預測，而且遵循相同的模式。 對於影像，每個影像都由P-TIFF主影像生成。

以下是影像URL的語法，其中包含幾個範例：

![影像](assets/main-workflow/dmc-url.jpg)

在URL中，問號左側的一切都是特定影像的虛擬路徑。 問號右側的一切都是影像伺服器修飾元，說明如何處理影像。 當您有多個修飾元時，它們會以&amp;符號分隔。

在第一個示例中，到映像&quot;Backd_A&quot;的虛擬路徑是`http://sample.scene7.com/is/image/s7train/BackpackA`。 「影像伺服器」修飾元會將影像調整為250像素的寬度（從wid=250），並使用Lanczos插值演算法重新調整影像，此演算法會隨著影像調整大小（從resMode=sharp2）而銳利化。

第二個範例將稱為「影像預設集」的項目套用至相同的Backk_A影像，如$!_template300$。 運算式兩側的$符號表示影像預設集（一組封裝的影像修飾元）正套用至影像。

一旦您瞭解Dynamic Media經典URL的組合方式，您就瞭解如何以程式設計方式變更URL，以及如何將它們更深入地整合在網站和後端系統中。

### 概念：瞭解快取延遲

新上傳和發佈的資產會立即顯示，而更新的資產則可能會受到10小時快取延遲的影響。 依預設，所有發佈的資產在到期前至少有10小時。 我們說最小值，因為每次檢視影像時，都會啟動一個時鐘，直到10小時過去，沒有人檢視過該影像。 此10小時時段是資產的「上線時間」。 一旦該資產的快取過期，就可傳送更新的版本。

除非發生錯誤，且影像／資產與先前發佈的版本名稱相同，否則通常不會發生此問題，但影像有問題。 例如，您意外上傳了低解析度版本，或您的藝術總監未核准影像。 在這種情況下，您會想要重新叫出原始影像，並使用相同的資產ID以新版本取代它。

瞭解如何[手動清除需要更新的URL的快取](https://docs.adobe.com/content/help/en/experience-manager-65/assets/dynamic/invalidate-cdn-cached-content.html)。

>[!TIP]
>
>為避免快取延遲的問題，請隨時前進— 一個晚上，一天，兩週，等等。 及時提供QA/接受，讓內部各方在發佈給大眾之前，先證明您的作品。 即使在前一晚工作，也能讓您進行變更並在當天晚上重新發佈。 到早上，10小時已過，快取會以正確的影像更新。

- 進一步瞭解[建立發佈工作](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/upload-publish/publishing-files.html#creating-a-publish-job)。
- 進一步瞭解[Publishing](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/upload-publish/publishing-files.html)。

## 步驟3:提供

請記住，Dynamic Media經典工作流程的最終產品是指向資產的URL。 URL可能指向個別影像、影像集、回轉集或其他影像集系列或視訊。 您需要擷取該URL並加以處理，例如編輯您的HTML，讓`<IMG>`標籤指向Dynamic Media經典影像，而不是指向來自您目前網站的影像。

在「傳送」步驟中，您必須將這些URL整合到您的網站、行動應用程式、電子郵件促銷活動或您想要顯示資產的任何其他數位觸點。

將影像的Dynamic Media經典URL整合至網站的範例：

![影像](assets/main-workflow/example-url-1.png)

紅色的URL是Dynamic Media經典的唯一特定元素。

您的IT團隊或整合合作夥伴可以率先編寫和變更程式碼，將Dynamic Media經典URL整合至您的網站。 Adobe有一個顧問團隊，可協助您完成此項工作，提供技術、創意或一般指引。

對於更複雜的解決方案，例如縮放檢視器，或結合縮放與替代檢視的檢視器，URL通常會指向由Dynamic Media經典代管的檢視器，而且該URL中也是資產ID的參考。

在新快顯視窗中開啟檢視器中影像集的連結（以紅色表示）範例：

![影像](assets/main-workflow/example-url-2.png)

>[!IMPORTANT]
>
>您需要將Dynamic Media經典URL整合至您的網站、行動應用程式、電子郵件和其他數位觸點— Dynamic Media經典不能為您這麼做！

## 預覽資產

您可能會想要預覽您已上傳或正在建立或編輯的資產，以確保客戶在檢視資產時，資產會依您的需求顯示。 您可以按一下資產縮圖上的&#x200B;**瀏覽／建置面板**&#x200B;上方的任何&#x200B;**預覽**&#x200B;按鈕，或前往&#x200B;**檔案>預覽**，以存取「預覽」視窗。 在瀏覽器視窗中，它會預覽目前在面板中的資產，不論是影像、視訊或建立的資產（如影像集）。

### 動態大小預覽（影像預設集）

您可以使用&#x200B;**Sizes**&#x200B;預覽功能，預覽多種尺寸的影像。 這會載入可用影像預設集的清單。 我們稍後將討論影像預設集，但會將它們視為「方式」，以指定大小載入影像，並具有特定銳利化和影像品質。

### 縮放預覽

您也可以使用&#x200B;**縮放**&#x200B;選項，在許多預建的縮放預設集中預覽影像，這些預設集是以不同包含的縮放檢視器為基礎。

進一步瞭解[預覽資產](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/managing-assets/previewing-asset.html)。
