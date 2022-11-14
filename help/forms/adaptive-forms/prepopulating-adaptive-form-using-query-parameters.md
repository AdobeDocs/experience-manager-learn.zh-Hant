---
title: 使用查詢參數填入適用性Forms。
description: 將查詢參數的資料填入適用性Forms。
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Experienced
kt: 11470
last-substantial-update: 2020-11-12T00:00:00Z
source-git-commit: fad7630d2d91d03b98a3982f73a689ef48700319
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 0%

---

# 使用查詢參數預先填入適用性Forms

我們的其中一位客戶需要使用查詢參數填入最適化表單。 例如，在以下URL中，適用性表單中的FirstName和LastName欄位分別設為John和Doe

```html
https://forms.enablementadobe.com/content/forms/af/testingxml.html?FirstName=John&LastName=Doe
```

為了完成此使用案例，已建立新的最適化表單範本，並與頁面元件建立關聯。 在此頁面元件中，我們有jsp來取得查詢參數並建立可用來填入最適化表單的xml結構。

有關建立新適用性表單範本和頁面元件的詳細資訊為 [在本影片中說明。](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/storing-and-retrieving-form-data/part5.html?lang=en)

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
>如果您的表單使用架構，則xml的結構將會不同，您必須據此建立xml。


## 在您的系統上部署資產

* [使用封裝管理器下載及安裝最適化表單範本](assets/populate-with-xml.zip)
* [下載並安裝範例最適化表單](assets/populate-af-with-query-paramters-form.zip)

* [預覽最適化表單](http://localhost:4502/content/dam/formsanddocuments/testingxml/jcr:content?wcmmode=disabled&amp;FirstName=John&amp;LastName=Doe)
您應會看到適用性表單已填入「John」和「Doe」值
