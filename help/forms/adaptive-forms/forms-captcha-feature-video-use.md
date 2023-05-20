---
title: 將驗證碼與自適應AEMForms一起使用
description: 添加並使用帶自適應AEMForms的驗證碼。
feature: Adaptive Forms,Workflow
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 7e5dcc6e-fe56-49af-97e3-7dfaa9c8738f
last-substantial-update: 2019-06-09T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '204'
ht-degree: 0%

---

# 將驗證碼與自適應AEMForms一起使用{#using-captchas-with-aem-adaptive-forms}

添加並使用帶自適應AEMForms的驗證碼。

>[!VIDEO](https://video.tv.adobe.com/v/18336?quality=12&learn=on)

*這段視頻介紹了使用內置驗證碼服務和AEMGoogle的reCAPTCHA服務將驗證碼添加到自適AEM應表單的過程。*

>[!NOTE]
>
>此功能僅在AEM6.3後可用。

>[!NOTE]
>
>**要在發佈實例上配置reCaptcha，請執行以下步驟**
>
>在作者實例上配置reCaptach
>
>開啟菲力克斯 [Web控制台](http://localhost:4502/system/console/bundles) 關於作者案
>
>搜索com.adobe.granite.crypto.file包
>
>注意束ID。 就我來說，是20
>
>導航到作者實例上檔案系統上的捆綁ID
>
>* &lt;author-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* 複製HMAC和主檔案
>
開啟 [felix Web控制台](http://localhost:4502/system/console/bundles) 在發佈實例上。 搜索com.adobe.granite.crypto.file包。 注意束ID
導航到發佈實例的檔案系統上的捆綁ID
* &lt;publish-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* 刪除現有的HMAC和主檔案。
* 貼上從作者實例複製的HMAC和主檔案
>
重新啟動AEM發佈伺服器

## 支撐材料 {#supporting-materials}

* [GoogleRE CAPTCHA](https://www.google.com/recaptcha)
