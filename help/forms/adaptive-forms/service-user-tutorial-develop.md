---
title: 在AEM Forms中使用服務進行開發
description: 本文將逐步說明在AEM Forms中建立服務使用者的程式
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: 5fa3d52a-6a71-45c4-9b1a-0e6686dd29bc
last-substantial-update: 2020-09-09T00:00:00Z
duration: 129
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '415'
ht-degree: 0%

---

# 在AEM Forms中使用服務進行開發

本文將逐步說明在AEM Forms中建立服務使用者的程式

在舊版Adobe Experience Manager (AEM)中，管理資源解析器用於需要存取存放庫的後端處理。 AEM 6.3不建議使用管理資源解析程式。而是使用存放庫中具有特定許可權的系統使用者。

深入瞭解[在AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/advanced/service-users.html?lang=zh-Hant)中建立及使用服務使用者的詳細資訊。

本文將逐步說明如何建立系統使用者及設定使用者對應程式屬性。

1. 導覽至[http://localhost:4502/crx/explorer/index.jsp](http://localhost:4502/crx/explorer/index.jsp)
1. 以「管理員」身分登入
1. 按一下「使用者管理」
1. 按一下「建立系統使用者」
1. 將使用者ID型別設定為「 data 」，然後按一下綠色圖示以完成建立系統使用者的程式
1. [開啟configMgr](http://localhost:4502/system/console/configMgr)
1. 搜尋&#x200B;_Apache Sling Service使用者對應程式服務_，然後按一下以開啟屬性
1. 按一下&#x200B;*+*&#x200B;圖示（加號）以新增下列服務對應

   * DevelopingWithServiceUser.core:getresourceresolver=data
   * DevelopingWithServiceUser.core:getformsresourceresolver=fd-service

1. 按一下「儲存」

在上述組態設定中，DevelopingWithServiceUser.core是套件的符號名稱。 getresourceresolver是子服務名稱。data是在先前步驟中建立的系統使用者。

我們也可以代表fd-service使用者取得資源解析程式。 此服務使用者用於檔案服務。 例如，如果您想要認證/套用使用許可權等，我們將使用fd-service使用者的資源解析器來執行作業

1. [下載並解壓縮與本文相關的zip檔案。](assets/developingwithserviceuser.zip)
1. 導覽至[http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)
1. 上傳並啟動OSGi套件組合
1. 確保套件組合處於作用中狀態
1. 您現在已成功建立&#x200B;*系統使用者*，並且已部署&#x200B;*服務使用者套件*。

   若要提供/content的存取權，請授予系統使用者（&#39;資料&#39;）內容節點的讀取許可權。

   1. 瀏覽至[http://localhost:4502/useradmin](http://localhost:4502/useradmin)
   1. 搜尋使用者「 data 」。 這是您在先前步驟中建立的相同系統使用者。
   1. 連按兩下使用者，然後按一下「許可權」索引標籤
   1. 將&#39;read&#39;存取權授予&#39;content&#39;資料夾。
   1. 若要使用服務使用者來存取/content資料夾，請使用下列程式碼



```java
com.mergeandfuse.getserviceuserresolver.GetResolver aemDemoListings = sling.getService(com.mergeandfuse.getserviceuserresolver.GetResolver.class);
   
resourceResolver = aemDemoListings.getServiceResolver();
   
// get the resource. This will succeed because we have given ' read ' access to the content node
   
Resource contentResource = resourceResolver.getResource("/content/forms/af/sandbox/abc.pdf");
```

如果您想要存取套件組合中的/content/dam/data.json檔案，請使用下列程式碼。 此程式碼假設您已授予/content/dam/節點上「資料」使用者的讀取許可權

```java
@Reference
GetResolver getResolver;
.
.
.
try {
   ResourceResolver serviceResolver = getResolver.getServiceResolver();

   // To get resource resolver specific to fd-service user. This is for Document Services
   ResourceResolver fdserviceResolver = getResolver.getFormsServiceResolver();

   Node resNode = getResolver.getServiceResolver().getResource("/content/dam/data.json").adaptTo(Node.class);
} catch(LoginException ex) {
   // Unable to get the service user, handle this exception as needed
} finally {
  // Always close all ResourceResolvers you open!
  
  if (serviceResolver != null( { serviceResolver.close(); }
  if (fdserviceResolver != null) { fdserviceResolver.close(); }
}
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
                System.out.println("#### Trying to get service resource resolver ....  in my bundle");
                HashMap < String, Object > param = new HashMap < String, Object > ();
                param.put(ResourceResolverFactory.SUBSERVICE, "getresourceresolver");
                ResourceResolver resolver = null;
                try {
                        resolver = resolverFactory.getServiceResourceResolver(param);
                } catch (LoginException e) {

                        System.out.println("Login Exception " + e.getMessage());
                }
                return resolver;

        }

        @Override
        public ResourceResolver getFormsServiceResolver() {

                System.out.println("#### Trying to get Resource Resolver for forms ....  in my bundle");
                HashMap < String, Object > param = new HashMap < String, Object > ();
                param.put(ResourceResolverFactory.SUBSERVICE, "getformsresourceresolver");
                ResourceResolver resolver = null;
                try {
                        resolver = resolverFactory.getServiceResourceResolver(param);
                } catch (LoginException e) {
                        System.out.println("Login Exception ");
                        System.out.println("The error message is " + e.getMessage());
                }
                return resolver;
        }

}
```
