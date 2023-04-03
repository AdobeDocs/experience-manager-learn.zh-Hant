---
title: 在提交POST時開啟代理程式UI
seo-title: Opening Agent UI On POST Submission
description: 這是建立打印管道第一個互動式通訊檔案的多步驟教學課程的11部分。 在本部分，我們將啟動代理ui介面，以在表單提交時建立臨機通信。
seo-description: This is part 11 of multistep tutorial for creating your first interactive communications document for the print channel. In this part, we will launch the agent ui interface for creating ad-hoc correspondence on form submission.
uuid: 96f34986-a5c3-400b-b51b-775da5d2cbd7
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6168
thumbnail: 40122.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: 509b4d0d-9f3c-46cb-8ef7-07e831775086
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 0%

---

# 在提交POST時開啟代理程式UI

在本部分，我們將啟動代理ui介面，以在表單提交時建立臨機通信。

本文將引導您完成在提交表單時開啟代理ui介面的步驟。 典型的使用案例是客戶服務代理用一些輸入參數填寫表單，在表單提交代理ui中開啟預填表單資料模型預填服務的資料。表單資料模型預填服務的輸入參數是從表單提交中提取的。

以下影片顯示使用案例

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

第1行：從request參數取得accountnumber

第2-8行：建立參數映射並設定適當的鍵和值以反映documentId、Random。

第9-10行：建立另一個映射對象以保存在「表單資料模型」中定義的輸入參數。

第11行：設定slingRequest屬性&quot;paramMap&quot;

第12-13行：將請求轉送至servlet

在伺服器上測試此功能

* [使用套件管理器匯入及安裝與本文相關的資產。](assets/launch-agent-ui.zip)
* [登入configMgr](http://localhost:4502/system/console/configMgr)
* 搜尋 _AdobeGranite CSRF篩選器_
* 新增 _/content/getprintchannel_ 在排除的路徑中
* 儲存您的變更。
* [開啟POST.jsp](http://localhost:4502/apps/AEMForms/openprintchannel/POST.jsp). 請確定傳遞至FormFieldRequestParameter的字串是有效的documentId。（19號線）。
* [開啟網頁](http://localhost:4502/content/OpenPrintChannel.html) 並輸入帳號並提交表格。
* 代理UI介面應會開啟，其中會預先填入表單中輸入之帳號的特定資料。

>[!NOTE]
>
>請確定您的表單資料模型的Get操作輸入參數系結至稱為「accountnumber」的請求屬性，以便運作。 如果將綁定值的名稱更改為任何其他名稱，請確保在POST.jsp的第25行上反映該更改
