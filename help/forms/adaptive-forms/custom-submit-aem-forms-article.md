---
title: 在AEM Forms中撰寫自訂提交
seo-title: 在AEM Forms中撰寫自訂提交
description: 快速輕鬆地為適用性表單建立自訂提交動作
seo-description: 快速輕鬆地為適用性表單建立自訂提交動作
feature: 適用性表單
topics: integrations
audience: developer
doc-type: article
activity: implement
version: 6.3,6.4,6.5
uuid: a26db0b9-7db4-4e80-813d-5c0438fabd1e
discoiquuid: 28611011-2ff9-477e-b654-e62e7374096a
topic: 開發
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 1%

---


# 在AEM Forms中撰寫自訂提交{#writing-a-custom-submit-in-aem-forms}

快速輕鬆地為適用性表單建立自訂提交動作

本文將引導您完成建立自訂提交動作以處理適用性Forms提交所需的步驟。

* 登入crx
* 在應用程式下建立「sling:folder」類型的節點。 我們將此節點命名為CustomSubmitHelpx。
* 保存新建立的節點。
* 將下列兩個屬性新增至新建立的節點
* 屬性名稱       |屬性值
* guideComponentType | fd/af/components/guidesubmittype
* guideDataModel     | xfa,xsd,basic
* jcr:description   |自訂提交幫助
* 儲存變更
* 在CustomSubmitHelpx節點下建立名為post.POST.jsp的新檔案。提交最適化表單時，將調用此JSP。 您可以按照此檔案中的要求編寫JSP代碼。 下列程式碼會將請求轉送至servlet。

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

* 在CustomSubmitHelpx節點下建立名為addfields .jsp的檔案。 此檔案將允許您訪問已簽名的文檔。
* 將下列程式碼新增至此檔案

```java
    <%@include file="/libs/fd/af/components/guidesglobal.jsp"%>

    <%@page import="org.slf4j.LoggerFactory" %>

    <%@page import="org.slf4j.Logger" %>

    <input type="hidden" id="useSignedPdf" name="_useSignedPdf" value=""/>;
```

* 儲存您的變更

現在，您會開始在最適化表單的提交動作中看到「CustomSubmitHelpx」，如此影像所示。

![具有自訂提交的最適化表單](assets/capture-2.gif)

