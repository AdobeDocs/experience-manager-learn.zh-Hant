---
title: 搭配AEM Adaptive Forms使用CAPTCHA
seo-title: 搭配AEM Adaptive Forms使用CAPTCHA
description: 新增和使用AEM Adaptive Forms的CAPTCHA。
seo-description: 新增和使用AEM Adaptive Forms的CAPTCHA。
feature: adaptive-forms
topics: integrations
audience: developer
doc-type: technical video
activity: setup
version: 6.4,6.5
uuid: bd63e207-4f4d-4f34-9ac4-7572ed26f646
discoiquuid: 5e184e44-e385-4df7-b7ed-085239f2a642
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '242'
ht-degree: 0%

---


# 搭配AEM Adaptive Forms使用CAPTCHA{#using-captchas-with-aem-adaptive-forms}

新增和使用AEM Adaptive Forms的CAPTCHA。

請造訪[AEM Forms範例](https://forms.enablementadobe.com/content/samples/samples.html?query=0)頁面，以取得此功能的即時示範連結。

>[!VIDEO](https://video.tv.adobe.com/v/18336/?quality=9&learn=on)

*此視訊會逐步說明如何使用內建的AEM CAPTCHA服務以及Google的reCAPTCHA服務，將CAPTCHA新增至AEM Adaptive Form。*

>[!NOTE]
>
>此功能僅適用於AEM 6.3以上版本。

>[!NOTE]
>
>**若要在發佈例項上設定reCaptcha，請遵循步驟**
>
>在作者例項上設定reCaptach
>
>在作者實例上開啟felix [web console](http://localhost:4502/system/console/bundles)
>
>搜尋com.adobe.granite.crypto.file套裝
>
>請注意bundle id。 我的例子是20
>
>導覽至您作者例項上檔案系統上的Bundle ID
>
>* &lt;author-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* 複製HMAC和主檔案

在您的發佈例項上開啟[felix web console](http://localhost:4502/system/console/bundles)。 搜尋com.adobe.granite.crypto.file套件。 請注意Bundle id
導覽至您發佈例項之檔案系統上的Bundle ID
* &lt;publish-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* 刪除現有的HMAC和主檔案。
* 貼上從作者實例複製的HMAC和主檔案

重新啟動您的AEM發佈伺服器

## 支援材料{#supporting-materials}

* [Google reCAPTCHA](https://www.google.com/recaptcha)

