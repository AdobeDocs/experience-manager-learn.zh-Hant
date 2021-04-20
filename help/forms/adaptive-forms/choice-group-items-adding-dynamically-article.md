---
title: 新增項目至選擇群組元件
seo-title: 新增項目至選擇群組元件
description: 動態新增項目至選擇群組元件
seo-description: 動態新增項目至選擇群組元件
feature: Adaptive Forms
topics: authoring
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
topic: Development
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '533'
ht-degree: 0%

---



# 動態新增項目至選擇群組元件

AEM Forms6.5引入了動態添加項目到自適應Forms選擇組元件（如CheckBox、單選按鈕和影像清單）的能力。

[此功能可在Samples Server上即時使用](https://forms.enablementadobe.com/content/samples/samples.html?query=0)。搜尋「動態核取方塊」項目資訊卡，然後按一下「試用」


您可以使用視覺編輯器以及程式碼編輯器來新增項目，視使用案例而定。

**使用視覺編輯器：** 您可以從函式呼叫或服務呼叫的結果填入選擇群組的項目。例如，您可以使用REST API呼叫的回應來設定選擇群組的項目。

在以下的螢幕擷取中，我們將設定貸款期間（年）選項，以取得名為getLoanPeriods的服務呼叫結果。

![規則編輯器](assets/ruleeditor.png)

**使用程式碼編輯器**:當您想要根據在表單中輸入的值動態設定選擇群組中的項目時。例如，以下代碼片段將複選框的項設定為在「最適化表單」的申請人名稱和配偶欄位中輸入的值。

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
* [開啟Forms和檔案](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
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
* [開啟Forms和檔案](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 按一下「建立」 |檔案上傳」，並上傳您在上一步驟中下載的檔案
* [預覽表格](http://localhost:4502/content/dam/formsanddocuments/amortizationschedule/jcr:content?wcmmode=disabled)
* 輸入貸款金額並從欄位中輸入標籤。 這將觸發顯示貸款期間欄位的規則。
* 選擇適當的貸款期間（從其餘呼叫中填入貸款期間的項目）
* 選擇利率，然後按一下「獲得攤銷計畫」
* 應填入攤銷表。 使用REST呼叫提取攤銷時間表。

>[!NOTE]
> 假定tomcat在埠8080和端AEM口4502上運行
