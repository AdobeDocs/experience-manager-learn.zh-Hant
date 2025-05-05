---
title: 在您的伺服器上部署範例資產
description: 在本機伺服器上取得使用案例
feature: Adaptive Forms,Acrobat Sign
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-13099
last-substantial-update: 2023-04-13T00:00:00Z
exl-id: f12f83fa-673a-454c-aa52-6ea769a182b7
duration: 36
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 0%

---

# 部署資產

下列資產/設定已部署在AEM Forms發佈伺服器上。

* [Adobe Sign包裝函式套裝](assets/AcrobatSign.core-1.0.0-SNAPSHOT.jar)

* [互動式通訊範本範例](assets/waiver-interactive-communication.zip)
* [部署DevelopingWithServiceUser套件](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip?lang=zh-Hant)
* 使用OSGi configMgr在Apache Sling服務使用者對應程式服務中新增以下專案
  **DevelopingWithServiceUser.core：getformsresourceresolver=fd-service**

## 部署範例react應用程式

* [下載範例react應用程式](assets/mult-step-form1.zip)
* 在新資料夾中解壓縮react應用程式的內容
* 導覽至資料夾，然後執行下列命令

```java
npm install
npm start
```

開啟EmergencyContact.js檔案，並變更擷取方法中的URL以符合您的環境。


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

若要啟用從REACT應用程式對AEM端點進行POST呼叫，您需要在Adobe Granite跨原始資源共用原則設定的「允許的原始項」欄位中指定適當的專案。

![cors-setting](assets/cors-settings.png)

如需CORS組態選項的詳細資訊，請參閱[透過AEM瞭解CORS](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html?lang=zh-Hant)。
