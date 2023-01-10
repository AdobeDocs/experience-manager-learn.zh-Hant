---
title: 新增DAM資料夾項目至選擇的群組元件
description: 動態新增項目至選擇的群組元件
feature: Adaptive Forms
version: 6.5
topic: Development
role: User
level: Beginner
last-substantial-update: 2023-01-01T00:00:00Z
source-git-commit: a2bbb26751c9182056b4fe6d36eeeec964001df8
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 0%

---

# 動態新增項目至選擇的群組元件

AEM Forms 6.5導入了動態新增項目至最適化Forms選擇群組元件（例如CheckBox、選項按鈕和影像清單）的功能。 在本文中，我們將說明使用DAM資料夾內容填入選擇群組元件的使用案例。 在螢幕擷取畫面中，3個檔案位於名為newsletter的資料夾中。每次將新新聞稿新增至資料夾時，選擇群組元件就會更新，以自動列出其內容。 使用者可以選取一或多個要下載的電子報。

![規則編輯器](assets/newsletters-download.png)

## 建立Servlet以傳回DAM資料夾內容

編寫下列程式碼以傳回JSON格式的DAM資料夾內容。

```java
package com.newsletters.core.servlets;
import static com.day.cq.commons.jcr.JcrConstants.JCR_CONTENT;
import java.io.IOException;
import java.io.PrintWriter;
import java.util.ArrayList;
import java.util.List;
import javax.servlet.Servlet;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.servlets.SlingSafeMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.google.gson.Gson;
import com.google.gson.JsonObject;

@Component(service = {
  Servlet.class
}, property = {
  "sling.servlet.methods=get",
  "sling.servlet.paths=/bin/listfoldercontents"
})
public class ListFolderContent extends SlingSafeMethodsServlet {
  private static final long serialVersionUID = 1 L;
  private static final Logger log = LoggerFactory.getLogger(ListFolderContent.class);
  protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
    Resource resource = request.getResourceResolver().getResource(request.getParameter("damFolder"));
    List < JsonObject > results = new ArrayList < > ();
    resource.getChildren().forEach(child -> {
      if (!JCR_CONTENT.equals(child.getName())) {
        JsonObject asset = new JsonObject();
        log.debug("##The child name is " + child.getName());
        asset.addProperty("assetname", child.getName());
        asset.addProperty("assetpath", child.getPath());
        results.add(asset);

      }
    });
    PrintWriter out = null;
    try {
      out = response.getWriter();
    } catch (IOException e) {

      log.debug(e.getMessage());
    }
    response.setContentType("application/json");
    response.setCharacterEncoding("UTF-8");
    Gson gson = new Gson();
    out.print(gson.toJson(results));
    out.flush();
  }

}
```

## 使用JavaScript函式建立用戶端程式庫

會從JavaScript函式叫用Servlet。 函式返回一個陣列對象，該對象將用於填充選擇組元件

```javascript
/**
 * Populate drop down/choice group  with assets from specified folder
 * @return {string[]} 
 */
function getDAMFolderAssets(damFolder) {
   // strUrl is whatever URL you need to call
   var strUrl = '/bin/listfoldercontents?damFolder=' + damFolder;
   var documents = [];
   $.ajax({
      url: strUrl,
      success: function(jsonData) {
         for (i = 0; i < jsonData.length; i++) {
            documents.push(jsonData[i].assetpath + "=" + jsonData[i].assetname);
         }
      },
      async: false
   });
   return documents;
}
```

## 建立最適化表單

建立最適化表單並將表單與用戶端程式庫建立關聯 **listfolderassets**. 將核取方塊元件新增至表單。 使用規則編輯器填入核取方塊的選項，如螢幕擷取畫面所示
![set-options](assets/set-options-newsletter.png)

我們正在叫用名為 **getDAMFolderAssets** 並傳遞DAM資料夾資產的路徑以在表單中列出。