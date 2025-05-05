---
title: 建立自訂提交服務以處理Headless最適化表單提交
description: 根據提交的資料傳回自訂回應
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-13520
exl-id: c23275d7-daf7-4a42-83b6-4d04b297c470
duration: 115
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '453'
ht-degree: 0%

---

# 建立自訂提交

AEM Forms提供許多立即可用的提交選項，可滿足大部分使用案例。除了這些預先定義的提交動作外，AEM Forms還允許您編寫自己的自訂提交處理常式，以根據您的需求處理表單提交。

若要撰寫自訂提交服務，請遵循下列步驟

## 建立AEM專案

如果您已有現有的AEM Forms as a Cloud Service專案，您可以[跳至撰寫自訂提交服務](#Write-the-custom-submit-service)

* 在您的c磁碟機上建立名為cloudmanager的資料夾。
* 導覽至這個新建立的資料夾
* 在命令提示字元視窗中複製並貼上[此文字檔](./assets/creating-maven-project.txt)的內容。您可能必須根據[最新版本](https://github.com/adobe/aem-project-archetype/releases)變更DarchetypeVersion=41。 撰寫本文時，最新版本是41。
* 按下Enter鍵來執行命令。如果一切順利，您應該會看到建置成功訊息。

## 寫入自訂提交服務{#Write-the-custom-submit-service}

啟動IntelliJ並開啟AEM專案。 建立名為&#x200B;**HandleRegistrationFormSubmission**&#x200B;的新Java類別，如下方熒幕擷取畫面所示
![自訂送出服務](./assets/custom-submit-service.png)

已撰寫下列程式碼來實作服務

```java
package com.aem.bankingapplication.core;
import java.util.HashMap;
import java.util.Map;
import com.google.gson.Gson;
import org.osgi.service.component.annotations.Component;
import com.adobe.aemds.guide.model.FormSubmitInfo;
import com.adobe.aemds.guide.service.FormSubmitActionService;
import com.adobe.aemds.guide.utils.GuideConstants;
import com.google.gson.JsonObject;
import org.slf4j.*;

@Component(
        service=FormSubmitActionService.class,
        immediate = true
)
public class HandleRegistrationFormSubmission implements FormSubmitActionService {
    private static final String serviceName = "Core Custom AF Submit";
    private static Logger logger = LoggerFactory.getLogger(HandleRegistrationFormSubmission.class);



    @Override
    public String getServiceName() {
        return serviceName;
    }

    @Override
    public Map<String, Object> submit(FormSubmitInfo formSubmitInfo) {
        logger.error("in my custom submit service");
        Map<String, Object> result = new HashMap<>();
        logger.error("in my custom submit service");
        String data = formSubmitInfo.getData();
        JsonObject formData = new Gson().fromJson(data,JsonObject.class);
        logger.error("The form data is "+formData);
        JsonObject jsonObject = new JsonObject();
        jsonObject.addProperty("firstName",formData.get("firstName").getAsString());
        jsonObject.addProperty("lastName",formData.get("lastName").getAsString());
        result.put(GuideConstants.FORM_SUBMISSION_COMPLETE, Boolean.TRUE);
        result.put("json",jsonObject.toString());
        return result;
    }

}
```

## 在應用程式下建立crx節點

展開ui.apps節點，在apps節點下建立名稱為&#x200B;**HandleRegistrationFormSubmission**&#x200B;的新套件，如下面的熒幕擷取畫面所示
![crx-node](./assets/crx-node.png)
在&#x200B;**HandleRegistrationFormSubmission**&#x200B;下建立名為.content.xml的檔案。 將下列程式碼複製並貼到.content.xml中

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:sling="http://sling.apache.org/jcr/sling/1.0"
    jcr:description="Handle Registration Form Submission"
    jcr:primaryType="sling:Folder"
    guideComponentType="fd/af/components/guidesubmittype"
    guideDataModel="xfa,xsd,basic"
    submitService="Core Custom AF Submit"/>
```

**submitService**&#x200B;元素的值必須與FormSubmitActionService實作中的&#x200B;**serviceName = &quot;Core Custom AF Submit&quot;**&#x200B;相符。

## 將程式碼部署到您的本機AEM Forms執行個體

將變更推送到Cloud Manager存放庫之前，建議將程式碼部署到本機雲端就緒的作者執行個體以測試程式碼。 確定作者執行個體正在執行。
若要將程式碼部署到您的雲端就緒製作執行個體，請導覽至AEM專案的根資料夾，然後執行下列命令

```
mvn clean install -PautoInstallSinglePackage
```

這會將程式碼當作單一套件部署到您的編寫執行個體

## 將程式碼推送到Cloud Manager並部署程式碼

在本地執行個體上驗證代碼後，將代碼推送至您的雲端執行個體。
將變更推送至本機Git存放庫，然後推送至Cloud Manager存放庫。 您可以參考[Git設定](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/setup-git.html?lang=zh-Hant)、[將AEM專案推送到Cloud Manager存放庫](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/push-project-to-cloud-manager-git.html?lang=zh-Hant)以及[部署到開發環境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/deploy-to-dev-environment.html?lang=zh-Hant)文章。

成功執行管道後，您應該能夠將表單的提交動作關聯到自訂提交處理常式，如下方熒幕擷取所示
![送出動作](./assets/configure-submit-action.png)

## 後續步驟

[在您的react應用程式中顯示自訂回應](./handle-response-react-app.md)
