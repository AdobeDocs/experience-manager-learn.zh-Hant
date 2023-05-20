---
title: 影片概觀
description: Dynamic Media Classic推出了上傳時自動轉換視頻，將視頻流傳輸到案頭和移動設備，以及根據設備和頻寬為回放而優化的自適應視頻集。 瞭解有關Dynamic Media Classic視頻的更多資訊，並獲取視頻概念和術語的入門知識。 然後深入學習如何上載和編碼視頻，選擇視頻預設以便上載、添加或編輯視頻預設，在視頻查看器中預覽視頻，將視頻部署到網站和移動站點，向視頻添加字幕和章節標籤，以及向案頭和移動用戶發佈視頻查看器。
feature: Dynamic Media Classic, Video Profiles, Viewer Presets
doc-type: tutorial
topics: development, authoring, configuring, videos, video-profiles
audience: all
activity: use
topic: Content Management
role: User
level: Beginner
exl-id: dfbf316f-3922-4bc7-b3f3-2a5bbdeb7063
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '6118'
ht-degree: 0%

---

# 影片概觀 {#video-overview}

Dynamic Media Classic推出了上傳時自動轉換視頻，將視頻流傳輸到案頭和移動設備，以及根據設備和頻寬為回放而優化的自適應視頻集。 視頻最重要的一點是工作流程很簡單 — 它的設計讓任何人都可以使用它，即使他們不熟悉視頻技術。

在本教程的本節結束時，您將瞭解如何：

- 將視頻上傳並編碼（轉碼）到不同大小和格式
- 從可用的視頻預設中進行選擇，以便上載
- 添加或編輯視頻編碼預設
- 在視頻查看器中預覽視頻
- 將視頻部署到網站和移動站點
- 將字幕和章節標籤添加到視頻
- 為案頭和移動用戶定制和發佈視頻查看器

>[!NOTE]
>
>本章中的所有URL僅用於說明目的；它們不是即時連結。

## Dynamic Media Classic視頻概述

我們首先來更好地瞭解使用Dynamic Media Classic進行視頻的可能性。

### 功能和功能

Dynamic Media Classic視頻平台提供視頻解決方案的所有部分 — 視頻的上傳，轉換和管理；在視頻中添加字幕和章節標籤的能力；以及使用預設來輕鬆回放的功能。

它使發佈高質量自適應視頻以跨多個螢幕進行流式傳輸變得輕鬆，包括案頭、iOS、Android™、BlackBerry®和Windows移動設備。 自適應視頻集將以不同比特率和格式（如400 kbps、800 kbps和1000 kbps）編碼的同一視頻的版本分組。 台式電腦或移動設備檢測可用頻寬。

此外，如果案頭或移動設備上的網路狀況發生變化，則自動切換視頻質量。 此外，如果客戶在案頭上進入全屏模式，自適應視頻集會使用更好的解析度來響應，從而改善客戶的觀看體驗。 使用自適應視頻集為客戶在多個螢幕和設備上播放Dynamic Media Classic視頻提供了最佳的可回放。

### 視頻管理

處理視頻可能比處理靜止數字影像更為複雜。 通過視頻，您可以處理多種格式和標準，以及觀眾能否回放您的剪輯的不確定性。 Dynamic Media Classic讓視頻工作變得容易，它提供了許多功能強大的工具，「在引擎蓋下」，但消除了與視頻一起工作的複雜性。

Dynamic Media Classic承認並可以使用多種不同的源格式。 但是，閱讀視頻只是工作的一部分 — 您還必須將視頻轉換為Web友好格式。 Dynamic Media Classic通過允許您將視頻轉換為H.264視頻來解決此問題。

使用許多專業和發燒友工具，自行轉換視頻會變得複雜。 Dynamic Media Classic通過提供針對不同質量設定而優化的簡單預設來保持簡單。 但是，如果您想要更多自定義內容，也可以建立自己的預設。

如果您有大量視頻，您將非常感激您能夠管理所有資產以及您在Dynamic Media Classic的影像和其他媒體。 您可以通過強大的元資料支援來組織、編錄和搜索您的資產(包括視頻XMP資產)。

### 視頻播放

與轉換視頻使其易於訪問和訪問的問題類似，是在您的站點上實施和部署視頻的問題。 選擇是購買播放器還是自行構建播放器，使其相容各種設備和螢幕，然後維護播放器可能是全職工作。

同樣，Dynamic Media Classic的方法是允許您選擇符合您需要的預設和查看器。 您有許多不同的查看器選項，並且有一個包含大量可用預設的庫。

您可以輕鬆地將視頻傳輸到Web和移動設備，因為Dynamic Media Classic支援HTML5視頻，這意味著您可以針對運行各種瀏覽器的用戶以及Android™和iOS平台用戶。 流視頻允許平滑播放較長或高清的內容，而漸進HTML5視頻具有針對小螢幕優化的預設。

視頻的查看器預設可根據查看器類型進行部分配置。

與所有觀眾一樣，整合是通過每個觀眾或視頻的單個Dynamic Media ClassicURL實現的。

>[!NOTE]
>
>最好使用Dynamic Media ClassicHTML5視頻觀看器。 HTML5視頻查看器中使用的預設是強健的視頻播放器。 通過將使用HTML5和CSS設計播放元件的能力與單個播放器相結合，可以嵌入播放功能，並根據瀏覽器的功能使用自適應和漸進式流，您可以將富媒體內容擴展到案頭、平板電腦和移動用戶，並確保簡化的視頻體驗。

關於Dynamic Media Classic視頻的最後一條說明可能適用於一些客戶：並非所有公司都可能為其帳戶啟用了自動轉換、流式處理或視頻預設。 如果由於某種原因無法訪問流視頻的URL，則可能是原因。 您可以上載和發佈逐步下載的視頻，並且可以訪問所有視頻查看器。 但是，要利用完整的Dynamic Media Classic視頻功能，請與您的客戶經理或銷售經理聯繫，以啟用這些功能。

瞭解有關 [Dynamic Media Classic視頻](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/quick-start-video.html)。

## Video 101

### 基本視頻概念和術語

在開始之前，我們先討論一下您應該熟悉的一些術語，以便使用視頻。 這些概念並不是Dynamic Media Classic特有的，如果你要為一個專業網站管理視頻，你最好能就這個主題接受進一步的教育。 在本節末尾，我們推薦一些資源。

- **編碼/轉碼。** 編碼是應用視頻壓縮將原始的未壓縮視頻資料轉換為便於使用的格式的過程。 代碼轉換雖然類似，但是指從一種編碼方法轉換到另一種編碼方法。

   - 使用視頻編輯軟體建立的主視頻檔案通常太大，並且格式不正確，無法傳送到聯機目標。 它們通常被編碼用於在案頭上快速回放和編輯，但不是用於通過Web傳送。
   - 為了將數字視頻轉換為適當的格式和規格以便在不同螢幕上重放，視頻檔案被轉碼為更小、有效的檔案大小，以便最佳地傳送到網路和移動設備。

- **視頻壓縮。** 減少用於表示數字視頻影像的資料量，是空間影像壓縮和時間運動補償的組合。

   - 大多數壓縮技術都有損耗，這意味著它們會丟棄資料，以實現更小的尺寸。
   - 例如，DV視頻壓縮相對較少，允許您輕鬆編輯源素材，但是它太大，無法通過Web使用甚至放入DVD。

- **檔案格式。** 該格式是一個容器，與ZIP檔案類似，它決定了視頻檔案中檔案的組織方式，但通常不決定它們的編碼方式。

   - 源視頻的常用檔案格式包括Windows Media(WMV)、QuickTime(MOV)、Microsoft® AVI和MPEG等。 Dynamic Media Classic發佈的格式為MP4。
   - 視頻檔案通常包含多個相互關聯並同步的軌道 — 一個視頻軌道（沒有音頻）和一個或多個音頻軌道（沒有視頻）。
   - 視頻檔案格式確定這些不同的資料軌道和元資料的組織方式。

- **轉碼器.** 視頻編解碼器描述通過使用壓縮對視頻進行編碼的算法。 音頻也通過音頻編解碼器進行編碼。

   - 編解碼器將播放視頻所需的資訊量降至最低。 不儲存關於每個單獨幀的資訊，而只儲存關於一個幀和下一個幀之間的差異的資訊。
   - 由於大多數視頻從一個幀到下一個幀的變化很小，因此編解碼器允許高壓縮率，這會導致較小的檔案大小。
   - 視頻播放器根據視頻的編解碼器對視頻進行解碼，然後在螢幕上顯示一系列影像或幀。
   - 常見視頻編解碼器包括H.264、On2 VP6和H.263。

![影像](assets/video-overview/bird-video.png)

- **解析度.** 視頻的高度和寬度（以像素為單位）。

   - 源視頻的大小由相機和編輯軟體的輸出決定。 高清攝像頭可建立高解析度1920 x 1080視頻，但是要在Web上平滑地回放，您需要將其採樣（調整大小）降到較小的解析度，如1280 x 720、640 x 480或更小。
   - 解析度對播放該視頻所需的檔案大小和頻寬有直接影響。

- **顯示縱橫比。** 視頻寬度與視頻高度的比率。 當視頻的長寬比與播放器的比例不匹配時，您可能會看到「黑條」或空格。 用於顯示視頻的兩種常見長寬比是：

   - 4:3 (1.33:1). 用於幾乎所有標準定義電視廣播內容。
   - 16:9 (1.78:1). 用於幾乎所有寬屏、高清電視內容(HDTV)和電影。

- **比特率/資料速率。** 編碼以構成視頻回放單秒（千比特/秒）的資料量。

   - 通常，比特率越低，對Web來說越理想，因為下載速度越快。 但是，這也意味著由於壓縮損失，質量很低。
   - 良好的編解碼器應該兼顧低比特率和高質量。

- **幀速率（每秒幀數，或FPS）。** 每秒視頻的幀數或靜止影像數。 通常，北美電視(NTSC)以29.97 FPS播出；歐洲和亞洲電視(PAL)以25幀/秒的速度播出；和膠片（模擬和數字）通常為24(23.976)FPS。

   - 為了使事情更加混亂，還有逐行和隔行掃描的幀。 每個逐行幀包含整個影像幀，而隔行幀包含影像幀中的每一行像素。 然後快速回放這些幀，並看起來混合在一起。 膠片使用逐行掃描方法，而數字視頻通常是隔行掃描的。
   - 通常，源素材是否隔行掃描無關 — Dynamic Media Classic將保留轉換後視頻中的掃描方法。
   - 流式/遞進式傳送。 視頻流是在連續流中發送媒體，當它到達時可以播放，而逐步下載的視頻會像從伺服器下載的任何其他檔案一樣從瀏覽器中下載並快取到本地。

希望本入門有助於您瞭解使用Dynamic Media Classic視頻時涉及的各種選項。

## 視頻工作流

在Dynamic Media Classic處理視頻時，您遵循的基本工作流與處理影像類似。

![影像](assets/video-overview/video-overview-2.png)

1. 首先將視頻檔案上傳到Dynamic Media Classic。 為此，請開啟 **工具菜單** 在Dynamic Media Classic分機面板的底部，然後 **上載到Dynamic Media Classic>檔案到資料夾名**&#x200B;或 **上載到Dynamic Media Classic>資料夾到資料夾名稱**。 「資料夾名稱」是您當前正在使用副檔名瀏覽的任何資料夾。 視頻檔案可能很大，因此建議使用FTP上傳大檔案。 作為上載的一部分，請選擇一個或多個視頻預設來對視頻進行編碼。 上傳時可將視頻轉碼為MP4視頻。 有關使用和建立編碼預設的詳細資訊，請參閱下面的「視頻預設」主題。 瞭解 [上傳和編碼視頻](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/uploading-encoding-videos.html)。
2. 選擇或選擇並修改視頻查看器預設並預覽視頻。 您可以選擇預構建的查看器預設或自定義自己的預設。 如果您瞄準的是移動用戶，則無需在此處執行任何操作，因為移動平台不需要查看器或預設。 瞭解有關 [在視頻查看器中預覽視頻](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/previewing-videos-video-viewer.html) 和 [添加或編輯視頻查看器預設](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/previewing-videos-video-viewer.html#adding-or-editing-a-video-viewer-preset)。
3. 運行視頻發佈、獲取URL並整合。 視頻工作流與影像工作流的此步驟的主要區別在於，您運行了特殊的「視頻發佈」，而不是（或可能）標準的「影像服務」發佈。 案頭上的視頻查看器整合與影像查看器整合非常相似，但是對於移動設備，它更簡單 — 您只需要視頻本身的URL。

### 關於轉碼

代碼轉換在早期被定義為從一種編碼方法轉換到另一種編碼方法的過程。 在Dynamic Media Classic的情況下，將源視頻從其當前格式轉換為MP4的過程。 在您的視頻出現在案頭瀏覽器或移動設備上之前，這是必需的。

Dynamic Media Classic可以為你處理所有的轉碼，這是一個巨大的優勢。 您可以自行對視頻進行代碼轉換並上傳已轉換為MP4的檔案，但這可能是一個需要複雜軟體的複雜過程。 除非你知道自己在做什麼，否則你通常不會在第一次嘗試時獲得好結果。

Dynamic Media Classic不僅會為您轉換檔案，而且提供易於使用的預設也使轉換變得容易。 您確實不需要瞭解此過程的技術方面 — 您只需要大致瞭解您希望從系統中獲得的最終大小，並瞭解最終用戶擁有的頻寬。

雖然預構建的預設非常方便，而且能滿足大多數需求，但有時您需要一些更自定的東西。 在這種情況下，您可以建立自己的編碼預設。 在Dynamic Media Classic，編碼預設稱為視頻預設。 本章稍後將對此進行說明。

### 關於流式處理

另一個值得注意的主要特徵是視頻流，這是Dynamic Media Classic視頻平台的標準特徵。 流媒體在被傳送時由最終用戶不斷接收並呈現給最終用戶。 這是重要和可取的，原因有幾。

與逐次下載相比，流傳輸通常需要的頻寬要少，因為只傳輸了觀看視頻的部分。 Dynamic Media Classic視頻流伺服器和觀眾使用自動頻寬檢測來提供用戶網際網路連接可能的最佳流。

通過流式傳輸，視頻比使用其他方法時開始播放得更快。 它還能更有效地利用網路資源，因為只有觀看的視頻的部分會發送到客戶端。

另一種傳送方法是漸進式下載。 與流式視頻相比，漸進式下載只有一個一致優勢 — 您不需要流式伺服器來傳輸視頻。 這當然是Dynamic Media Classic的要點 — Dynamic Media Classic在平台中內置了一個流媒體伺服器，因此您不需要維護這塊專用硬體的麻煩或額外成本。

可從任何普通Web伺服器提供漸進式下載視頻。 雖然這可以方便而且可能具有成本效益，但請記住，漸進式下載的搜索和導航功能有限，用戶可以訪問和重新調整您的內容的用途。 在某些情況下，例如在嚴格的網路防火牆後回放，流傳輸會被阻止；在這些情況下，最好回滾到逐步交付。

對於愛好者或流量要求較低的網站而言，漸進式下載是一個不錯的選擇；如果他們不介意自己的內容被快取在用戶電腦上；如果他們只需要提供較短長度的視頻（不到10分鐘）;或者，如果他們的訪問者因某種原因無法接收流視頻。

如果您需要高級功能並控制視頻傳輸，和/或您需要向較大的觀眾（例如，數個100個同時觀看者）顯示視頻，需要流式傳輸視頻，跟蹤和報告使用情況或查看統計資訊，或者希望為觀眾提供最佳的互動式播放體驗。

最後，如果您擔心保護您的媒體以解決智慧財產權或權限管理問題，流式傳輸將提供更安全的視頻傳輸，因為流式傳輸時媒體不會保存到客戶端的快取中。

## 視訊預設集

上載視頻時，您可以從一個或多個預設中進行選擇，這些預設包含通過編碼將主視頻轉換為Web友好格式的設定。 視頻預設有兩種風格，即自適應視頻預設和單個編碼預設。

請參閱 [可用視頻預設](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html#video-presets-for-encoding-video-files)。

預設情況下會激活自適應視頻預設，這意味著它們可用於編碼。 如果希望使用「單編碼預設」，則管理員需要激活該預設，以便其顯示在「視頻預設」清單中。

瞭解如何 [激活或取消激活視頻預設](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/uploading-encoding-videos.html#activating-or-deactivating-video-encoding-presets)。

您可以選擇Dynamic Media Classic附帶的許多預構建預設之一，也可以建立自己的預設；但是，預設情況下不會選擇要上載的預設。 換句話說， **如果在上載時未選擇視頻預設，則視頻將不會被轉換，並且可能無法發佈**。 但是，您可以自己離線轉換視頻，然後上傳和發佈。 僅當希望Dynamic Media Classic為您進行轉換時，才需要「視頻預設」。

上載時，通過選擇 **視頻選項** 的子菜單。 然後，選擇是要為電腦、移動或平板電腦編碼。

- 電腦用於案頭。 在這裡，您通常會發現佔用更多頻寬的較大預設（如HD）。
- 移動和平板電腦為iPhone和Android™智慧手機等設備建立MP4視頻。 Mobile和Tablet的唯一區別是，Tablet預設通常具有更高的頻寬，因為這些預設是基於WiFi的使用。 移動預設已針對較慢的3G使用進行了優化。

### 在選擇預設前問自己的問題

選擇預設時，您應瞭解您的觀眾和源素材。 您對客戶有什麼瞭解？ 他們如何通過電腦顯示器或移動設備觀看視頻？

您的視頻解析度是什麼？ 如果選擇的預設大於原始預設，則可能會得到模糊/像素化視頻。 如果視頻大於預設值，則可以，但不要選擇大於源視頻的預設值。

它的長寬比是多少？ 如果在轉換後的視頻周圍看到黑條，則選擇了錯誤的長寬比。 Dynamic Media Classic無法自動檢測這些設定，因為它必須先檢查檔案，然後才能上載。

### 視頻選項細分

「視頻預設」通過指定這些設定確定視頻的編碼方式。 如果您不熟悉這些術語，請閱讀上面的「基本視頻概念和術語」主題。

![影像](assets/video-overview/video-overview-3.jpg)

- **外觀比例.** 標準4:3或寬屏16:9。
- **大小.** 這與顯示解析度相同，並以像素為單位。 這與縱橫比有關。 在16:9的比率下，視頻為432 x 240像素，而在4:3，視頻為320 x 240像素。
- **FPS.** 標準幀速率為每秒30幀、每秒25幀或每秒24幀(fps)，具體取決於視頻標準 — NTSC、PAL或Film。 此設定無關緊要，因為Dynamic Media Classic始終使用與源視頻相同的幀速率。
- **格式.** 這是MP4
- **頻寬。** 這是目標用戶的所需連接速度。 他們的網際網路連接是快還是慢？ 他們通常使用台式電腦或移動設備嗎？ 這也與解析度（大小）有關，因為視頻越大，需要的頻寬就越多。

### 確定視頻的資料速率或「比特率」

計算視頻的比特率是將視頻提供到Web時最不為人所理解的因素之一，但可能是最重要的因素，因為它直接影響用戶體驗。 如果將比特率設定得過高，則視頻質量會高，但效能會差。 網路連接速度較慢的用戶在視頻播放時會被迫等待，因為視頻會不斷暫停。 但是，如果設定得太低，質量就會受到影響。 在「視頻預設」中，Dynamic Media Classic根據目標頻寬建議一系列資料。 這是個好開始。

但是，如果你想自己弄明白，你需要一個比特率計算器。 這是視頻專業人員和發燒友常用的一種工具，用於估算資料在給定流或介質（如DVD）中的容量。

## 建立自定義視頻預設

有時，您可能會發現需要一個與內置編碼視頻預設的設定不匹配的特殊視頻預設。 如果您有特定大小的自定義視頻，例如從3D動畫軟體建立的視頻或從原始大小裁剪的視頻，則可能會發生這種情況。 或許您希望嘗試使用不同的頻寬設定來提供更高或更低的視頻質量。 無論如何，建立自定義的單編碼視頻預設。

### 視頻預設工作流

1. 視頻預設位於 **設定>應用程式設定>視頻預設**。 在這裡，您將找到公司可用的所有編碼預設的清單。

   - 每個流視頻帳戶都有幾十個預置模版，如果您建立自己的自定義預置模版，您也會在此處看到這些預設。
   - 可以使用下拉菜單按類型進行篩選。 預設分為電腦、移動和平板。
      ![影像](assets/video-overview/video-overview-4.jpg)

2. 「活動」列允許您選擇是要在上載時顯示所有預設，還是只顯示您選擇的預設。 如果您在美國，則可能想取消選中「歐洲PAL預設」；如果在英國/EMEA，則取消選中「NTSC預設」。
3. 按一下 **添加** 的子菜單。 這將開啟「添加視頻預設」面板。 此處的過程類似於建立「影像預設」。
4. 首先，給它一個 **預設名稱** 清單中。 在上例中，此預設用於螢幕捕獲教程視頻。
5. 的 **說明** 是可選的，但是它為用戶提供了描述此預設目的的工具提示。
6. 的 **編碼檔案尾碼** 附加到您在此處建立的視頻名稱的末尾。 請記住，您將擁有主視頻以及此編碼視頻，該視頻是主視頻的衍生項，並且Dynamic Media Classic沒有兩個資產可以具有相同的資產ID。
7. **播放設備** 選擇所需的視頻檔案格式（電腦、移動或平板）。 請記住，Mobile和Tablet產生的MP4格式相同。 Dynamic Media Classic只需知道預設的類別；但是，理論上的區別是，Tablet預設通常用於更快的網際網路連接，因為所有預設都支援WiFi。
8. **目標資料速率** 這是您必須自己弄明白的，不過您可以在下面的影像上看到建議的範圍。 或者，可將滑塊拖動到近似目標頻寬。 要獲得更精確的數字，請使用位速率計算器。 有一些試驗和錯誤。

   ![影像](assets/video-overview/video-overview-5.jpg)

9. 設定源檔案 **縱橫比**。 此設定直接與大小相關，如下。 如果您選擇 _自定義_，必須手動輸入寬度和高度。
10. 如果選擇縱橫比，則為 **解析度大小** ，而Dynamic Media Classic將自動填寫另一個值。 但是，對於自定義長寬比，請填寫這兩個值。 您的大小應與資料速率一致。 如果設定低資料速率和大小，您會認為質量很差。
11. 按一下 **保存** 保存預設。 與其它所有預設不同，此時您不需要發佈，因為這些預設僅用於上載檔案。 稍後，您必須發佈已編碼的視頻，但預設僅供內部Dynamic Media Classic使用。
12. 要驗證視頻預設是否在上載清單中，請轉到 **上載**。 選擇 **作業選項** 擴展 **視頻選項**。 您的預設列在您選擇的播放設備（電腦、移動或平板電腦）的類別中。

瞭解有關 [添加或編輯視頻預設](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/uploading-encoding-videos.html#adding-or-editing-a-video-encoding-preset)。

## 將字幕添加到視頻

有時，在視頻中添加字幕會很有用，例如，當您需要以多種語言向觀眾提供視頻，但是不想用另一種語言對音頻進行調音或以不同語言再次錄制視頻時。 此外，添加字幕為聽力受損者和使用隱藏字幕者提供了更方便的使用。 Dynamic Media Classic使您可以輕鬆地在視頻中添加字幕。

瞭解如何 [將字幕添加到視頻](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/adding-captions-video.html)。

## 將章節標籤添加到視頻

對於長格式視頻，您的觀眾可能會欣賞通過使用章節標籤導航視頻所提供的能力和便利。 Dynamic Media Classic提供了在視頻中輕鬆添加章節標籤的能力。

瞭解如何 [將章節標籤添加到視頻](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/adding-chapter-markers-video.html)。

## 視頻實施主題

### 發佈和複製URL

Dynamic Media Classic工作流中的最後一步是發佈視頻內容。 但是，在「高級」下找到視頻有自己的發佈工作，稱為「視頻伺服器發佈」。

![影像](assets/video-overview/video-overview-6.jpg)

瞭解如何 [發佈視頻](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#publishing-video)。

一旦運行視頻發佈，您就能獲取URL，以在Web瀏覽器中訪問您的視頻和任何現成的Dynamic Media Classic查看器預設。 但是，如果自定義或建立自己的視頻查看器預設，則需要運行單獨的Image Server發佈。

- 瞭解如何 [將URL連結到移動網站或網站](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#linking-a-video-url-to-a-mobile-site-or-a-website)。
- 瞭解如何 [將視頻查看器嵌入網頁](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#embedding-the-video-viewer-on-a-web-page)。

您還可以使用第三方或自定義視頻播放器部署視頻。

瞭解如何 [使用第三方視頻播放器部署視頻](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#deploying-video-using-a-third-party-video-player)。

此外，如果還要使用視頻縮略圖（從視頻中提取的影像），則需要運行Image Server發佈。 這是因為視頻的縮略圖影像位於影像伺服器上，而視頻本身位於視頻伺服器上。 視頻縮略圖可用於視頻搜索結果、視頻播放清單中，並可用作視頻播放前在視頻查看器中顯示的初始「海報幀」。

瞭解有關 [使用視頻縮略圖](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#working-with-video-thumbnails)。

### 選擇和自定義查看器預設

選擇和自定義查看器預設的過程與影像的過程相同。 您可以建立預設或修改現有預設並以新名稱保存，進行編輯，然後運行「影像服務」發佈。 所有查看器預設都發佈到影像伺服器，而不只是影像的預設，因此您必須運行影像發佈才能查看新的或修改的預設。

>[!TIP]
>
>在Video Server發佈後運行Image Serving發佈，以發佈與視頻關聯的任何縮略圖。

## 視頻搜索引擎優化

搜索引擎優化(SEO)是提高搜索引擎中網站或網頁可見性的過程。 雖然搜索引擎擅長收集關於基於文本的內容的資訊，但是，除非向它們提供該資訊，否則它們無法充分獲取有關視頻的資訊。 使用Dynamic Media Classic視頻SEO，您可以使用元資料為搜索引擎提供視頻的說明。 通過視頻SEO功能，您可以建立視頻小節和媒體RSS(mRSS)源。

- **視頻站點地圖**。 將視頻內容確切地告知Google網站的位置和內容。 因此，視頻在Google可以完全搜索。 例如，視頻站點地圖可以指定視頻的運行時間和類別。
- **mRSS源**。 內容發佈者用於將媒體檔案饋送到Yahoo! 視頻搜索。 Google支援視頻站點地圖和媒體RSS(mRSS)源協定，以向搜索引擎提交資訊。

建立視頻小節集和mRSS源時，您會決定要包括的視頻檔案中的元資料欄位。 這樣，您就可以將視頻描述到搜索引擎，以便搜索引擎能夠更準確地將流量引導到您網站上的視頻。

建立站點地圖或源後，您可以讓Dynamic Media Classic自動發佈它、手動發佈它，或只生成一個以後可以編輯的檔案。 此外，Dynamic Media Classic可以每天自動生成並發佈此檔案。

在流程結束時，您會將檔案或URL提交到搜索引擎。 這項任務是在Dynamic Media Classic以外完成的；然而，我們在下面簡要討論。

### 站點地圖/mRSS檔案要求

要使Google和其他搜索引擎不拒絕您的檔案，它們必須採用適當的格式並包含某些資訊。 Dynamic Media Classic生成格式正確的檔案；但是，如果某些視頻不能提供這些資訊，則檔案中不會包含這些資訊。

必填欄位為「登錄頁」（提供視頻的頁面的URL，而不是視頻本身的URL）、「標題」和「說明」。 每個視頻必須有這些項目的條目，否則它將不會包含在生成的檔案中。 可選欄位為「標籤」和「類別」。

還有兩個其它必填欄位 — 內容URL、視頻資產本身的URL和縮略圖，即視頻縮略圖的URL，但Dynamic Media Classic會自動為您填寫這些值。

建議的工作流是在使用元資料上傳之前將此資料嵌入到視頻XMP中，Dynamic Media Classic將在上傳時提取此資料。 您將使用諸如Adobe Bridge等應用程式將資料填充到標準元資料欄位中。

按照此方法，您不必使用Dynamic Media Classic手動輸入此資料。 但是，您也可以使用Dynamic Media Classic的元資料預設作為每次輸入相同資料的快速方法。

有關該主題的詳細資訊，請參見 [查看、添加和導出元資料](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/managing-assets/viewing-adding-exporting-metadata.html)。

![影像](assets/video-overview/video-overview-7.jpg)

在填充元資料後，您可以在該視頻資產的詳細資訊視圖上查看元資料。 關鍵字也可能存在，但這些關鍵字位於「關鍵字」頁籤下。

- 瞭解有關 [添加關鍵字](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/managing-assets/viewing-adding-exporting-metadata.html#add-or-edit-keywords)。
- 瞭解有關 [視頻SEO](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/video-seo-search-engine-optimization.html)。
- 瞭解 [視頻SEO的設定](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/video-seo-search-engine-optimization.html#choosing-video-seo-settings)。

#### 設定視頻SEO

設定視頻SEO首先選擇所需的格式類型、生成方法以及應將哪些元資料欄位放入檔案中。

1. 轉到 **設定>應用程式設定>視頻SEO >設定**。
2. 在 **生成模式** 的子菜單。 預設值為Off，因此要啟用它，請選擇Video Sitemap、mRSS或兩者。
3. 選擇是自動生成還是手動生成。 為簡單起見，我們建議您將其設定為 **自動模式**。 如果選擇「自動」，則還可以設定 **標籤為發佈** 選項，否則檔案將不會即時運行。 Sitemap和RSS檔案是XML文檔的類型，必須像其他任何資產一樣發佈。 如果您現在沒有準備好所有資訊，或者只想進行一次性生成，請使用其中一種手動模式。
4. 填充檔案中使用的元資料標籤。 此步驟不是可選的。 至少必須包括標有星號(\*)的三個欄位： **登錄頁** 。 **標題**, **說明**。 要將元資料用於這些任務，請將右側的「元資料」面板中的欄位拖放到窗體上的相應欄位中。 Dynamic Media Classic將自動填充佔位符欄位，以顯示每個視頻的實際資料。 您不必使用元資料欄位。 您可以在此處鍵入一些靜態文本，但每個視頻將顯示相同的文本。
5. 在三個必填欄位中輸入資訊後，Dynamic Media Classic將啟用 **保存** 和 **保存並生成** 按鈕。 按一下一保存設定。 使用 **保存** 如果您處於「自動」模式，並且希望Dynamic Media Classic稍後生成檔案。 使用 **保存並生成** 的子菜單。

### 測試和發佈視頻站點地圖、mRSS源或兩個檔案

生成的檔案將出現在帳戶的根（基）目錄中。

![影像](assets/video-overview/video-overview-8.jpg)

這些檔案必須發佈，因為視頻SEO工具無法自行運行發佈。 只要它們被標籤為要發佈，它們就會在下次運行發佈時發送到發佈伺服器。

發佈後，您的檔案可使用此URL格式使用。

![影像](assets/video-overview/video-url-format.png)

範例:

![影像](assets/video-overview/video-format-example.png)

### 提交到搜索引擎

該過程的最後一步是將檔案和/或URL提交到搜索引擎。 Dynamic Media Classic不能為你這一步，但是，假定您提交URL而不是XML檔案本身，則下次生成檔案並發佈時應更新源。

向您的搜索引擎提交的方法將有所不同，但是，對於Google，您使用Google網站管理員工具。 到達後，轉到 **站點配置>站點集** ，然後按一下 **提交站點地圖** 按鈕 您可以在此處將Dynamic Media ClassicURL放置到SEO檔案。

### 視頻SEO報告

Dynamic Media Classic提供一份報告，向您顯示哪些視頻已成功包含在檔案中，更重要的是，這些視頻由於錯誤而未包含。 要訪問報表，請轉到 **設定>應用程式設定>視頻SEO >報告**。

![影像](assets/video-overview/video-overview-9.jpg)

## MP4視頻的移動實現

Dynamic Media Classic不包括移動設備的查看器預設，因為觀眾不必在受支援的移動設備上播放視頻。 只要您編碼為H.264 MP4格式，支援的平板電腦和智慧手機就可以播放視頻而無需查看器。 Android和iOS(iPhone和iPad)設備支援此功能。

不需要查看器的原因是兩個平台都支援本機H.264。 你可以將視頻嵌入到HTML5網頁中，也可以將視頻嵌入到應用程式本身中，Android和iOS作業系統將提供一個控制器來播放視頻。

因此，Dynamic Media Classic不會為您提供移動設備查看器的URL，而是直接為視頻提供URL。 在MP4視頻的「預覽」窗口中，有「案頭」和「移動」連結。 移動URL指向已發佈的視頻。

有關已發佈視頻的一個重要注意事項是，URL列出了視頻的完整路徑，而不只是資產ID。 處理影像時，無論資料夾結構如何，都可以按其資產ID調用影像。 但是，對於視頻，還必須指定資料夾結構。 在上面的URL中，視頻儲存在路徑中：

![影像](assets/video-overview/mobile-implement-1.png)

這也可以表示為公司名稱/資料夾路徑/視頻名稱。

### 方法#1:瀏覽器播放 — HTML5代碼

要將MP4視頻嵌入到網頁中，請使用HTML5視頻標籤。

![影像](assets/video-overview/browser-playback.png)

此方法也適用於案頭Web，但是您可能會遇到瀏覽器支援問題 — 並非所有案頭Web瀏覽器本身都支援H.264視頻，包括Firefox。

### 方法#2:在iOS上播放應用程式 — 媒體播放器框架

或者，您可以將Dynamic Media ClassicMP4視頻嵌入到移動應用程式碼中。 以下是iOS使用媒體播放器框架的通用示例，該框架僅供說明之用：

![影像](assets/video-overview/app-playback.png)

## 其他資源

觀看 [Dynamic Media技能構建器：Dynamic Media Classic視頻](https://seminars.adobeconnect.com/p2ueiaswkuze) 按需網路研討會，瞭解如何在Dynamic Media Classic使用視頻功能。
