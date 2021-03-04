---
title: 與服務用戶一起發展AEM Forms
seo-title: 與服務用戶一起發展AEM Forms
description: 本文將帶您瞭解在AEM Forms建立服務使用者的程式
seo-description: 本文將帶您瞭解在AEM Forms建立服務使用者的程式
uuid: 996f30df-3fc5-4232-a104-b92e1bee4713
feature: 適用性表單
topics: development,administration
audience: implementer,developer
doc-type: article
activity: setup
discoiquuid: 65bd4695-e110-48ba-80ec-2d36bc53ead2
topic: 開發
role: 開發人員
level: 經驗豐富
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '451'
ht-degree: 0%

---


# 與服務用戶一起發展AEM Forms

本文將帶您瞭解在AEM Forms建立服務使用者的程式

在舊版Adobe Experience Manager(AEM)中，管理資源解析器用於後端處理，後端處理需要訪問儲存庫。 6.3中不建議使用管理資AEM源解析器。而是使用在儲存庫中具有特定權限的系統用戶。

本文將逐步介紹系統用戶的建立過程和用戶映射器屬性的配置。

1. 導覽至[http://localhost:4502/crx/explorer/index.jsp](http://localhost:4502/crx/explorer/index.jsp)
1. 以「 admin 」身份登錄
1. 按一下「 User Administration 」（用戶管理）
1. 按一下「 Create System User 」（建立系統用戶）
1. 將用戶ID類型設定為「 data 」，然後按一下綠色表徵圖以完成建立系統用戶的過程
1. [開啟configMgr](http://localhost:4502/system/console/configMgr)
1. 搜尋「 Apache Sling Service User Mapper Service」，然後按一下以開啟屬性
1. 按一下&#x200B;*+*&#x200B;表徵圖（加號）添加以下服務映射

   * DevelopingWithServiceUser.core:getresourceresolver=data
   * DevelopingWithServiceUser.core:getformsresourceresolver=fd-service

1. 按一下「保存」

在上述組態設定中，DevelopingWithServiceUser.core是套件的符號名稱。 getresourceresolver是子服務名。data是在前面步驟中建立的系統用戶。

我們還可以代表fd-service用戶獲取資源解析器。 此服務用戶用於文檔服務。 例如，如果您想要認證／套用使用權等，我們將使用fd-service使用者的資源解析程式來執行操作

1. [下載並解壓縮與此文章相關的zip檔案。](assets/developingwithserviceuser.zip)
1. 導覽至[http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)
1. 上傳並啟動OSGi套件
1. 確保包處於活動狀態
1. 您現在已成功建立&#x200B;*系統用戶*&#x200B;並且部署了&#x200B;*服務用戶包*。

   若要提供對/content的存取權，請授予系統使用者（「資料」）對內容節點的讀取權限。

   1. 導覽至[http://localhost:4502/useradmin](http://localhost:4502/useradmin)
   1. 搜索用戶「 data 」。 這是您在先前步驟中建立的系統使用者。
   1. 連按兩下使用者，然後按一下「權限」標籤
   1. 授予「已讀取」對「內容」資料夾的訪問權限。
   1. 若要使用服務使用者存取/content資料夾，請使用下列程式碼

   ```java
   com.mergeandfuse.getserviceuserresolver.GetResolver aemDemoListings = sling.getService(com.mergeandfuse.getserviceuserresolver.GetResolver.class);
   
   resourceResolver = aemDemoListings.getServiceResolver();
   
   // get the resource. This will succeed because we have given ' read ' access to the content node
   
   Resource contentResource = resourceResolver.getResource("/content/forms/af/sandbox/abc.pdf");
   ```

   如果您想要存取套件中的/content/dam/data.json檔案，請使用下列程式碼。 此程式碼假設您已為/content/dam/節點上的「data」使用者提供讀取權限

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

