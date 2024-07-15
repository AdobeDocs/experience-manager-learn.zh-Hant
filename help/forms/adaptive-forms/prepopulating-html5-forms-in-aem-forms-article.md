---
title: 使用資料屬性預先填入HTML5 Forms。
description: 從後端來源擷取資料，填入HTML5表單。
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: ab0f5282-383b-4be6-9c57-cded6ab37528
last-substantial-update: 2020-01-09T00:00:00Z
duration: 94
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '449'
ht-degree: 0%

---

# 使用資料屬性預先填入HTML5 Forms {#prepopulate-html-forms-using-data-attribute}


使用AEM Forms以HTML格式轉譯的XDP範本稱為HTML5或Mobile Forms。 常見的使用案例是在轉譯表單時預先填入這些表單。

有2種方式可以在資料轉譯為HTML時與xdp範本合併。

**dataRef**：您可以在URL中使用dataRef引數。 此引數會指定與範本合併之資料檔案的絕對路徑。 此引數可以是Rest服務的URL，會以XML格式傳回資料。

**資料**：此引數會指定與範本合併的UTF-8編碼資料位元組。 如果指定此引數，HTML5表單會忽略dataRef引數。 我們建議您使用資料方法，作為最佳實務。

建議使用您要預先填入表單的資料，在請求中設定資料屬性。

slingRequest.setAttribute(&quot;data&quot;， content)；

在此範例中，我們使用內容來設定資料屬性。 內容代表您想要預先填入表單的資料。 通常，您會透過對內部服務進行REST呼叫來擷取「內容」。

若要達到此使用案例，您需要建立自訂設定檔。 建立自訂設定檔的詳細資訊已清楚記錄在[此處的AEM Forms檔案中](https://helpx.adobe.com/aem-forms/6/html5-forms/custom-profile.html)。

建立自訂設定檔後，您將會建立JSP檔案，透過呼叫後端系統來擷取資料。 擷取資料後，您將使用slingRequest.setAttribute(&quot;data&quot;， content)；預先填入表單

轉譯XDP時，您也可以將一些引數傳入xdp，並根據引數的值，從後端系統擷取資料。

[例如，此URL具有名稱引數](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=john)

您編寫的JSP將可以透過request.getParameter(&quot;name&quot;)存取name引數。 然後，您可以將此引數的值傳遞至後端程式，以擷取所需的資料。
若要讓此功能在您的系統上運作，請遵循下列步驟：

* [使用封裝管理員下載並匯入資產到AEM](assets/prepopulatemobileform.zip)
此套件將安裝下列專案

   * CustomProfile
   * 範例XDP
   * 將傳回資料以填入表單的範例POST端點

>[!NOTE]
>
>如果您要透過呼叫Workbench程式來填入表單，則可在/apps/AEMFormsDemoListings/customprofiles/PrepopulateForm/html.jsp中加入callWorkbenchProcess.jsp，而非setdata.jsp

* [將您最愛的瀏覽器指向此url](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=Adobe%20Systems)。 表單應預先填入name引數的值
