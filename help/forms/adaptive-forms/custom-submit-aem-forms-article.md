---
title: 在AEM Forms中撰寫自訂提交
description: 快速輕鬆建立您自己的自訂最適化表單提交動作
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 64b586a6-e9ef-4a3d-8528-55646ab03cc4
last-substantial-update: 2021-04-09T00:00:00Z
duration: 59
source-git-commit: b1734f75bdda174788d880be28fa19f8e787af0a
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 1%

---

# 在AEM Forms中撰寫自訂提交 {#writing-a-custom-submit-in-aem-forms}

快速輕鬆建立您自己的自訂最適化表單提交動作

>[!NOTE]
>本文中的程式碼無法用於以核心元件為基礎的最適化表單。
>[以下提供核心元件型最適化表單的同等文章](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/custom-submit-headless-forms/custom-submit-service.html?lang=en)


本文將逐步引導您執行建立自訂提交動作以處理最適化Forms提交所需的步驟。

* 登入crx
* 在應用程式底下建立「sling：folder」型別的節點。 讓我們呼叫此節點CustomSubmitHelpx。
* 儲存新建立的節點。
* 將下列三個屬性新增至新建立的節點

| 屬性名稱 | 屬性值 |
|----------------    | ---------------------------------|
| guideComponentType | fd/af/components/guidesubmittype |
| guideDataModel | xfa、xsd、basic |
| jcr：description | CustomSubmitHelp |


* 儲存變更
* 在CustomSubmitHelpx節點下建立名為post.js.jsp的新檔案。提交最適化表單時，會呼叫此POST。 您可以視需要在此檔案中撰寫JSP程式碼。 以下程式碼會將請求轉送給servlet。

```java
<%
%><%@include file="/libs/foundation/global.jsp"%>
<%@taglib prefix="cq" uri="http://www.day.com/taglibs/cq/1.0"%>
<%@ page import="org.apache.sling.api.request.RequestParameter,com.day.cq.wcm.api.WCMMode,com.adobe.forms.common.submitutils.CustomParameterRequest,com.adobe.aemds.guide.submitutils.*" %>

<%@ page import="org.apache.sling.api.request.RequestParameter,com.day.cq.wcm.api.WCMMode" %>
<%@page session="false" %>
<%

   com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/storeafsubmission",null,null);

%>
```

* 在CustomSubmitHelpx節點下建立名為addfields .jsp的檔案。 此檔案可讓您存取已簽署的檔案。
* 將下列程式碼新增至此檔案

```java
    <%@include file="/libs/fd/af/components/guidesglobal.jsp"%>

    <%@page import="org.slf4j.LoggerFactory" %>

    <%@page import="org.slf4j.Logger" %>

    <input type="hidden" id="useSignedPdf" name="_useSignedPdf" value=""/>;
```

* 儲存您的變更

現在，您會開始在最適化表單的提交動作中看到「CustomSubmitHelpx」，如本圖所示。

![具有自訂提交的最適化表單](assets/capture-2.gif)
