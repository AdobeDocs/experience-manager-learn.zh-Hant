---
title: 將項添加到選擇組元件
description: 動態將項添加到選擇組元件
feature: Adaptive Forms
version: 6.5
topic: Development
role: User
level: Beginner
exl-id: 8fbea634-7949-417f-a4d6-9e551fff63f3
last-substantial-update: 2021-09-10T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '489'
ht-degree: 0%

---

# 將項動態添加到選擇組元件

AEM Forms6.5引入了將項目動態添加到自適應Forms選擇組元件（如CheckBox、單選按鈕和影像清單）的功能。


您可以使用可視編輯器和代碼編輯器添加項目，具體取決於您的使用案例。

**使用可視編輯器：** 您可以根據函式調用或服務調用的結果來填充選擇組的項。 例如，可以通過使用REST API調用的響應來設定選擇組的項。

在下面的螢幕快照中，我們將將「貸款期間（年）」選項設定為名為getLoanPeriods的服務呼叫的結果。

![規則編輯器](assets/ruleeditor.png)

**使用代碼編輯器**:根據在表單中輸入的值動態設定選擇組中的項目時。 例如，以下代碼段將複選框的項設定為在「自適應表單」的申請人名稱和配偶欄位中輸入的值。

在代碼段中，我們正在設定WorkingMembers的項，該項是複選框元件。 通過讀取自適應表單的panciptName和配偶文本欄位的值，動態構建項的陣列

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

**使用規則編輯器添加項**

>[!VIDEO](https://video.tv.adobe.com/v/26847?quality=12&learn=on)

**使用代碼編輯器添加項**

>[!VIDEO](https://video.tv.adobe.com/v/26848?quality=12&learn=on)

要在系統上嘗試此操作：

**使用代碼編輯器添加項**

* [下載資產](assets/usingthecodeeditor.zip)
* [開啟Forms和文檔](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 按一下「建立」 「|檔案上載」，並上載您在上一步中下載的檔案
* [預覽表單](http://localhost:4502/content/dam/formsanddocuments/simpleform/jcr:content?wcmmode=disabled)
* 輸入申請人姓名並選擇要結婚的婚姻狀態
* 輸入配偶姓名
* 按一下「下一步」
* 如果婚姻狀況已結婚，您應看到填有申請人姓名和配偶姓名的複選框

**使用可視編輯器添加項**

* [下載資產](assets/usingthevisualeditor.zip)
* 如果尚未安裝Tomcat，請安裝它。 [此處提供了安裝tomcat的說明](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html)
* [部署Tomcat中此zip檔案中包含的SampleRest.war檔案](assets/sample-rest.zip)
* [開啟Forms和文檔](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 按一下「建立」 「|檔案上載」，並上載您在上一步中下載的檔案
* [預覽表單](http://localhost:4502/content/dam/formsanddocuments/amortizationschedule/jcr:content?wcmmode=disabled)
* 在欄位中輸入貸款金額和標籤。 這將觸發顯示貸款期間欄位的規則。
* 選擇適當的貸款期間（貸款期間的項目由剩餘呼叫填充）
* 選擇利率，然後按一下「獲取攤銷計畫」
* 應填充攤銷表。 使用REST調用獲取攤銷計畫。

>[!NOTE]
> 假定tomcat在埠8080和埠4502AEM上運行
