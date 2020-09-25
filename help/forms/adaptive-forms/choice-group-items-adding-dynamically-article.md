---
title: 新增項目至選擇群組元件
seo-title: 新增項目至選擇群組元件
description: 動態新增項目至選擇群組元件
seo-description: 動態新增項目至選擇群組元件
feature: adaptive-forms
topics: authoring
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
translation-type: tm+mt
source-git-commit: ecbd4d21c5f41b2bc6db3b409767b767f00cc5d1
workflow-type: tm+mt
source-wordcount: '527'
ht-degree: 0%

---



# 動態新增項目至選擇群組元件

AEM Forms 6.5引入了將項目動態新增至Adaptive Forms選擇群組元件（例如CheckBox、Radio Button和Image List）的功能。

[此功能可在Samples Server上即時使用](https://forms.enablementadobe.com/content/samples/samples.html?query=0)。 搜尋「動態核取方塊」項目資訊卡，然後按一下「試用」


您可以使用視覺編輯器以及程式碼編輯器來新增項目，視您的使用案例而定。

**使用視覺編輯器：** 您可以根據函式呼叫或服務呼叫的結果填入選擇群組的項目。 例如，您可以使用REST API呼叫的回應來設定選擇群組的項目。

在以下的螢幕擷取中，我們將設定貸款期間（年）選項，以取得名為getLoanPeriods的服務呼叫結果。

![規則編輯器](assets/ruleeditor.png)

**使用程式碼編輯器**:當您想要根據在表單中輸入的值動態設定選擇群組中的項目時。 例如，以下代碼片段將複選框的項設定為在「最適化表單」的申請人名稱和配偶欄位中輸入的值。

在代碼片段中，我們正在設定WorkingMembers的項目（作為複選框元件）。 項目的陣列是動態建立的，方法是擷取最適化表單的applicantName和配偶文字欄位的值

```javascript
 
 if(MaritalStatus.value=="Married")
  {
WorkingMembers.items =["spouse="+spouse.value,"applicant="+applicantName.value];
  }
else
  {
    WorkingMembers.items =["applicant="+applicantName.value];
  }
```

提交的資料如下

```xml
<afUnboundData>

<data>

<applicantName>John Jacobs</applicantName>

<MaritalStatus>Married</MaritalStatus>

<spouse>Gloria Rios</spouse>

<WorkingMembers>spouse,applicant</WorkingMembers>

</data>

</afUnboundData>
```

**使用規則編輯器新增項目**

>[!VIDEO](https://video.tv.adobe.com/v/26847?quality=12&learn=on)

**使用程式碼編輯器新增項目**

>[!VIDEO](https://video.tv.adobe.com/v/26848?quality=12&learn=on)

要在系統上試用此功能：

**使用程式碼編輯器來新增項目**

* [下載資產](assets/usingthecodeeditor.zip)
* [開啟表單和檔案](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 按一下「建立」 |檔案上傳」，並上傳您在上一步驟中下載的檔案
* [預覽表格](http://localhost:4502/content/dam/formsanddocuments/simpleform/jcr:content?wcmmode=disabled)
* 輸入申請人姓名並選擇要結婚的婚姻狀態
* 輸入配偶姓名
* 按「下一步」
* 如果婚姻狀態已婚，您應該看到填入申請人姓名和配偶姓名的複選框

**使用視覺編輯器來新增項目**

* [下載資產](assets/usingthevisualeditor.zip)
* 如果尚未安裝Tomcat。 [此處提供安裝tomcat的說明](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html)
* [在Tomcat中部署SampleRest.war檔案](https://forms.enablementadobe.com/content/DemoServerBundles/SampleRest.war)
* [開啟表單和檔案](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 按一下「建立」 |檔案上傳」，並上傳您在上一步驟中下載的檔案
* [預覽表格](http://localhost:4502/content/dam/formsanddocuments/amortizationschedule/jcr:content?wcmmode=disabled)
* 輸入貸款金額並從欄位中輸入標籤。 這將觸發顯示貸款期間欄位的規則。
* 選擇適當的貸款期間（從其餘呼叫中填入貸款期間的項目）
* 選擇利率，然後按一下「獲得攤銷計畫」
* 應填入攤銷表。 使用REST呼叫提取攤銷時間表。

>[!NOTE]
> 假定tomcat在埠8080上運行，AEM在埠4502上運行
