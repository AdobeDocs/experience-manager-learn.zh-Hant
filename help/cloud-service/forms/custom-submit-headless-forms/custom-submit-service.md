---
title: 建立自訂提交服務以處理Headless最適化表單提交
description: 根據提交的資料傳回自訂回應
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 13520
source-git-commit: 2dceb4dd4ee1079c100c9cbca94332d61d17ef57
workflow-type: tm+mt
source-wordcount: '474'
ht-degree: 0%

---


# 建立自訂提交

AEM Forms提供許多立即可用的提交選項，可滿足大部分的使用案例。除了這些預先定義的提交動作外，AEM Forms還允許您撰寫自己的自訂提交處理常式，以根據您的需求處理表單提交。

若要撰寫自訂提交服務，請遵循下列步驟

## 建立AEM專案

如果您已有現有的AEM FormsCloud Service專案，您可以 [跳至寫入自訂提交服務](#Write-the-custom-submit-service)

* 在您的c磁碟機上建立名為cloudmanager的資料夾。
* 導覽至這個新建立的資料夾
* 複製並貼上內容 [此文字檔](./assets/creating-maven-project.txt) 命令提示字元視窗中。您可能需要根據 [最新版本](https://github.com/adobe/aem-project-archetype/releases). 撰寫本文時，最新版本是41。
* 按Enter鍵執行命令。如果一切順利，您應該會看到建置成功訊息。

## 寫入自訂提交服務{#Write-the-custom-submit-service}

啟動IntelliJ並開啟AEM專案。 建立名為的新Java類別 **HandleRegistrationFormSubmit** 如下方熒幕擷圖所示
![custom-submit-service](./assets/custom-submit-service.png)

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

展開ui.apps節點，建立名為的新套件 **HandleRegistrationFormSubmit** 在「應用程式」節點下，如下方熒幕擷取畫面所示
![crx-node](./assets/crx-node.png)
在.content.xml下建立檔案 **HandleRegistrationFormSubmit**. 將下列程式碼複製並貼到.content.xml中

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:sling="http://sling.apache.org/jcr/sling/1.0"
    jcr:description="Handle Registration Form Submission"
    jcr:primaryType="sling:Folder"
    guideComponentType="fd/af/components/guidesubmittype"
    guideDataModel="xfa,xsd,basic"
    submitService="Core Custom AF Submit"/>
```

的值 **submitService** 元素必須相符  **serviceName = &quot;核心自訂AF提交&quot;** 在FormSubmitActionService實作中。

## 將程式碼部署到您的本機AEM Forms執行個體

將變更推送到Cloud Manager存放庫之前，建議將程式碼部署到本機雲端就緒的作者執行個體，以測試程式碼。 請確定作者執行個體正在執行。
若要將程式碼部署到您的雲端就緒編寫執行個體，請導覽至AEM專案的根資料夾，然後執行下列命令

```
mvn clean install -PautoInstallSinglePackage
```

這會將程式碼部署為單一套件到您的編寫執行個體

## 將程式碼推送到Cloud Manager並部署程式碼

在本地執行個體上驗證代碼後，將代碼推送至您的雲端執行個體。
將變更推送至本機Git存放庫，然後推送至Cloud Manager存放庫。 您可參閱  [Git設定](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/setup-git.html)， [將AEM專案推送到cloud manager存放庫](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/push-project-to-cloud-manager-git.html) 和 [部署到開發環境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/deploy-to-dev-environment.html) 文章。

成功執行管道後，您應該能夠將表單的提交動作與自訂提交處理常式建立關聯，如下方熒幕擷取畫面所示
![submit-action](./assets/configure-submit-action.png)

## 後續步驟

[在您的react應用程式中顯示自訂回應](./handle-response-react-app.md)











