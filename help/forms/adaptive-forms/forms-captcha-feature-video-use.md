---
title: 搭配最適化FormsAEM使用CAPTCHA
seo-title: 搭配最適化FormsAEM使用CAPTCHA
description: 新增並使用具備最適化FormsAEM的驗證碼。
seo-description: 新增並使用具備最適化FormsAEM的驗證碼。
feature: Adaptive Forms,Workflow
topics: integrations
audience: developer
doc-type: technical video
activity: setup
version: 6.4,6.5
uuid: bd63e207-4f4d-4f34-9ac4-7572ed26f646
discoiquuid: 5e184e44-e385-4df7-b7ed-085239f2a642
topic: Development
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 0%

---


# 將CAPTCHA與最適AEM化Forms一起使用{#using-captchas-with-aem-adaptive-forms}

新增並使用具備最適化FormsAEM的驗證碼。

請造訪[AEM Forms範例](https://forms.enablementadobe.com/content/samples/samples.html?query=0)頁面，以取得此功能的即時示範連結。

>[!VIDEO](https://video.tv.adobe.com/v/18336/?quality=9&learn=on)

*這段視訊會逐步說明如何使用內建的AEMCAPTCHA服務以及Google reCAPTCHA服務，將AEMCAPTCHA新增至Adaptive Form。*

>[!NOTE]
>
>此功能僅適用於AEM6.3版以上版本。

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

重新啟AEM動發佈伺服器

## 支援材料{#supporting-materials}

* [Google reCAPTCHA](https://www.google.com/recaptcha)

