---
title: 在發佈提交時開啟代理程式UI
description: 這是多步驟教學課程的第11部分，說明如何為列印管道建立您的第一份互動式通訊檔案。 在本部分中，我們將啟動代理程式ui介面，以在表單提交時建立隨選通訊。
feature: Interactive Communication
doc-type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6168
thumbnail: 40122.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: 509b4d0d-9f3c-46cb-8ef7-07e831775086
duration: 170
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '322'
ht-degree: 0%

---

# 在發佈提交時開啟代理程式UI

在本部分中，我們將啟動代理程式ui介面，以在表單提交時建立隨選通訊。

本文將逐步引導您完成在提交表單時開啟代理程式ui介面的相關步驟。 典型的使用案例是客戶服務代理使用一些輸入引數填寫表單，並且在表單提交代理程式ui上開啟了預先填入表單資料模型預填服務的資料。表單資料模型預填服務的輸入引數是從表單提交中擷取。

以下影片說明使用案例

>[!VIDEO](https://video.tv.adobe.com/v/40122?quality=12&learn=on)

```java
String accountNumber = request.getParameter("accountnumber"))
ParameterMap parameterMap = new ParameterMap();
RequestParameter icLetterId[] = new RequestParameter[1];
icLetterId[0] = new FormFieldRequestParameter("/content/dam/formsanddocuments/retirementstatementprint");
parameterMap.put("documentId", icLetterId);
RequestParameter Random[] = new RequestParameter[1];
Random[0] = new FormFieldRequestParameter("209457");
parameterMap.put("Random", Random);
Map map = new HashMap();
map.put("accountnumber",accountNumber);
slingRequest.setAttribute("paramMap",map);
CustomParameterRequest wrapperRequest = new CustomParameterRequest(slingRequest,parameterMap,"GET");
wrapperRequest.getRequestDispatcher("/aem/forms/createcorrespondence.html").include(wrapperRequest, response);
```

第1行：從requestparameter取得accountnumber

第2-8行：建立引數對應並設定適當的索引鍵和值以反映documentId，Random。

第9-10行：建立另一個Map物件以保留表單資料模型中定義的輸入引數。

第11行：設定slingRequest屬性「paramMap」

第12-13行：將請求轉送至servlet

若要在您的伺服器上測試此功能

* [使用封裝管理員匯入及安裝與本文相關的資產。](assets/launch-agent-ui.zip)
* [登入configMgr](http://localhost:4502/system/console/configMgr)
* 搜尋&#x200B;_Adobe Granite CSRF篩選器_
* 在排除的路徑中新增&#x200B;_/content/getprintchannel_
* 儲存您的變更。
* [開啟POST.jsp](http://localhost:4502/apps/AEMForms/openprintchannel/POST.jsp)。 確定傳遞給FormFieldRequestParameter的字串是有效的documentId。（第19行）。
* [開啟網頁](http://localhost:4502/content/OpenPrintChannel.html)，然後輸入accountnumber並提交表單。
* 代理程式UI介面應會開啟，並預先填入表單中所輸入帳號的特定資料。

>[!NOTE]
>
>請確定您的表單資料模型的Get作業輸入引數已繫結至名為「accountnumber」的請求屬性，這樣才能運作。 如果您將bindingvalue的名稱變更為任何其他名稱，請確定變更反映在POST.jsp的第25行上
