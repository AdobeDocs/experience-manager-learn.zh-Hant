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
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '471'
ht-degree: 0%

---

# 使用資料屬性預先填入HTML5 Forms {#prepopulate-html-forms-using-data-attribute}


使用AEM Forms以HTML格式轉譯的XDP範本稱為「HTML5」或「行動Forms」。 常見的使用案例是在轉譯表單時預先填入這些表單。

有2種方式可在將資料轉譯為HTML時，將資料與xdp範本合併。

**dataRef**:您可以在URL中使用dataRef參數。 此參數指定與模板合併的資料檔案的絕對路徑。 此參數可以是XML格式傳回資料之其餘服務的URL。

**資料**:此參數指定與範本合併的UTF-8編碼資料位元組。 如果指定了此參數，HTML5表單將忽略dataRef參數。 建議您使用資料方法，作為最佳實務。

建議的方法是在請求中設定資料屬性，並預先填入您要填入的表格資料。

slingRequest.setAttribute(&quot;data&quot;, content);

在此範例中，我們會使用內容設定資料屬性。 內容代表您要預先填入表單的資料。 通常，您會對內部服務發出REST呼叫來擷取「內容」。

若要達成此使用案例，您必須建立自訂設定檔。 有關建立自訂設定檔的詳細資訊，請詳見 [AEM Forms檔案此處](https://helpx.adobe.com/aem-forms/6/html5-forms/custom-profile.html).

建立自訂設定檔後，您將建立JSP檔案，此檔案會呼叫後端系統以擷取資料。 擷取資料後，您就會使用slingRequest.setAttribute(&quot;data&quot;, content);預先填入表單

轉譯XDP時，您也可以將某些參數傳遞至xdp，並根據參數的值，從後端系統擷取資料。

[例如，此url有名稱參數](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=john)

您編寫的JSP將可通過request.getParameter(&quot;name&quot;)訪問name參數。 接著，您可以將此參數的值傳遞至後端程式，以擷取所需資料。
若要讓此功能在您的系統上正常運作，請遵循下列步驟：

* [使用套件管理器將資產下載並匯入AEM](assets/prepopulatemobileform.zip)
套件將安裝下列

   * CustomProfile
   * 範例XDP
   * 將傳回資料以填入表單的範例POST端點

>[!NOTE]
>
>如果您想透過呼叫workbench程式來填入表單，您可能想在/apps/AEMFormsDemoListings/customprofiles/PrepopulateForm/html.jsp中加入callWorkbenchProcess.jsp，而非setdata.jsp

* [將您最喜愛的瀏覽器指向此url](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=Adobe%20Systems). 表單應已預先填入name參數的值
