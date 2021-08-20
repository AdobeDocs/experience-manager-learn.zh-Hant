---
title: 在AEM Forms中使用監看資料夾
description: 在AEM Forms中設定和使用監看資料夾
feature: 輸出服務
version: 6.4,6.5
topic: 開發
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 0%

---


# 在AEM Forms中使用監看資料夾{#using-watched-folders-in-aem-forms}

管理員可以設定網路資料夾（稱為「觀看資料夾」），以便當使用者將檔案（例如PDF檔案）放入「觀看資料夾」時，系統會開始進行預先設定的工作流程、服務或指令碼操作，以處理新增的檔案。 服務執行指定操作後，會將結果檔案保存在指定的輸出資料夾中。 如需工作流程、服務和指令碼的詳細資訊。

若要進一步了解如何建立已觀看的資料夾，請[按一下這裡](https://helpx.adobe.com/experience-manager/6-4/forms/using/Creating-Configure-watched-folder.html)

「監看的資料夾」用於以批處理模式生成文檔。 使用觀看的資料夾機制，您可以為列印通道產生互動式通訊，或使用輸出服務來合併資料與範本。

本文將介紹如何利用輸出服務，通過觀看資料夾機制，將資料與範本合併。

輸出服務是AEM檔案服務的一部分。 輸出服務支援AEM Forms Designer的各種輸出格式和輸出設計功能。 輸出服務可以轉換XFA模板和XML資料，以生成各種格式的打印文檔。

要了解有關輸出服務的詳細資訊，請按一下這裡](https://helpx.adobe.com/aem-forms/6/output-service.html)。
[要在系統上設定監視資料夾，請執行以下步驟：
* [下載並解壓縮zip檔案的內容](assets/outputservicewatchedfolderkt.zip)。此zip檔案包含用於建立觀看資料夾的套件和使用觀看資料夾機制測試輸出服務的範例檔案
   * Windows系統

      * 使用套件管理器將outputservicewatchedfolder.zip匯入AEM
      * 這將在C驅動器上建立名為outputservicewatchedfolder的監視資料夾。
   * 非Windows系統
      * [開啟已觀看資料夾的組態設定](http://localhost:4502/crx/de/index.jsp#/etc/fd/watchfolder/config/outputservice)
      * 將外站服務節點的資料夾路徑屬性設定為指向適當的位置
      * 儲存您的變更
      * 上述位置將是您的觀看資料夾。

將SamplePdfFile和SampleXdpFile資料夾拖放至監看資料夾的輸入資料夾。 成功處理檔案時，結果會放置在已觀看資料夾的結果資料夾下。


>[!NOTE]
>
>如果與觀看資料夾相關聯的指令碼需要多個檔案，則需要建立資料夾並將所有必要檔案放置在資料夾中，並將資料夾拖放到觀看資料夾的輸入資料夾中。
>
>如果與觀看資料夾相關聯的指令碼只需要一個輸入檔案，您可以將該檔案直接拖放到觀看資料夾的輸入資料夾中

