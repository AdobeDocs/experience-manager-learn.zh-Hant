---
title: Workflow[AEMPart1]中的變數
description: 在工作流中使用XML、JSON、ArrayList和Document類型的變AEM量
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

# 工作流中的XMLAEM變數

當您具有基於XSD的自適應表單並且希望從工作流中的自適應表單提交中提取值時，通常使用XML類型的變數。

以下視頻將引導您完成建立字串和XML類型的變數並在工作流中使用它們所需的步驟。

XML變數可用於預填充自適應表單或將自適應表單的提交資料儲存在工作流中。

字串變數可以通過Xpathing填充到XML變數中。 然後，此字串變數通常用於填充「發送電子郵件」元件中的電子郵件模板佔位符

>[!NOTE]
>
>如果您的自適應表單與XSD不關聯，則獲取元素值的XPath將看起來類似
>
>**/afData/afUnboundData/data/submitterName**

如上所示，自適應表單資料被儲存在資料元素下。 **_在上面的XPath submitterName是Adaptive Form中文本欄位的名稱。_**

>[!NOTE]
>
>**AEM Forms6.5.0**  — 建立XML類型的變數以捕獲工作流模型中提交的資料時，請不要將XSD與變數關聯。 這是因為當您提交基於XSD的Adaptive Form時，提交的資料與XSD不符。 XSD投訴資料包含在/afData/afBoundData/元素中。
>
>**AEM Forms6.5.1**  — 如果將XSD與XML變數關聯，則可以瀏覽架構元素以執行變數映射。 您將無法訪問未綁定到架構元素的表單資料。 如果您的使用案例是訪問綁定到架構元素的資料以及未綁定的資料，則不要在工作流中將架構與XML變數綁定。您必須使用適當的XPath表達式來獲取所需的資料

## 建立XML變數

>[!VIDEO](https://video.tv.adobe.com/v/26440?quality=12&learn=on)

### 將架構與XML變數一起使用

**使用架構映射XML變數。 將此功能用於AEM Forms6.5.1以後**

>[!VIDEO](https://video.tv.adobe.com/v/28098?quality=12&learn=on)

#### 在發送電子郵件中使用變數

>[!VIDEO](https://video.tv.adobe.com/v/26441?quality=12&learn=on)

要使系統上的資產正常工作，請執行以下步驟：

* [使用包管理器將資產下載AEM並導入到](assets/xmlandstringvariable.zip)
* [瀏覽工作流模型](http://localhost:4502/editor.html/conf/global/settings/workflow/models/vacationrequest.html) 瞭解工作流中使用的變數
* [配置電子郵件服務](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [開啟自適應窗體](http://localhost:4502/content/dam/formsanddocuments/applicationfortimeoff/jcr:content?wcmmode=disabled)
* 填寫詳細資訊並提交表單。
