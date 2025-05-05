---
title: 搭配使用CAPTCHA與AEM Adaptive Forms
description: 透過AEM最適化Forms新增及使用驗證碼。
feature: Adaptive Forms,Workflow
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 7e5dcc6e-fe56-49af-97e3-7dfaa9c8738f
last-substantial-update: 2019-06-09T00:00:00Z
duration: 260
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 0%

---

# 搭配使用CAPTCHA與AEM Adaptive Forms{#using-captchas-with-aem-adaptive-forms}

透過AEM最適化Forms新增及使用驗證碼。

>[!VIDEO](https://video.tv.adobe.com/v/18336?quality=12&learn=on)

*此影片會逐步說明使用內建AEM CAPTCHA服務及AEM reCAPTCHA服務，將驗證碼新增至Google最適化表單的程式。*

>[!NOTE]
>
>此功能僅適用於AEM 6.3以上版本。

>[!NOTE]
>
>**若要在發佈執行個體上設定reCaptcha，請遵循步驟**
>
>在作者執行個體上設定reCaptach
>
>在作者執行個體上開啟Felix [網頁主控台](http://localhost:4502/system/console/bundles)
>
>搜尋com.adobe.granite.crypto.file套件組合
>
>記下套件ID。 在我的執行個體上，這個數字是20
>
>導覽至作者執行個體上檔案系統上的套件ID
>
>* &lt;author-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
>* 複製HMAC和主檔案
>
>開啟發佈執行個體上的[felix web主控台](http://localhost:4502/system/console/bundles)。 搜尋com.adobe.granite.crypto.file套件組合。 記下套件組合ID
>
>導覽至發佈執行個體檔案系統上的套件ID
>
>* &lt;publish-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
>* 刪除現有的HMAC和主檔案。
>* 貼上從製作執行個體複製的HMAC和主檔案
>
>重新啟動AEM發佈伺服器

## 支援材料 {#supporting-materials}

* [Google reCAPTCHA](https://www.google.com/recaptcha)
