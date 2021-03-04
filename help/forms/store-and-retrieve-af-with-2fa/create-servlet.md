---
title: 建立servlet
description: 建立servlet以處理保存表單資料的POST請求
feature: 適用性表單
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6539
thumbnail: 6539.pg
topic: 開發
role: 開發人員
level: 經驗豐富
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '88'
ht-degree: 4%

---

# 建立servlet

下一步是建立將呼叫自訂OSGi服務適當方法的servlet。 Servlet可以訪問自適應表單資料、檔案附件資訊。 Servlet將返回唯一的應用程式ID，該ID可用於檢索部分完成的自適應表單。

當用戶按一下最適化表單上的「保存並退出」按鈕時，將調用此servlet

```java
package com.techmarketing.core.servlets;

import java.io.PrintWriter;
import javax.servlet.Servlet;
import javax.sql.DataSource;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.json.JSONObject;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.google.gson.JsonObject;
import store.and.fetch.core.*;

@Component(service = {
    Servlet.class
}, property = {
    "sling.servlet.methods=post",
    "sling.servlet.paths=/bin/storeafdatawithattachments"
})
public class StoreDataInDBWithAttachmentsInfo extends SlingAllMethodsServlet {
    private static final Logger log = LoggerFactory.getLogger(StoreDataInDBWithAttachmentsInfo.class);
    private static final long serialVersionUID = 1 L;
    @Reference(target = "(&(datasource.name=aemformstutorial))")
    private DataSource dataSource;
    @Reference
    AemFormsAndDB aemFormsAndDB;
    protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
        log.debug("#### Inside dopost of StoreDataInDBWithAttachmentsInfo ####");
        String afData = request.getParameter("data");
        String tel = request.getParameter("mobileNumber");
        log.debug("$$$The telephone number is  " + tel);
        log.debug("The request parameter " + afData);
        try {
            JSONObject fileMap = new JSONObject(request.getParameter("fileMap").toString());
            String newFileMap = aemFormsAndDB.storeAFAttachments(fileMap, request);
            String application_id = aemFormsAndDB.storeFormData(afData, newFileMap.toString(), tel);
            JsonObject jsonObject = new JsonObject();
            jsonObject.addProperty("path", application_id);
            response.setContentType("application/json");
            response.setHeader("Cache-Control", "nocache");
            response.setCharacterEncoding("utf-8");
            PrintWriter out = null;
            out = response.getWriter();
            out.println(jsonObject.toString());
        } catch (Exception ex) {
            log.debug(ex.getMessage());
        }
    }
}
```
