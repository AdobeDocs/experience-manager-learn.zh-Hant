---
title: 使用AEM Forms的Acroforms
description: 將Acroforms與AEM Forms整合的第2部分。 從Acroform建立結構描述。
feature: adaptive-forms
doc-type: Tutorial
version: 6.5
badgeIntegration: label="整合" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 54
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '176'
ht-degree: 0%

---


# 從Acroform建立結構

下一步是從上一步建立的Acroform建立方案。 提供範例應用程式，以在本教學課程中建立結構。 若要建立結構描述，請遵循下列指示：

1. 登入 [CRXDE Lite](http://localhost:4502/crx/de)
2. 開啟檔案 `/apps/AemFormsSamples/components/createxsd/POST.jsp`
3. 變更 `saveLocation` 到硬碟上的適當資料夾。 確定已建立您要儲存的資料夾。
4. 將瀏覽器指向 [建立XSD](http://localhost:4502/content/DocumentServices/CreateXsd.html) 頁面託管於AEM。
5. 拖放Acroform。
6. 檢查步驟3中指定的資料夾。 結構描述檔案會儲存至此位置。

## 上傳Acroform

為了讓此示範在您的系統上運作，您將需要建立一個名為的資料夾。 `acroforms` 在AEM Assets中。 將Acroform上傳至此 `acroforms` 資料夾。

>[!NOTE]
>
>程式碼範例會在此資料夾中尋找縮寫。 合併最適化表單提交的資料需要該表單。
