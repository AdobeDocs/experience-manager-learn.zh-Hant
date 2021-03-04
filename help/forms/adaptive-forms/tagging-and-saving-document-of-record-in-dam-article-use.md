---
title: 在DAM中標籤和儲存AEM FormsDoR
seo-title: 在DAM中標籤和儲存AEM FormsDoR
description: 本文將介紹AEM Forms在DAM中儲存和標籤DoR的使用AEM案例。 根據提交的表單資料來標籤檔案。
seo-description: 本文將介紹AEM Forms在DAM中儲存和標籤DoR的使用AEM案例。 根據提交的表單資料來標籤檔案。
uuid: b9ba13ed-52d5-4389-a7d5-bf85e58fea49
feature: 最適化Forms，工作流程
topics: developing
audience: implementer
doc-type: article
activity: develop
version: 6.4,6.5
discoiquuid: 53961454-633b-4cd8-aef7-e64ab4e528e4
topic: 開發
role: 開發人員
level: 經驗豐富
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '661'
ht-degree: 0%

---


# 在DAM {#tagging-and-storing-aem-forms-dor-in-dam}中標籤和儲存AEM FormsDoR

本文將介紹AEM Forms在DAM中儲存和標籤DoR的使用AEM案例。 根據提交的表單資料來標籤檔案。

客戶常會要求在AEMDAM中儲存並標籤由AEM Forms產生的記錄檔案(DoR)。 檔案的標籤必須基於Forms最適化組織提交的資料。 例如，如果提交資料中的就業狀態為「已退休」，我們想用「已退休」標籤來標籤檔案，並將檔案儲存在DAM中。

使用案例如下：

* 使用者填寫最適化表單。 在適應性表單中，捕獲用戶的婚姻狀態（例如單身）和就業狀態（例如退休）。
* 提交表單時，會觸AEM發工作流程。 此工作流程會將檔案標籤為婚姻狀態（單一）和就業狀態（退休），並將檔案儲存在DAM中。
* 一旦將檔案儲存在DAM中，管理員就可以透過這些標籤來搜尋檔案。 例如，在「單一」或「已退休」上搜尋會擷取適當的DoR。

為了滿足此使用案例，編寫了定制流程步驟。 在此步驟中，我們會從提交的資料中擷取適當資料元素的值。 然後，我們使用此值來建構標籤圖格。 例如，如果婚姻狀態元素的值是「單一」，則標籤標題會變成**Peak:EmploymentStatus/Single。 **使用TagManager API，我們會尋找標籤並將標籤套用至DoR。

下列程式碼片段會示範如何尋找標籤並將標籤套用至檔案。

```java
Tag tagFound = tagManager.resolveByTitle(tagTitle+xmlElement.getTextContent());
//tagTitle is "Peak:EmploymentStatus/" and the xmlElement.getTextContent() will return the value Single. So the tag title becomes Peak:EmploymentStatus/Single. Once the tag is found we put the tag in array and apply the tags to the resource as shown below
tagArray[i] = tagFound;
tagManager.setTags(metadata, tagArray, true);
```

若要讓此範例在您的系統上運作，請遵循下列步驟：
* [部署使用服務使用者套件進行開發](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [下載並部署setvalue組合](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。這是自訂OSGI搭售，可從提交的表單資料設定標籤。

* [下載範例最適化表單](assets/tag-and-store-in-dam-assets.zip)

* [前往Forms和檔案](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* 按一下「建立」 |檔案上傳及上傳sampleadaptiveform.zip

* [使用套件管理](assets/tag-and-store-in-dam-assets.zip) 器匯入文AEM章資產
* 在預覽模式](http://localhost:4502/content/dam/formsanddocuments/summit/peakform/jcr:content?wcmmode=disabled)中開啟[範例表單。 填寫「人員」區段並提交表格。
* [導覽至DAM中的「尖峰」資料夾](http://localhost:4502/assets.html/content/dam/Peak)。您應該會在「峰值」資料夾中看到DoR。 檢查文檔的屬性。 應適當標籤。
恭喜！! 您已成功在系統上安裝示例

* 讓我們來探索[workflow](http://localhost:4502/editor.html/conf/global/settings/workflow/models/TagAndStoreDoRinDAM.html)，它會在表單提交時觸發。
* 工作流程中的第一步會串連申請人名稱和居住地郡，以建立唯一的檔案名稱。
* 工作流程的第二個步驟會傳遞標籤階層和需要標籤的表單欄位元素。 處理步驟從提交的資料中提取值，並構建需要標籤文檔的標籤標題。
* 如果您想要將DoR儲存在DAM中的其他資料夾中，您可使用下列螢幕擷取中指定的設定屬性來指定資料夾位置。

其他兩個參數是DoR和Data File Path的特定參數，如Adaptive Form submission選項中所指定。 請確定您在此處指定的值與您在「最適化表單」提交選項中指定的值相符。

![標籤多](assets/tag_dor_service_configuration.gif)

