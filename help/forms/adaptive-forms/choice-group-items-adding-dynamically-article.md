---
title: 新增專案至選擇群組元件
description: 動態新增專案至選擇群組元件
feature: Adaptive Forms
version: 6.5
topic: Development
role: User
level: Beginner
exl-id: 8fbea634-7949-417f-a4d6-9e551fff63f3
last-substantial-update: 2021-09-10T00:00:00Z
duration: 374
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '456'
ht-degree: 0%

---

# 將專案動態新增至選擇群組元件

AEM Forms 6.5匯入動態新增專案至最適化Forms選擇群組元件（例如核取方塊、選項按鈕和影像清單）的功能。


您可以根據使用案例，使用視覺化編輯器和程式碼編輯器新增專案。

**使用視覺化編輯器：** 您可以從函式呼叫或服務呼叫的結果中填入選擇群組的專案。 例如，您可以透過使用REST API呼叫的回應來設定選擇群組的專案。

在下方熒幕擷圖中，我們將Loan Period(years)的選項設定為getLoanPeriods服務電話的結果。

![規則編輯器](assets/ruleeditor.png)

**使用程式碼編輯器**：當您想要根據表單中輸入的值動態設定選擇群組中的專案時。 例如，下列程式碼片段會將核取方塊的專案設定為在最適化表單的申請人名稱和配偶欄位中輸入的值。

在程式碼片段中，我們將設定WorkingMembers （核取方塊元件）的專案。 正在透過擷取適用性表單的applicantName和配偶文字欄位值來動態建立專案的陣列

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

**使用規則編輯器新增專案**

>[!VIDEO](https://video.tv.adobe.com/v/26847?quality=12&learn=on)

**使用程式碼編輯器新增專案**

>[!VIDEO](https://video.tv.adobe.com/v/26848?quality=12&learn=on)

若要在您的系統上嘗試此方法：

**使用程式碼編輯器新增專案**

* [下載資產](assets/usingthecodeeditor.zip)
* [開啟Forms和檔案](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 按一下「建立」 |檔案上傳」並上傳您在上一步中下載的檔案
* [預覽表單](http://localhost:4502/content/dam/formsanddocuments/simpleform/jcr:content?wcmmode=disabled)
* 輸入應徵者名稱，並選取已婚者的婚姻狀況
* 輸入配偶姓名
* 按「下一步」
* 若婚姻狀況已婚，您應該看到已填入申請人名稱與配偶名稱的核取方塊

**使用視覺化編輯器新增專案**

* [下載資產](assets/usingthevisualeditor.zip)
* 請安裝Tomcat （如果尚未安裝）。 [此處提供安裝tomcat的說明](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html)
* [將此zip檔案中包含的SampleRest.war檔案部署至Tomcat中](assets/sample-rest.zip)
* [開啟Forms和檔案](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 按一下「建立」 |檔案上傳」並上傳您在上一步中下載的檔案
* [預覽表單](http://localhost:4502/content/dam/formsanddocuments/amortizationschedule/jcr:content?wcmmode=disabled)
* 在欄位外輸入貸款金額與標籤。 這將觸發顯示貸款期間欄位的規則。
* 選取適當的貸款期間（從剩餘通話填入貸款期間的專案）
* 選取利率並按一下「取得攤銷排程」
* 應填入攤銷表。 攤銷排程是使用REST呼叫擷取。

>[!NOTE]
> 我們假定tomcat是在連線埠8080上執行，而AEM是在連線埠4502上執行
