---
title: 使用資源型別註冊servlet
description: 在AEM Forms CS中將servlet對應到資源型別
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Developer Tools
jira: KT-14581
duration: 90
exl-id: 2a33a9a9-1eef-425d-aec5-465030ee9b74
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 2%

---

# 簡介

與按資源型別繫結相比，按路徑繫結servlet有幾個缺點，即：

* 無法使用預設JCR存放庫ACL來控制路徑繫結的servlet的存取許可權
* 路徑繫結的servlet只能註冊到路徑而非資源型別（即沒有尾碼處理）
* 如果路徑繫結的servlet不是作用中（例如，如果組合遺失或未啟動），POST可能會導致非預期的結果。 通常在下列位置建立節點： `/bin/xyz` 對於只檢視存放庫的開發人員而言，這會覆蓋繫結對應的servlet路徑鑑於這些缺點，強烈建議將servlet繫結到資源型別而不是路徑

## 建立Servlet

在IntelliJ上啟動您的aem-banking專案。 在servlet資料夾下建立名為GetFieldChoices的servlet，如下方熒幕擷取畫面所示。
![選擇](assets/fetchchoices.png)

## 範例Servlet

以下servlet已繫結至Sling資源型別： _**azure/fetchchoices**_



```java
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.apache.sling.servlets.annotations.SlingServletResourceTypes;
import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.jcr.Session;
import javax.servlet.Servlet;
import java.io.IOException;
import java.io.Serializable;

@Component(
        service={Servlet.class }
)

        @SlingServletResourceTypes(
                resourceTypes="azure/fetchchoices",
                methods= "GET",
                extensions="json"
                )


public class GetFieldChoices extends SlingAllMethodsServlet implements Serializable {
    private static final long serialVersionUID = 1L;
    private final  transient Logger log = LoggerFactory.getLogger(this.getClass());


   

    protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {

        log.debug("The form path I got was "+request.getParameter("formPath"));

    }
}
```

## 在CRX中建立資源

* 登入您的本機AEM SDK。
* 建立名為的資源 `fetchchoices` （您可以隨意命名此節點），型別為 `cq:Page` 在內容節點下。
* 儲存您的變更
* 建立名為的節點 `jcr:content` 型別 `cq:PageContent` 並儲存變更
* 將下列屬性新增至 `jcr:content` 節點

| 屬性名稱 | 屬性值 |
|--------------------|--------------------|
| jcr:title | 公用程式Servlet |
| sling:resourceType | `azure/fetchchoices` |


此 `sling:resourceType` 值必須與servlet中指定的resourceTypes=&quot;azure/fetchchoices相符。

您現在可以透過請求資源來叫用servlet `sling:resourceType` = `azure/fetchchoices` 路徑中任何選取器或副檔名在Sling servlet中註冊。

```html
http://localhost:4502/content/fetchchoices/jcr:content.json?formPath=/content/forms/af/forrahul/jcr:content/guideContainer
```

路徑 `/content/fetchchoices/jcr:content` 是資源和擴充功能的路徑 `.json` 是servlet中指定的內容

## 同步您的AEM專案

1. 在您最愛的編輯器中開啟AEM專案。 我已使用intelliJ來做這件事。
1. 建立名為的資料夾 `fetchchoices` 在 `\aem-banking-application\ui.content\src\main\content\jcr_root\content`
1. 按一下右鍵 `fetchchoices` 資料夾並選取 `repo | Get Command` （此功能表專案是本教學課程上一章所設定）。

這應該會將此節點從AEM同步至您的本機AEM專案。

您的AEM專案結構應如下所示
![resource-resolver](assets/mapping-servlet-resource.png)
使用以下專案更新aem-banking-application\ui.content\src\main\content\META-INF\vault資料夾中的filter.xml

```xml
<filter root="/content/fetchchoices" mode="merge"/>
```

您現在可以使用Cloud Manager將變更推送到AEMas a Cloud Service環境。

## 後續步驟

[啟用Forms Portal元件](./forms-portal-components.md)
