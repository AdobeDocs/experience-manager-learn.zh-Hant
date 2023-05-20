---
title: 在AEM Forms撰寫自定義提交
description: 快速、簡便地為自適應表單建立您自己的自定義提交操作
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 64b586a6-e9ef-4a3d-8528-55646ab03cc4
last-substantial-update: 2021-04-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '206'
ht-degree: 0%

---

# 在AEM Forms撰寫自定義提交 {#writing-a-custom-submit-in-aem-forms}

快速、簡便地為自適應表單建立您自己的自定義提交操作

本文將引導您完成建立自定義提交操作以處理自適應Forms提交所需的步驟。

* 登錄到crx
* 在應用下建立「sling :folder 」類型的節點。 我們將此節點命名為CustomSubmitHelpx。
* 保存新建立的節點。
* 將以下兩個屬性添加到新建立的節點
* 屬性名稱 |屬性值
* guideComponentType | fd/af/components/guidesubmittype
* guideDataModel | xfa,xsd,basic
* jcr：說明 |自定義提交幫助
* 保存更改
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

* 在CustomSubmitHelpx節點下建立名為addfields .jsp的檔案。 此檔案將允許您訪問已簽名的文檔。
* 將以下代碼添加到此檔案

```java
    <%@include file="/libs/fd/af/components/guidesglobal.jsp"%>

    <%@page import="org.slf4j.LoggerFactory" %>

    <%@page import="org.slf4j.Logger" %>

    <input type="hidden" id="useSignedPdf" name="_useSignedPdf" value=""/>;
```

* 保存更改

現在，您將開始在自適應表單的提交操作中看到「CustomSubmitHelpx」，如此影像所示。

![自定義提交的自適應表單](assets/capture-2.gif)
