---
title: 使用資料屬性預先填入HTML5Forms。
seo-title: 使用資料屬性預先填入HTML5Forms。
description: 從後端來源擷取資料，以填入HTML5表格。
seo-description: 從後端來源擷取資料，以填入HTML5表格。
feature: Adaptive Forms
topics: mobile-forms
audience: developer
doc-type: article
activity: implement
version: 6.3,6.4,6.5.
uuid: 889d2cd5-fcf2-4854-928b-0c2c0db9dbc2
discoiquuid: 3aa645c9-941e-4b27-a538-cca13574b21c
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '515'
ht-degree: 0%

---


# 使用資料屬性{#prepopulate-html-forms-using-data-attribute}預先填入HTML5Forms

請造訪[AEM Forms範例](https://forms.enablementadobe.com/content/samples/samples.html?query=0)頁面，以取得此功能的即時示範連結。

使用AEM Forms以HTML格式轉譯的XDP範本稱為HTML5或行動Forms。 常見的使用案例是，在轉譯這些表單時預先填入這些表單。

當xdp範本轉換為HTML時，有2種方式可將資料與xdp範本合併。

**dataRef**:您可以在URL中使用dataRef參數。此參數指定與模板合併的資料檔案的絕對路徑。 此參數可以是傳回XML格式資料之其餘服務的URL。

**資料**:此參數指定與範本合併的UTF-8編碼資料位元組。如果指定此參數，HTML5表單會忽略dataRef參數。 建議您使用資料方法作為最佳實務。

建議的方法是使用您要預先填入表單的資料，來設定請求中的資料屬性。

slingRequest.setAttribute(&quot;data&quot;, content);

在此範例中，我們會設定內容的資料屬性。 內容代表您要預先填入表單的資料。 通常，您會透過對內部服務進行REST呼叫來擷取「內容」。

若要達成此使用案例，您需要建立自訂描述檔。 有關建立自定義配置檔案的詳細資訊，請在[AEM Forms文檔中詳細說明。](https://helpx.adobe.com/aem-forms/6/html5-forms/custom-profile.html)

建立自訂描述檔後，您就會建立JSP檔案，透過呼叫後端系統來擷取資料。 擷取資料後，您就會使用slingRequest.setAttribute(&quot;data&quot;, content);要預先填入表格

在呈現XDP時，您也可以將某些參數傳入至xdp，並根據參數的值，從後端系統擷取資料。

[例如，此URL具有名稱參數](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=john)

您編寫的JSP將可透過request.getParameter(&quot;name&quot;)存取name參數。 然後，您可以將此參數的值傳遞至後端程式，以擷取所需的資料。
若要讓此功能在您的系統上運作，請遵循下列步驟：

* [下載並使用套件管理AEM器將資產匯入套](assets/prepopulatemobileform.zip)
件將會安裝下列

   * CustomProfile
   * 範例XDP
   * 將傳回資料以填入表單的範例POST端點

>[!NOTE]
>
>如果您想要透過呼叫workbench進程來填入表單，您可能想將callWorkbenchProcess.jsp包含在/apps/AEMFormsDemoListings/customprofiles/PrepopulateForm/html.jsp中，而非setdata.jsp

* [將您最愛的瀏覽器指向此URL](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=Adobe%20Systems)。表單應預先填入名稱參數的值
