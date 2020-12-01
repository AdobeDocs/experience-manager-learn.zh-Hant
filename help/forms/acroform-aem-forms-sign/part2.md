---
title: Acroforms與AEM Forms
seo-title: 將最適化表單資料與Acroform合併
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 0%

---


# 從Acroform建立結構

下一步是從先前步驟中建立的Acroform建立架構。 本教學課程提供範例應用程式來建立架構。 若要建立結構，請依照下列指示進行：

1. 登入[CRXDE Lite](http://localhost:4502/crx/de)
2. 開啟至檔案`/apps/AemFormsSamples/components/createxsd/POST.jsp`
3. 將`saveLocation`更改為硬碟上的適當資料夾。 請確定您要儲存至的檔案夾已建立。
4. 將您的瀏覽器指向AEM上代管的「建立XSD[」頁面。](http://localhost:4502/content/DocumentServices/CreateXsd.html)
5. 拖放Acroform。
6. 檢查步驟3中指定的資料夾。 架構檔案將保存到此位置。

## 上傳Acroform

若要在您的系統上使用此示範程式，您必須在AEM Assets中建立名為`acroforms`的檔案夾。 將Acroform上傳至此`acroforms`資料夾。

>[!NOTE]
>
>范常式式碼會在此資料夾中尋找Acroform。 需要Acroform才能合併最適化表單的已提交資料。
