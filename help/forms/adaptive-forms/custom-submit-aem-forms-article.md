---
title: 在AEM Forms中撰寫自訂提交
seo-title: 在AEM Forms中撰寫自訂提交
description: 快速且簡單的方式，為最適化表單建立自訂的提交動作
seo-description: 快速且簡單的方式，為最適化表單建立自訂的提交動作
feature: adaptive-forms
topics: integrations
audience: developer
doc-type: article
activity: implement
version: 6.3,6.4,6.5
uuid: a26db0b9-7db4-4e80-813d-5c0438fabd1e
discoiquuid: 28611011-2ff9-477e-b654-e62e7374096a
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 0%

---


# 在AEM Forms中撰寫自訂提交 {#writing-a-custom-submit-in-aem-forms}

快速且簡單的方式，為最適化表單建立自訂的提交動作

本文將引導您完成建立自訂提交動作以處理最適化表單提交所需的步驟。

* 登入crx
* 在應用程式下建立類型為&quot;sling :folder &quot;的節點。 我們將此節點稱為CustomSubmitHelpx。
* 保存新建立的節點。
* 將以下兩個屬性添加到新建立的節點
* 屬性名稱       |屬性值
* guideComponentType | fd/af/components/guidesubmittype
* guideDataModel     | xfa,xsd,basic
* jcr:description   | CustomSubmitHelpx
* 儲存變更
* 在CustomSubmitHelpx節點下建立一個名為post.POST.jsp的新檔案。提交自適應表單時，將調用此JSP。 您可以根據需要在此檔案中編寫JSP代碼。 以下代碼將請求轉發到servlet。

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

* 儲存變更

現在，您將開始在最適化表單的提交動作中看到「CustomSubmitHelpx」，如此圖所示。

![自訂提交的最適化表單](assets/capture-2.gif)

