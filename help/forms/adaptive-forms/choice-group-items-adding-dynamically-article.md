---
title: 將項添加到選擇組元件
seo-title: 將項添加到選擇組元件
description: 動態新增項目至選擇的群組元件
seo-description: 動態新增項目至選擇的群組元件
feature: 適用性表單
topics: authoring
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
topic: 開發
role: Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '530'
ht-degree: 0%

---



# 動態新增項目至選擇的群組元件

AEM Forms 6.5導入了動態新增項目至最適化Forms選擇群組元件（例如CheckBox、選項按鈕和影像清單）的功能。

[此功能可在Samples Server上即時使用](https://forms.enablementadobe.com/content/samples/samples.html?query=0)。搜尋「動態核取方塊」項目卡片，然後按一下「嘗試」


您可以根據您的使用案例，使用視覺編輯器以及程式碼編輯器來新增項目。

**使用視覺編輯器：** 您可以從函式呼叫或服務呼叫的結果填入選擇群組的項目。例如，您可以使用REST API呼叫的回應來設定選擇群組的項目。

在下面的螢幕截圖中，我們將Loan Periods(years)選項設定為名為getLoanPeriods的服務呼叫的結果。

![規則編輯器](assets/ruleeditor.png)

**使用程式碼編輯器**:當您想要根據表單中輸入的值，以動態方式設定選擇群組中的項目時。例如，下列程式碼片段會將核取方塊的項目設定為在「適用性表單」的申請人名稱和配偶欄位中輸入的值。

在程式碼片段中，我們會設定WorkingMembers的項目，這是核取方塊元件。 通過讀取最適化表單的applicantName和配偶文本欄位的值，動態地建立項的陣列

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

**使用代碼編輯器新增項目**

>[!VIDEO](https://video.tv.adobe.com/v/26848?quality=12&learn=on)

要在您的系統上嘗試，請執行以下操作：

**使用代碼編輯器來新增項目**

* [下載資產](assets/usingthecodeeditor.zip)
* [開啟Forms和檔案](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 按一下「建立」 |檔案上傳」，並上傳您在上一步下載的檔案
* [預覽表單](http://localhost:4502/content/dam/formsanddocuments/simpleform/jcr:content?wcmmode=disabled)
* 輸入申請人姓名並選擇要結婚的婚姻狀態
* 輸入配偶姓名
* 按「下一步」
* 如果婚姻狀況已結婚，您應該看到填有申請人姓名和配偶姓名的複選框

**使用可視化編輯器來新增項目**

* [下載資產](assets/usingthevisualeditor.zip)
* 如果尚未安裝Tomcat，請安裝它。 [此處提供安裝tomcat的說明](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html)
* [在Tomcat中部署SampleRest.war檔案](https://forms.enablementadobe.com/content/DemoServerBundles/SampleRest.war)
* [開啟Forms和檔案](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 按一下「建立」 |檔案上傳」，並上傳您在上一步下載的檔案
* [預覽表單](http://localhost:4502/content/dam/formsanddocuments/amortizationschedule/jcr:content?wcmmode=disabled)
* 在欄位中輸入貸款金額和分頁。 這會觸發顯示貸款期間欄位的規則。
* 選擇適當的貸款期間（從其餘呼叫中填充貸款期間的項）
* 選擇利率，然後按一下「獲取攤銷計畫」
* 應填入攤銷表。 使用REST呼叫來擷取攤銷排程。

>[!NOTE]
> 假定在埠8080上運行tomcat，在埠4502上運行AEM
