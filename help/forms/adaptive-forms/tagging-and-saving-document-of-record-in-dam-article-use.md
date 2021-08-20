---
title: 在DAM中標籤和儲存AEM Forms DoR
description: 本文將逐步說明在AEM DAM中儲存和標籤AEM Forms產生的DoR的使用案例。 文檔的標籤是根據提交的表單資料完成的。
feature: 適用性表單
version: 6.4,6.5
topic: 開發
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '616'
ht-degree: 0%

---


# 在DAM中標籤和儲存AEM Forms DoR {#tagging-and-storing-aem-forms-dor-in-dam}

本文將逐步說明在AEM DAM中儲存和標籤AEM Forms產生的DoR的使用案例。 文檔的標籤是根據提交的表單資料完成的。

客戶的常見要求是，在AEM DAM中儲存並標籤AEM Forms產生的記錄檔(DoR)。 檔案的標籤必須以最適化Forms提交的資料為基礎。 例如，如果提交資料中的雇傭狀態為「已淘汰」，我們想使用「已淘汰」標籤標籤檔案，並將檔案儲存在DAM中。

使用案例如下：

* 使用者填寫最適化表單。 在最適化表單中，會擷取使用者的婚姻狀態（前單身）和就業狀態（前退休）。
* 提交表單時，會觸發AEM工作流程。 此工作流程會以婚姻狀態（單一）和雇傭狀態（已淘汰）來標籤檔案，並將檔案儲存在DAM中。
* 將檔案儲存在DAM中後，管理員應該就能透過這些標籤來搜尋檔案。 例如，在Single或Refired上搜尋會擷取適當的DoR。

為了滿足此使用案例的要求，已編寫自定義流程步驟。 在此步驟中，我們會從提交的資料中擷取適當資料元素的值。 接著，我們使用此值來建構標籤圖磚。 例如，如果婚姻狀態元素的值為「Single」，則標籤標題會變成**Peak:EmploymentStatus/Single。 **我們會使用TagManager API找到標籤，並將標籤套用至DoR。

下列程式碼片段會示範如何尋找標籤，以及將標籤套用至檔案。

```java
Tag tagFound = tagManager.resolveByTitle(tagTitle+xmlElement.getTextContent());
//tagTitle is "Peak:EmploymentStatus/" and the xmlElement.getTextContent() will return the value Single. So the tag title becomes Peak:EmploymentStatus/Single. Once the tag is found we put the tag in array and apply the tags to the resource as shown below
tagArray[i] = tagFound;
tagManager.setTags(metadata, tagArray, true);
```

若要讓此範例在您的系統上運作，請遵循下列步驟：
* [部署Developmentwithserviceuser套件組合](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [下載並部署setvalue套件組合](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。這是自訂OSGI套件組合，會從提交的表單資料設定標籤。

* [下載範例最適化表單](assets/tag-and-store-in-dam-assets.zip)

* [前往Forms和檔案](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* 按一下建立 |檔案上傳和上傳範例adaptiveform.zip

* [使用AEM套件管](assets/tag-and-store-in-dam-assets.zip) 理器匯入文章資產
* 在預覽模式下開啟[範例表單](http://localhost:4502/content/dam/formsanddocuments/summit/peakform/jcr:content?wcmmode=disabled)。 填寫「人員」部分並提交表格。
* [導覽至DAM中的「尖峰」資料夾](http://localhost:4502/assets.html/content/dam/Peak)。您應該會在Peak資料夾中看到DoR。 檢查文檔的屬性。 應適當標籤。
恭喜!! 您已成功在系統上安裝示例

* 讓我們來探索表單提交時觸發的[工作流程](http://localhost:4502/editor.html/conf/global/settings/workflow/models/TagAndStoreDoRinDAM.html)。
* 工作流程的第一步是串連申請人名稱和居住縣，以建立唯一的檔案名稱。
* 工作流程的第二步會傳遞標籤階層以及需要標籤的表單欄位元素。 處理步驟從提交的資料中提取值，並構建需要標籤文檔的標籤標題。
* 如果要將DoR儲存在DAM中的其他資料夾中，可使用下面螢幕截圖中指定的配置屬性指定資料夾位置。

其他兩個參數則專屬於「適用性表單提交」選項中指定的「DoR」和「資料檔案路徑」。 請確定您在此處指定的值與您在適用性表單提交選項中指定的值相符。

![標籤或](assets/tag_dor_service_configuration.gif)

