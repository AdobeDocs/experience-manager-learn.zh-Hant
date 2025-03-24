---
title: AEM工作流程中的變數[第1部分]
description: 在AEM工作流程中使用XML、JSON、ArrayList、檔案型別的變數
feature: Adaptive Forms, Workflow
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
exl-id: f9782684-3a74-4080-9680-589d3f901617
duration: 561
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '404'
ht-degree: 0%

---

# AEM工作流程中的XML變數

如果您有XSD式最適化表單，而且想要從工作流程中的最適化表單提交中擷取值，通常會使用XML型別的變數。

以下影片將逐步引導您完成建立字串和XML型別變數，以及在工作流程中使用這些變數所需的步驟。

XML變數可用來預先填入最適化表單，或將最適化表單的提交資料儲存在您的工作流程中。

字串變數可由Xpathing填入XML變數中。 然後，此字串變數通常用於填入「傳送電子郵件」元件中的電子郵件範本預留位置

>[!NOTE]
>
>如果您的調適型表單未與XSD相關聯，則用於取得元素值的XPath看起來會類似
>
>**/afData/afUnboundData/data/submitterName**

最適化表單資料儲存在資料元素下，如上所示。 **_上述XPath submitterName為最適化表單中文字欄位的名稱。_**

>[!NOTE]
>
>**AEM Forms 6.5.0** — 當您建立型別為XML的變數來擷取工作流程模型中提交的資料時，請勿將XSD與變數建立關聯。 這是因為當您提交以XSD為基礎的最適化表單時，提交的資料與XSD不相容。 XSD投訴資料包含在/afData/afBoundData/元素中。
>
>**AEM Forms 6.5.1** — 如果您將XSD與XML變數相關聯，可以瀏覽結構描述元素以進行變數對應。 您將無法存取未繫結至結構描述元素的表單資料。 如果您的使用案例是存取繫結至結構描述元素的資料以及未繫結的資料，則請勿在工作流程中繫結結構描述與XML變數。您必須使用適當的XPath運算式來取得您需要的資料

## 建立XML變數

>[!VIDEO](https://video.tv.adobe.com/v/26440?quality=12&learn=on)

### 搭配使用結構描述和XML變數

**使用結構描述對應XML變數。 將此功能用於AEM Forms 6.5.1以上**

>[!VIDEO](https://video.tv.adobe.com/v/28098?quality=12&learn=on)

#### 在傳送電子郵件中使用變數

>[!VIDEO](https://video.tv.adobe.com/v/26441?quality=12&learn=on)

若要讓資產在您的系統上正常運作，請遵循下列步驟：

* [使用封裝管理程式下載資產並將其匯入AEM](assets/xmlandstringvariable.zip)
* [探索工作流程模型](http://localhost:4502/editor.html/conf/global/settings/workflow/models/vacationrequest.html)以瞭解用於工作流程中的變數
* [設定電子郵件服務](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [開啟最適化表單](http://localhost:4502/content/dam/formsanddocuments/applicationfortimeoff/jcr:content?wcmmode=disabled)
* 填寫詳細資料並提交表單。
