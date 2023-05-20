---
title: 使用查詢參數填充自適應Forms。
description: 使用查詢參數中的資料填充自適應Forms。
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Experienced
kt: 11470
last-substantial-update: 2020-11-12T00:00:00Z
exl-id: 14ac6ff9-36b4-415e-a878-1b01ff9d3888
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 0%

---

# 使用查詢參數預填充自適應Forms

其中一位客戶要求使用查詢參數填充自適應表單。 例如，在以下URL中，自適應表單中的FirstName和LastName欄位分別設定為John和Doe

```html
https://forms.enablementadobe.com/content/forms/af/testingxml.html?FirstName=John&LastName=Doe
```

為了完成此使用情形，建立了新的自適應表單模板並與頁面元件相關聯。 在此頁元件中，我們有一個jsp來獲取查詢參數並建立可用於填充自適應表單的xml結構。

有關建立新的Adaptive Form模板和頁面元件的詳細資訊包括 [在視頻中解釋。](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/storing-and-retrieving-form-data/part5.html?lang=en)

以下是在jsp頁中使用的代碼

```java
java.util.Enumeration enumeration = request.getParameterNames();
String dataXml = "<afData><afUnboundData><data>";
while (enumeration.hasMoreElements())
{
   String parameterName = (String) enumeration.nextElement();
   dataXml = dataXml + "<" + parameterName + ">" + request.getParameter(parameterName) + "</" + parameterName + ">";

}

dataXml = dataXml + "</data></afUnboundData></afData>";
//System.out.println("The data xml is "+dataXml);
slingRequest.setAttribute("data", dataXml);
```

>[!NOTE]
>
>如果您的表單正在使用架構，則xml的結構將不同，您必須相應地生成xml。


## 在系統上部署資產

* [使用包管理器下載並安裝Adaptive Form Template](assets/populate-with-xml.zip)
* [下載並安裝示例自適應表單](assets/populate-af-with-query-paramters-form.zip)

* [預覽自適應窗體](http://localhost:4502/content/dam/formsanddocuments/testingxml/jcr:content?wcmmode=disabled&amp;FirstName=John&amp;LastName=Doe)
您應看到自適應窗體，其中填充了值John和Doe
