---
title: Acroforms與AEM Forms
seo-title: 將最適化表單資料與Acroform合併
description: 整合Acroforms與AEM Forms的第2部分。 從Acroform建立架構。
feature: 適用性表單
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
source-git-commit: 451ca39511b52e90a44bba25c6739280f49a0aac
workflow-type: tm+mt
source-wordcount: '184'
ht-degree: 0%

---


# 從表格建立結構

下一步是從先前步驟中建立的Acroform建立架構。 提供範例應用程式，以在本教學課程中建立結構。 要建立架構，請按照以下說明操作：

1. 登入[CRXDE Lite](http://localhost:4502/crx/de)
2. 開啟至檔案`/apps/AemFormsSamples/components/createxsd/POST.jsp`
3. 將`saveLocation`更改為硬碟上的適當資料夾。 請確定您要儲存的資料夾已建立。
4. 將瀏覽器指向AEM上托管的[建立XSD](http://localhost:4502/content/DocumentServices/CreateXsd.html)頁面。
5. 拖放Acroform。
6. 檢查步驟3中指定的資料夾。 架構檔案將保存到此位置。

## 上傳Acroform

若要讓此示範在您的系統中運作，您需要在AEM Assets中建立名為`acroforms`的資料夾。 將Acroform上傳至此`acroforms`資料夾。

>[!NOTE]
>
>范常式式碼會在此資料夾中尋找表格。 合併最適化表單的已提交資料時，需要使用Acroform。
