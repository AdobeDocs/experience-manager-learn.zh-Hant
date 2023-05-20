---
title: 在伺服器上部署示例資產
description: 獲取在本地伺服器上工作的使用案例
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
kt: 13099
last-substantial-update: 2023-04-13T00:00:00Z
exl-id: 44f4261b-d6fe-42ad-a3aa-2a36ca897b5e
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '148'
ht-degree: 0%

---

# 部署資產

在AEM Forms發佈伺服器上部署了以下資產/配置。

* [Adobe Sign包裝包](assets/AcrobatSign.core-1.0.0-SNAPSHOT.jar)

* [互動式通信模板示例](assets/waiver-interactive-communication.zip)
* [部署DevelopingWithServiceUser捆綁包](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip)
* 使用OSGi configMgr在Apache Sling服務用戶映射器服務中添加以下條目
   **DevegingWithServiceUser.core:getformsresourceresolver=fd-service**
* [可以從此處下載示例React App代碼](assets/src.zip)



需要在您的本地環境中部署示例反應應用

您必須更改終結點URL以匹配您的環境。 開啟EmercyContact.js檔案並更改讀取方法中的URL

```javascript
 const getWebForm=async()=>
     {
        setSpinner(true)
        console.log("inside widgetURL function emergency contact");
        // NOTE: replace the `aemforms.azure.com:4503` with your AEM FORM server
        let res = await fetch("http://aemforms.azure.com:4503/bin/getwidgeturl",
          {
            method: "POST",
            body: JSON.stringify({"icTemplate":"/content/forms/af/waiver/waiver/channels/print","waiver":formData})
                     
         })
 
```

要啟用從REACT應用對終AEM點進行POST調用，您需要在Adobe花崗岩跨源資源共用策略配置的「允許的源」欄位中指定相應的元素

![cors設定](assets/cors-settings.png)
