---
title: 使用表單資料模型建立促銷活動描述檔
seo-title: 使用表單資料模型建立促銷活動描述檔
description: 使用Adobe Campaign Standard表單資料模型建立AEM Forms描述檔的步驟
seo-description: 使用Adobe Campaign Standard表單資料模型建立AEM Forms描述檔的步驟
uuid: 3216827e-e1a2-4203-8fe3-4e2a82ad180a
feature: adaptive-forms, form-data-model
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 461c532e-7a07-49f5-90b7-ad0dcde40984
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '449'
ht-degree: 2%

---


# 使用表單資料模型{#create-campaign-profile-using-form-data-model}建立促銷活動描述檔

使用Adobe Campaign Standard表單資料模型建立AEM Forms描述檔的步驟

## 建立自訂驗證{#create-custom-authentication}

使用Swagger檔案建立資料源時，AEM Forms支援以下類型的驗證類型

* 無
* OAuth 2.0
* 基本驗證
* API 金鑰
* 自訂驗證

![campafdm](assets/campaignfdm.gif)

我們必須使用自訂驗證才能對Adobe Campaign Standard進行REST呼叫。

要使用自定義身份驗證，我們必須開發實現IAuthentication介面的OSGi元件

需要實作getAuthDetails方法。 此方法將返回AuthenticationDetails對象。 此AuthenticationDetails對象將設定對Adobe Campaign進行REST API調用所需的HTTP標頭。

以下是用於建立自訂驗證的程式碼。 getAuthDetails方法可完成所有工作。 我們建立AuthenticationDetails對象。 然後，我們將適當的HttpHeaders新增至此物件，並傳回此物件。

```java
package aemfd.campaign.core;

import java.io.IOException;
import java.security.NoSuchAlgorithmException;
import java.security.spec.InvalidKeySpecException;

import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.adobe.aemfd.dermis.authentication.api.IAuthentication;
import com.adobe.aemfd.dermis.authentication.exception.AuthenticationException;
import com.adobe.aemfd.dermis.authentication.model.AuthenticationDetails;
import com.adobe.aemfd.dermis.authentication.model.Configuration;

import aemforms.campaign.core.CampaignService;
import formsandcampaign.demo.CampaignConfigurationService;
@Component(service=IAuthentication.class,immediate=true)

public class CampaignAuthentication implements IAuthentication {
 @Reference
 CampaignService campaignService;
  @Reference
     CampaignConfigurationService config;
private Logger log = LoggerFactory.getLogger(CampaignAuthentication.class);
 @Override
 public AuthenticationDetails getAuthDetails(Configuration arg0) throws AuthenticationException {
 try {
   AuthenticationDetails auth = new AuthenticationDetails();
   auth.addHttpHeader("Cache-Control", "no-cache");
   auth.addHttpHeader("Content-Type", "application/json");
   auth.addHttpHeader("X-Api-Key",config.getApiKey() );
         auth.addHttpHeader("Authorization", "Bearer "+campaignService.getAccessToken());
         log.debug("Returning auth");
         return auth;
   
  } catch (NoSuchAlgorithmException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  } catch (InvalidKeySpecException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  } catch (IOException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  return null;
  
 }

 @Override
 public String getAuthenticationType() {
  // TODO Auto-generated method stub
  return "Campaign Custom Authentication";
 }

}
```

## 建立資料源{#create-data-source}

第一步是建立Swagger檔案。 Swagger檔案定義REST API，此API將用於在Adobe Campaign Standard建立描述檔。 swagger檔案定義REST API的輸入參數和輸出參數。

使用swagger檔案建立資料源。 建立資料來源時，您可以指定驗證類型。 在本例中，我們將使用自訂驗證來驗證Adobe Campaign。上面所列的程式碼是用來驗證Adobe Campaign。

範例Swagger檔案會提供給您，做為資產與本文相關的一部分。**請確定您更改swagger檔案中的主機和basePath以匹配您的ACS實例**

## 測試解決方案{#test-the-solution}

若要測試解決方案，請遵循下列步驟：
* [請確定您已依照此處所述的步驟進行](aem-forms-with-campaign-standard-getting-started-tutorial.md)
* [下載並解壓縮此檔案以取得Swagger檔案](assets/create-acs-profile-swagger-file.zip)
* 使用Swagger檔案建立資料來源
建立表單資料模型，並以上一步驟中建立的資料來源為基礎
* 根據在前一步驟中建立的表單資料模型建立自適應表單。
* 將下列元素從資料來源標籤拖放至最適化表單

   * 電子郵件
   * 名字
   * 姓氏
   * 行動電話

* 將提交動作設定為「使用表單資料模型提交」。
* 設定資料模型以適當提交。
* 預覽表格。 填寫欄位並送出。
* 驗證配置式是否在Adobe Campaign Standard建立。
