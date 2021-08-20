---
title: 與服務使用者一起開發AEM Forms
description: 本文會逐步帶您了解在AEM Forms中建立服務使用者的程式
feature: 適用性表單
topic: 開發
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 0%

---


# 與服務使用者一起開發AEM Forms

本文會逐步帶您了解在AEM Forms中建立服務使用者的程式

在舊版Adobe Experience Manager(AEM)中，管理資源解析器用於後端處理，需要存取存放庫。 AEM 6.3不建議使用管理資源解析器，而是使用在存放庫中具有特定權限的系統使用者。

本文逐步說明系統使用者的建立，以及使用者對應程式屬性的設定。

1. 導覽至[http://localhost:4502/crx/explorer/index.jsp](http://localhost:4502/crx/explorer/index.jsp)
1. 以「 admin 」身份登錄
1. 按一下「用戶管理」
1. 按一下「建立系統用戶」
1. 將userid類型設定為「 data 」，然後按一下綠色表徵圖以完成建立系統用戶的過程
1. [開啟configMgr](http://localhost:4502/system/console/configMgr)
1. 搜尋「 Apache Sling Service User Mapper Service 」，然後按一下以開啟屬性
1. 按一下&#x200B;*+*&#x200B;圖示（加號）以新增下列服務對應

   * DevelopingWithServiceUser.core:getresourceresolver=data
   * DevelopingWithServiceUser.core:getformsresourceresolver=fd-service

1. 按一下「保存」

在上述配置設定中，DevelopingWithServiceUser.core是捆綁包的符號名稱。 getresourceresolver是子服務名稱。data是先前步驟中建立的系統使用者。

我們也可以代表fd-service使用者取得資源解析器。 此服務用戶用於文檔服務。 例如，如果您想要認證/套用使用權等，我們將使用fd-service使用者的資源解析器來執行操作

1. [下載並解壓縮與本文相關聯的zip檔案。](assets/developingwithserviceuser.zip)
1. 導覽至[http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)
1. 上傳並啟動OSGi套件組合
1. 確保該捆綁處於活動狀態
1. 您現在已成功建立&#x200B;*系統用戶*&#x200B;並部署了&#x200B;*服務用戶包*。

   要提供對/content的訪問，請為系統用戶（「資料」）授予對內容節點的讀取權限。

   1. 導覽至[http://localhost:4502/useradmin](http://localhost:4502/useradmin)
   1. 搜索用戶「資料」。 這是您在先前步驟中建立的相同系統使用者。
   1. 按兩下該用戶，然後按一下「 Permissions 」頁簽
   1. 授予「讀取」對「內容」資料夾的訪問權限。
   1. 若要使用服務使用者取得/content資料夾的存取權，請使用下列程式碼

   ```java
   com.mergeandfuse.getserviceuserresolver.GetResolver aemDemoListings = sling.getService(com.mergeandfuse.getserviceuserresolver.GetResolver.class);
   
   resourceResolver = aemDemoListings.getServiceResolver();
   
   // get the resource. This will succeed because we have given ' read ' access to the content node
   
   Resource contentResource = resourceResolver.getResource("/content/forms/af/sandbox/abc.pdf");
   ```

   如果您想要存取套件中的/content/dam/data.json檔案，請使用下列程式碼。 此程式碼假設您已為/content/dam/節點上的「data」使用者授予讀取權限

   ```java
   @Reference
   GetResolver getResolver;
   .
   .
   .
   ResourceResolver serviceResolver = getResolver.getServiceResolver();
   // to get resource resolver specific to fd-service user. This is for Document Services
   ResourceResolver fdserviceResolver = getResolver.getFormsServiceResolver();
   Node resNode = getResolver.getServiceResolver().getResource("/content/dam/data.json").adaptTo(Node.class);
   ```

   實作的完整程式碼如下

   ```java
   package com.mergeandfuse.getserviceuserresolver.impl;
   
   import java.util.HashMap;
   
   import org.apache.sling.api.resource.LoginException;
   import org.apache.sling.api.resource.ResourceResolver;
   import org.apache.sling.api.resource.ResourceResolverFactory;
   import org.osgi.service.component.annotations.Component;
   import org.osgi.service.component.annotations.Reference;
   import com.mergeandfuse.getserviceuserresolver.GetResolver;
   
   @Component(service = GetResolver.class)
   public class GetResolverImpl implements GetResolver {
    @Reference
    ResourceResolverFactory resolverFactory;
    @Override
    public ResourceResolver getServiceResolver() {
     HashMap<String, Object> param = new HashMap<String, Object>();
     param.put(ResourceResolverFactory.SUBSERVICE, "getresourceresolver");
     ResourceResolver resolver = null;
     try {
      resolver = resolverFactory.getServiceResourceResolver(param);
     } catch (LoginException e) {
      // TODO Auto-generated catch block
      e.printStackTrace();
     }
     return resolver;
    }
   ```

