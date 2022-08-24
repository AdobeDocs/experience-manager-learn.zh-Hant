---
title: 使用資料屬性預填充HTML5Forms。
description: 通過從後端源讀取資料來填充HTML5表單。
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: ab0f5282-383b-4be6-9c57-cded6ab37528
source-git-commit: d7b1ab815d9c8a0d0342f7b57d1efb08fb39a26a
workflow-type: tm+mt
source-wordcount: '471'
ht-degree: 0%

---

# 使用資料屬性預填充HTML5Forms {#prepopulate-html-forms-using-data-attribute}


使用AEM Forms以HTML格式呈現的XDP模板稱為HTML5或移動Forms。 常見的使用情形是在呈現這些表單時預先填充它們。

當將xdp模板呈現為HTML時，有兩種方法將資料與其合併。

**資料引用**:可以在URL中使用dataRef參數。 此參數指定與模板合併的資料檔案的絕對路徑。 此參數可以是URL，用於以XML格式返回資料的余料服務。

**資料**:此參數指定與模板合併的UTF-8編碼資料位元組。 如果指定此參數，則HTML5表單將忽略dataRef參數。 作為最佳做法，我們建議使用資料方法。

建議的方法是在請求中設定要預先填充表單的資料屬性。

slingRequest.setAttribute（&quot;data&quot;，內容）;

在本示例中，我們將設定包含內容的資料屬性。 內容表示要預填充表單的資料。 通常，您會通過向內部服務發出REST呼叫來獲取「內容」。

要實現此使用情形，您需要建立自定義配置檔案。 有關建立自定義配置檔案的詳細資訊，請在 [AEM Forms文檔](https://helpx.adobe.com/aem-forms/6/html5-forms/custom-profile.html)。

建立自定義配置檔案後，將建立一個JSP檔案，該檔案將通過調用後端系統來獲取資料。 讀取資料後，將使用slingRequest.setAttribute(&quot;data&quot;, content);預填充表單

在呈現XDP時，您還可以將一些參數傳遞到xdp，並根據參數的值可以從後端系統獲取資料。

[例如，此url具有name參數](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=john)

您編寫的JSP將有權通過request.getParameter(&quot;name&quot;)訪問name參數。 然後，您可以將此參數的值傳遞到後端進程以獲取所需資料。
要使此功能在您的系統上工作，請執行以下步驟：

* [使用包管理器將資AEM產下載並導入](assets/prepopulatemobileform.zip)
軟體包將安裝以下

   * 自定義配置檔案
   * 示例XDP
   * 將返回資料以填充表單的示例POST終結點

>[!NOTE]
>
>如果要通過調用workbench進程來填充表單，則可能希望在/apps/AEMFormsDemoListings/customprofiles/PrepopulateForm/html.jsp中包含callWorkbenchProcess.jsp，而不是setdata.jsp

* [將您最喜愛的瀏覽器指向此URL](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=Adobe%20Systems)。 窗體應使用name參數的值進行預填充
