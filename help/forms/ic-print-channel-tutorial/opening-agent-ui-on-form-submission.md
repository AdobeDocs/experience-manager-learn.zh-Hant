---
title: 在POST提交時開啟代理UI
seo-title: Opening Agent UI On POST Submission
description: 這是多步教程的第11部分，用於為打印通道建立第一個互動式通信文檔。 在本部分，我們將啟動代理用戶介面，用於建立表單提交時的即席通信。
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

# 在POST提交時開啟代理UI

在本部分，我們將啟動代理用戶介面，用於建立表單提交時的即席通信。

本文將引導您完成在提交表單時開啟代理用戶介面涉及的步驟。 典型的使用案例是客戶服務代理用一些輸入參數填寫表單，在表單提交代理用戶介面上用表單資料模型預填充服務中預先填充的資料開啟。表單資料模型預填充服務的輸入參數從表單提交中提取。

以下視頻顯示用例

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

第1行：從request參數獲取帳號

第2-8行：建立參數映射並設定相應的鍵和值以反映documentId,Random。

第9-10行：建立另一個映射對象以保存在表單資料模型中定義的輸入參數。

第11行：設定slingRequest屬性&quot;paramMap&quot;

第12-13行：將請求轉發到Servlet

在伺服器上test此功能

* [使用包管理器導入和安裝與本文相關的資產。](assets/launch-agent-ui.zip)
* [登錄到configMgr](http://localhost:4502/system/console/configMgr)
* 搜索 _Adobe花崗岩CSRF濾池_
* 添加 _/content/getprintchannel_ 在排除的路徑中
* 保存更改。
* [開啟POST.jsp](http://localhost:4502/apps/AEMForms/openprintchannel/POST.jsp)。 確保傳遞到FormFieldRequestParameter的字串是有效的documentId。（19號線）
* [開啟網頁](http://localhost:4502/content/OpenPrintChannel.html) 並輸入帳號並提交表格。
* 代理UI介面應開啟，並預先填充特定於表單中輸入的帳號的資料。

>[!NOTE]
>
>確保表單資料模型的「獲取」操作的輸入參數綁定到名為「accountnumber」的請求屬性，以便此操作正常工作。 如果將bindingvalue的名稱更改為任何其他名稱，請確保更改反映在POST.jsp的第25行上
