---
title: 與服務用戶一起發展AEM Forms
description: 本文介紹在AEM Forms建立服務用戶的過程
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: 5fa3d52a-6a71-45c4-9b1a-0e6686dd29bc
source-git-commit: c462d48d26c9a7aa0e4cfc4f24005b41e8e82cb8
workflow-type: tm+mt
source-wordcount: '445'
ht-degree: 0%

---

# 與服務用戶一起發展AEM Forms

本文介紹在AEM Forms建立服務用戶的過程

在以前的Adobe Experience Manager(AEM)版本中，管理資源解析器用於後端處理，需要訪問儲存庫。 6.3中不建議使用管理資源解AEM析程式。而是使用儲存庫中具有特定權限的系統用戶。

瞭解有關 [建立和使用服務用戶AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/advanced/service-users.html)。

本文介紹了系統用戶的建立和用戶映射器屬性的配置。

1. 導航到 [http://localhost:4502/crx/explorer/index.jsp](http://localhost:4502/crx/explorer/index.jsp)
1. 登錄為「 admin 」
1. 按一下「 User Administration 」
1. 按一下「 Create System User 」（建立系統用戶）
1. 將用戶ID類型設定為「 data 」，然後按一下綠色表徵圖以完成建立系統用戶的過程
1. [開啟configMgr](http://localhost:4502/system/console/configMgr)
1. 搜索 _Apache Sling服務用戶映射器服務_ 並按一下以開啟屬性
1. 按一下 *+* 表徵圖（加號）以添加以下服務映射

   * DevelopingWithServiceUser.core:getresourceresolver=data
   * DevelopingWithServiceUser.core:getformsresourceresolver=fd-service

1. 按一下「保存」

在上述配置設定中，DevelopingWithServiceUser.core是捆綁包的符號名稱。 getresourceresolver是子服務名。data是在前一步驟中建立的系統用戶。

還可以代表fd-service用戶獲取資源解析器。 此服務用戶用於文檔服務。 例如，如果要驗證/應用使用權限等，我們將使用fd-service用戶的資源解析程式來執行這些操作

1. [下載並解壓縮與此文章關聯的zip檔案。](assets/developingwithserviceuser.zip)
1. 導航到 [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)
1. 上載並啟動OSGi捆綁包
1. 確保捆綁包處於活動狀態
1. 您現在已成功建立 *系統用戶* 還部署了 *服務用戶捆綁*。

   要提供對/content的訪問，請為系統用戶(「 data 」)授予對內容節點的讀取權限。

   1. 導航到 [http://localhost:4502/useradmin](http://localhost:4502/useradmin)
   1. 搜索用戶「資料」。 這是您在前一步中建立的系統用戶。
   1. 按兩下用戶，然後按一下「權限」頁籤
   1. 為「內容」資料夾授予「讀取」訪問權限。
   1. 要使用服務用戶訪問/content資料夾，請使用以下代碼



```java
com.mergeandfuse.getserviceuserresolver.GetResolver aemDemoListings = sling.getService(com.mergeandfuse.getserviceuserresolver.GetResolver.class);
   
resourceResolver = aemDemoListings.getServiceResolver();
   
// get the resource. This will succeed because we have given ' read ' access to the content node
   
Resource contentResource = resourceResolver.getResource("/content/forms/af/sandbox/abc.pdf");
```

如果要訪問捆綁包中的/content/dam/data.json檔案，將使用以下代碼。 此代碼假定您已為/content/dam/節點上的「data」用戶授予讀取權限

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

實施的完整代碼如下

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
