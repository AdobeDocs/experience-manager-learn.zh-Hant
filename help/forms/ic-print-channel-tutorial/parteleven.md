---
title: 在提交貼文時開啟代理UI
seo-title: 在提交貼文時開啟代理UI
description: 這是多步驟教學課程的第11部分，以針對列印頻道建立您的第一個互動式通訊檔案。 在本部分，我們將啟動代理用戶介面，以便在表單提交時建立臨機通信。
seo-description: 這是多步驟教學課程的第11部分，以針對列印頻道建立您的第一個互動式通訊檔案。 在本部分，我們將啟動代理用戶介面，以便在表單提交時建立臨機通信。
uuid: 96f34986-a5c3-400b-b51b-775da5d2cbd7
feature: interactive-communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 5682f024-a2f3-46a0-8274-3bdefe4a3905
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '358'
ht-degree: 0%

---


# 在提交貼文時開啟代理UI

在本部分，我們將啟動代理用戶介面，以便在表單提交時建立臨機通信。

本文將引導您瞭解在提交表單時開啟代理使用者介面的步驟。 典型的使用案例是客戶服務代理用一些輸入參數填寫表單，表單提交代理用戶介面上用表單資料模型預填充服務中預先填充的資料開啟。表單資料模型預填充服務的輸入參數是從表單提交中提取的。

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

第1行：從request參數取得帳戶編號

第2-8行：建立參數映射並設定適當的索引鍵和值，以反映documentId、Random。

第9-10行：建立另一個Map對象以保存在表單資料模型中定義的輸入參數。

第11行：設定slingRequest屬性&quot;paramMap&quot;

第12-13行：將請求轉發到servlet

若要在伺服器上測試此功能

* [使用套件管理員匯入並安裝與本文相關的資產。](assets/launch-agent-ui.zip)
* [登入configMgr](http://localhost:4502/system/console/configMgr)
* 搜尋 _Adobe Granite CSRF濾鏡_
* 在排除 _的路徑中新增_ /content/getprintchannel
* 儲存您的變更。
* [開啟POST.jsp](http://localhost:4502/apps/AEMForms/openprintchannel/POST.jsp)。 請確定傳遞至FormFieldRequestParameter的字串是有效的documentId。（19號線）。
* [開啟網頁](http://localhost:4502/content/OpenPrintChannel.html) ，輸入帳號並提交表格。
* 代理UI介面應開啟，並預先填入表單中輸入之帳號專屬的資料。

>[!NOTE]
>
>請確定您的表單資料模型的「取得」作業的輸入參數已系結至名為「accountnumber」的「請求屬性」，如此才能運作。 如果將綁定值的名稱更改為任何其他名稱，請確保在POST.jsp的第25行中反映該更改

