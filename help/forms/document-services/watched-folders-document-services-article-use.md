---
title: 在AEM Forms使用監視資料夾
seo-title: 在AEM Forms使用監視資料夾
description: 在AEM Forms配置和使用監視資料夾
seo-description: 在AEM Forms配置和使用監視資料夾
uuid: 32c4bda2-363d-4294-925e-405a176f7f8d
feature: 輸出服務
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: a40e2381-0dc8-4784-9b80-15e27b244035
topic: 開發
role: 開發人員
level: 中級
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '444'
ht-degree: 0%

---


# 在AEM Forms使用監視資料夾{#using-watched-folders-in-aem-forms}

管理員可以設定網路資料夾（稱為「Watched Folder」），如此當使用者將檔案（例如PDF檔案）置於「Watched Folder」中時，就會啟動預先設定的工作流程、服務或指令碼操作，以處理新增的檔案。 服務執行指定的操作後，會將結果檔案保存到指定的輸出資料夾中。 如需工作流程、服務和指令碼的詳細資訊。

若要進一步瞭解如何建立受監視的資料夾，請按一下此處[](https://helpx.adobe.com/experience-manager/6-4/forms/using/Creating-Configure-watched-folder.html)

「監視的檔案夾」可用來以批次模式產生檔案。 使用監看資料夾機制，您可以為列印頻道產生互動式通訊，或使用輸出服務將資料與範本合併。

本文將介紹利用輸出服務通過監視資料夾機制將資料與模板合併的使用案例。

輸出服務是OSGi服務，是檔案服務的一AEM部分。 輸出服務支援AEM Forms設計師的各種輸出格式和輸出設計功能。 輸出服務可轉換XFA範本和XML資料，以產生各種格式的列印檔案。

若要進一步瞭解輸出服務，請按一下此處](https://helpx.adobe.com/aem-forms/6/output-service.html)。
[要在系統上設定監視資料夾，請執行以下步驟：
* [下載並解壓縮zip檔案的內容](assets/outputservicewatchedfolderkt.zip)。此zip檔案包含用於建立watched資料夾的套件，以及使用watched資料夾機制測試輸出服務的範例檔案
   * Windows系統

      * 使用套件管理器將outputservicewatchedfolder.zipAEM匯入
      * 這將在您的C驅動器上建立一個名為outputservicewatchedfolder的監視資料夾。
   * 非Windows系統
      * [開啟監視資料夾的配置設定](http://localhost:4502/crx/de/index.jsp#/etc/fd/watchfolder/config/outputservice)
      * 將外部服務節點的資料夾路徑屬性設定為指向適當的位置
      * 儲存變更
      * 上述位置將是您的監視資料夾。

將SamplePdfFile和SampleXdpFile資料夾拖放到監視資料夾的輸入資料夾中。 成功處理檔案後，結果會放在您受監視檔案夾的結果檔案夾下。


>[!NOTE]
>
>如果與監視資料夾關聯的指令碼需要多個檔案，則需要建立一個資料夾並將所有必需檔案放置在資料夾中，並將該資料夾放在監視資料夾的輸入資料夾中。
>
>如果與監視資料夾關聯的指令碼只需要一個輸入檔案，則可以將該檔案直接拖放到監視資料夾的輸入資料夾中

