---
title: 搭配AEM適用性Forms使用驗證碼
description: 新增及使用CAPTCHA與AEM適用性Forms。
feature: 適用性Forms，工作流程
version: 6.4,6.5
topic: 開發
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 0%

---


# 搭配AEM適用性Forms使用驗證碼{#using-captchas-with-aem-adaptive-forms}

新增及使用CAPTCHA與AEM適用性Forms。

請造訪[AEM Forms範例](https://forms.enablementadobe.com/content/samples/samples.html?query=0#collapse1)頁面，以取得此功能的即時示範連結。

>[!VIDEO](https://video.tv.adobe.com/v/18336/?quality=9&learn=on)

*此影片會逐步說明如何使用內建的AEM CAPTCHA服務以及Google reCAPTCHA服務，將驗證碼新增至AEM適用性表單。*

>[!NOTE]
>
>此功能僅適用於AEM 6.3以上版本。

>[!NOTE]
>
>**若要在發佈執行個體上設定reCaptcha，請遵循步驟**
>
>在製作執行個體上設定reCaptach
>
>在製作執行個體上開啟Felix [Web主控台](http://localhost:4502/system/console/bundles)
>
>搜尋com.adobe.granite.crypto.file套件
>
>記下套件ID。 就我而言，是20
>
>導覽至作者例項上檔案系統上的套件ID
>
>* &lt;author-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* 複製HMAC和主檔案

在您的發佈執行個體上開啟[felix web主控台](http://localhost:4502/system/console/bundles)。 搜尋com.adobe.granite.crypto.file套件組合。 請注意套件ID
導覽至發佈執行個體之檔案系統上的套件ID
* &lt;publish-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* 刪除現有的HMAC和主檔案。
* 貼上從製作例項複製的HMAC和主檔案

重新啟動AEM發佈伺服器

## 支援材料 {#supporting-materials}

* [Google reCAPTCHA](https://www.google.com/recaptcha)

