---
title: 在AEM Forms使用監視資料夾
description: 配置和使用AEM Forms中的監視資料夾
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: abb74d44-d1b9-44d6-a49f-36c01acfecb4
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '423'
ht-degree: 0%

---

# 在AEM Forms使用監視資料夾{#using-watched-folders-in-aem-forms}

管理員可以配置網路資料夾（稱為監視資料夾），以便當用戶將檔案(如PDF檔案)放在監視資料夾中時，會啟動預先配置的工作流、服務或指令碼操作以處理添加的檔案。 在服務執行指定操作後，它將結果檔案保存在指定的輸出資料夾中。 有關工作流、服務和指令碼的詳細資訊。

要瞭解有關建立監視資料夾的詳細資訊， [按一下這裡](https://helpx.adobe.com/experience-manager/6-4/forms/using/Creating-Configure-watched-folder.html)

監視資料夾用於在批處理模式下生成文檔。 使用監視資料夾機制，可以為打印通道生成Interactive Communications，或使用輸出服務將資料與模板合併。

本文將介紹利用輸出服務通過監視資料夾機制將資料與模板合併的使用情形。

輸出服務是Document Services的一部分的OSGiAEM服務。 輸出服務支援AEM Forms設計器的各種輸出格式和輸出設計功能。 輸出服務可以轉換XFA模板和XML資料，以生成各種格式的打印文檔。

要瞭解有關輸出服務的詳細資訊， [請按一下這裡](https://helpx.adobe.com/aem-forms/6/output-service.html)。
要在系統上設定受監視資料夾，請執行以下步驟：
* [下載並解壓縮zip檔案的內容](assets/outputservicewatchedfolderkt.zip).此zip檔案包含用於建立監視資料夾的包和使用監視資料夾機制test輸出服務的示例檔案
   * Windows系統

      * 使用包管理器將outputservicewatchedfolder.zip導AEM入到
      * 這將在C驅動器上建立一個名為outputservicewatchedfolder的監視資料夾。
   * 非Windows系統
      * [開啟監視資料夾的配置設定](http://localhost:4502/crx/de/index.jsp#/etc/fd/watchfolder/config/outputservice)
      * 將外接服務節點的資料夾路徑屬性設定為指向適當位置
      * 保存更改
      * 上述位置是您的監視資料夾。

將SamplePdfFile和SampleXdpFile資料夾拖放到監視資料夾的輸入資料夾中。 成功處理檔案後，結果將放在監視資料夾的結果資料夾下。


>[!NOTE]
>
>如果與受監視資料夾關聯的指令碼需要多個檔案，則您需要建立一個資料夾並將所有所需檔案放在資料夾中，然後將該資料夾放到受監視資料夾的輸入資料夾中。
>
>如果與受監視資料夾關聯的指令碼只需要一個輸入檔案，則可以將該檔案直接放入受監視資料夾的輸入資料夾中
