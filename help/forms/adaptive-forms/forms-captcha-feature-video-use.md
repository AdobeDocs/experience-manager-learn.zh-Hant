---
title: 搭配AEM適用性Forms使用驗證碼
description: 新增及使用CAPTCHA與AEM適用性Forms。
feature: Adaptive Forms,Workflow
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 7e5dcc6e-fe56-49af-97e3-7dfaa9c8738f
source-git-commit: 9529b1f6d1a863fc570822c8ecd6c4be01b36729
workflow-type: tm+mt
source-wordcount: '204'
ht-degree: 0%

---

# 搭配AEM適用性Forms使用驗證碼{#using-captchas-with-aem-adaptive-forms}

新增及使用CAPTCHA與AEM適用性Forms。

>[!VIDEO](https://video.tv.adobe.com/v/18336/?quality=9&learn=on)

*本影片會逐步說明如何使用內建的AEM CAPTCHA服務以及Google的reCAPTCHA服務，將驗證碼新增至AEM適用性表單。*

>[!NOTE]
>
>此功能僅適用於AEM 6.3以上版本。

>[!NOTE]
>
>**若要在發佈執行個體上設定reCaptcha，請遵循步驟**
>
>在製作執行個體上設定reCaptach
>
>開啟Felix [Web主控台](http://localhost:4502/system/console/bundles) 在author例項上
>
>搜尋com.adobe.granite.crypto.file套件
>
>記下套件ID。 就我而言，是20
>
>導覽至作者例項上檔案系統上的套件ID
>
>* &lt;author-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* 複製HMAC和主檔案
開啟 [felix web console](http://localhost:4502/system/console/bundles) 在您的發佈例項上。 搜尋com.adobe.granite.crypto.file套件組合。 請注意套件ID
導覽至發佈執行個體之檔案系統上的套件ID
* &lt;publish-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* 刪除現有的HMAC和主檔案。
* 貼上從製作例項複製的HMAC和主檔案

重新啟動AEM發佈伺服器

## 支援材料 {#supporting-materials}

* [Google reCAPTCHA](https://www.google.com/recaptcha)
