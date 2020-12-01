---
title: AEM Workflow[Part1]中的變數
seo-title: AEM Workflow[Part1]中的變數
description: 在AEM工作流程中使用xml,json,arraylist,document類型的變數
seo-description: 在AEM工作流程中使用xml,json,arraylist,document類型的變數
feature: workflow
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '437'
ht-degree: 0%

---


# AEM工作流程中的XML變數

當您有以XSD為基礎的最適化表單，並想要從工作流程中的最適化表單提交擷取值時，通常會使用XML類型的變數。

以下視訊會逐步帶您完成建立字串和XML類型變數，並在工作流程中使用這些變數所需的步驟。

XML變數可用來預先填入最適化表單，或將最適化表單的提交資料儲存在工作流程中。

字串變數可由Xpathing填入XML變數。 此字串變數通常用於填入「傳送電子郵件」元件中的電子郵件範本預留位置

>[!NOTE]
>
>如果您的最適化表單與XSD沒有關聯，則取得元素值的XPath看起來會像
>
>**/afData/afUnboindData/data/submitterName**

自適應表單資料儲存在如上所示的資料元素下。 **_在上述XPath submitterName中，是Adaptive Form中文本欄位的名稱。_**

>[!NOTE]
>
>**AEM Forms 6.5.0**  —— 當您建立XML類型的變數以擷取工作流程模型中提交的資料時，請勿將XSD與變數建立關聯。這是因為當您提交基於XSD的最適化表單時，提交的資料與XSD不相容。 XSD投訴資料會封入/afData/afBoundData/元素中。
>
>**AEM Forms 6.5.1**  —— 如果您將XSD與XML變數建立關聯，您可以瀏覽架構元素以進行變數對應。您將無法存取未系結至架構元素的表單資料。 如果您的使用案例是存取系結至架構元素的資料以及未系結的資料，則請勿在工作流程中將架構與您的XML變數系結。您必須使用適當的XPath運算式才能取得所需的資料

## 建立XML變數

>[!VIDEO](https://video.tv.adobe.com/v/26440?quality=12?autoplay=1)

### 將架構與XML變數搭配使用

**將XML變數與架構對應。將此功能與AEM Forms 6.5.1之後版本搭配使用**

>[!VIDEO](https://video.tv.adobe.com/v/28098?quality=9&learn=on)

#### 在傳送電子郵件時使用變數

>[!VIDEO](https://video.tv.adobe.com/v/26441?quality=12&learn=on)

若要讓資產在您的系統上運作，請遵循下列步驟：

* [使用套件管理員將資產下載並匯入AEM](assets/xmlandstringvariable.zip)
* [探索工作流](http://localhost:4502/editor.html/conf/global/settings/workflow/models/vacationrequest.html) 程模型，瞭解工作流程中使用的變數
* [設定電子郵件服務](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [開啟最適化表單](http://localhost:4502/content/dam/formsanddocuments/applicationfortimeoff/jcr:content?wcmmode=disabled)
* 填寫詳細資訊並提交表格。

