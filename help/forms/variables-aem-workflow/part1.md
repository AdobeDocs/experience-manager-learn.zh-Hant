---
title: AEM Workflow[Part1]中的變數
description: 在AEM工作流程中使用XML、JSON、ArrayList、Document類型的變數
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: f9782684-3a74-4080-9680-589d3f901617
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '422'
ht-degree: 0%

---

# AEM工作流程中的XML變數

當您有以XSD為基礎的適用性表單，且想要在工作流程中從適用性表單提交擷取值時，通常會使用XML類型的變數。

以下影片會逐步帶您了解建立字串和XML類型的變數，以及在工作流程中使用這些變數所需的步驟。

XML變數可用來預先填入最適化表單，或將最適化表單的提交資料儲存在工作流程中。

字串變數可由Xpathing填入至XML變數。 然後，此字串變數通常用於填入「傳送電子郵件」元件中的電子郵件範本預留位置

>[!NOTE]
>
>如果您的適用性表單未與XSD關聯，則取得元素值的XPath看起來會像
>
>**/afData/afUnboundData/data/submitterName**

適用性表單資料會儲存在資料元素下，如上所示。 **_在上面的XPath submitterName是適用性表單中文本欄位的名稱。_**

>[!NOTE]
>
>**AEM Forms 6.5.0**  — 當您建立XML類型的變數以在工作流模型中擷取已提交的資料時，請勿將XSD與變數建立關聯。 這是因為提交基於XSD的適用性表單時，提交的資料與XSD不相容。 XSD投訴資料以/afData/afBoundData/元素括住。
>
>**AEM Forms 6.5.1**  — 如果將XSD與XML變數關聯，則可以瀏覽架構元素以執行變數映射。 您將無法存取未系結至結構元素的表單資料。 如果您的使用案例是訪問綁定到架構元素的資料以及未綁定的資料，則不要在工作流中將架構與XML變數綁定。您必須使用適當的XPath表達式來獲取所需的資料

## 建立XML變數

>[!VIDEO](https://video.tv.adobe.com/v/26440?quality=12&learn=on)

### 使用結構與XML變數

**將XML變數與結構對應。 將此功能與AEM Forms 6.5.1以上版本搭配使用**

>[!VIDEO](https://video.tv.adobe.com/v/28098?quality=12&learn=on)

#### 在傳送電子郵件時使用變數

>[!VIDEO](https://video.tv.adobe.com/v/26441?quality=12&learn=on)

若要讓資產在您的系統上運作，請依照下列步驟操作：

* [使用套件管理器將資產下載並匯入AEM](assets/xmlandstringvariable.zip)
* [探索工作流程模型](http://localhost:4502/editor.html/conf/global/settings/workflow/models/vacationrequest.html) 了解工作流程中使用的變數
* [設定電子郵件服務](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [開啟最適化表單](http://localhost:4502/content/dam/formsanddocuments/applicationfortimeoff/jcr:content?wcmmode=disabled)
* 填寫詳細資訊並提交表格。
