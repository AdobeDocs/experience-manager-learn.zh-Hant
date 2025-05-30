---
title: 使用查詢引數填入最適化Forms。
description: 使用查詢引數的資料填入最適化Forms。
feature: Adaptive Forms
version: Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
jira: KT-11470
last-substantial-update: 2020-11-12T00:00:00Z
exl-id: 14ac6ff9-36b4-415e-a878-1b01ff9d3888
duration: 49
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '199'
ht-degree: 0%

---

# 使用查詢引數預先填入最適化Forms

我們其中一位客戶要求使用查詢引數填入最適化表單。 例如，在以下url中，最適化表單中的FirstName和LastName欄位分別設定為John和Doe

```html
https://forms.enablementadobe.com/content/forms/af/testingxml.html?FirstName=John&LastName=Doe
```

為了完成此使用案例，已建立新的最適化表單範本並與頁面元件建立關聯。 在此頁面元件中，我們有jsp可取得查詢引數，並建立可用來填入調適型表單的xml結構。

有關建立新最適化表單範本和頁面元件的詳細資訊[在本影片中說明。](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/storing-and-retrieving-form-data/part5.html?lang=zh-Hant)

以下是用於jsp頁面的程式碼

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
>如果您的表單使用結構描述，則xml的結構將會不同，您必須據此建置xml。


## 在您的系統上部署資產

* [使用封裝管理員下載及安裝最適化表單範本](assets/populate-with-xml.zip)
* [下載並安裝最適化表單範例](assets/populate-af-with-query-paramters-form.zip)

* [預覽最適化表單](http://localhost:4502/content/dam/formsanddocuments/testingxml/jcr:content?wcmmode=disabled&amp;FirstName=John&amp;LastName=Doe)
您應該會看到最適化表單填入值John和Doe
