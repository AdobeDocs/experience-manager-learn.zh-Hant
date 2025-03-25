---
title: 使用表單資料模型發佈二進位資料
description: 使用表單資料模型將二進位資料發佈至AEM DAM
feature: Workflow
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 9c62a7d6-8846-424c-97b8-2e6e3c1501ec
last-substantial-update: 2021-01-09T00:00:00Z
duration: 95
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 0%

---

# 使用表單資料模型發佈二進位資料{#using-form-data-model-to-post-binary-data}

從AEM Forms 6.4開始，我們現在能叫用表單資料模型服務，作為AEM工作流程中的步驟。 本文將逐步引導您瞭解使用表單資料模型服務張貼記錄檔案的範例使用案例。

使用案例如下：

1. 使用者填寫並提交最適化表單。
1. 最適化表單已設定為產生記錄檔案。
1. 提交此最適化表單時，會觸發AEM工作流程，進而使用叫用表單資料模型服務將記錄檔案發佈至AEM DAM。

![posttodam](assets/posttodamshot1.png)

表單資料模型標籤 — 屬性

在「服務輸入」標籤中，我們對映下列專案

* 相對於承載具有DOR.pdf屬性的檔案（需要儲存的二進位物件）。 這表示在提交最適化表單時，產生的記錄檔案會儲存在名為DOR.pdf的檔案中，且與工作流程裝載相關。**請確定此DOR.pdf與您在設定最適化表單的提交屬性時所提供的相同。**

* fileName — 這是用來在DAM中儲存二進位物件的名稱。 因此您想要此屬性動態產生，以便每個提交的fileName都是唯一的。 為此目的，我們使用工作流程中的程式步驟來建立稱為filename的中繼資料屬性，並將其值設定為提交表單之人員的「成員名稱」和「帳號」的組合。 例如，若人員的成員名稱是John Jacobs，其帳號是9846，則檔案名稱將是John Jacobs_9846.pdf

![fdmserviceinput](assets/fdminputservice.png)

服務輸入

>[!NOTE]
>
>疑難排解提示 — 如果由於某些原因沒有在DAM中建立DOR.pdf，請按一下[這裡](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2Fglobal%2Fsettings%2Fcloudconfigs%2Ffdm%2Fpostdortodam)來重設資料來源驗證設定。 這些是AEM驗證設定，預設為admin/admin。

若要在您的伺服器上測試此功能，請遵循下列步驟：

1.[部署Developingwithserviceuser套件](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [下載並部署setvalue組合](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。此自訂OSGI組合是用來建立中繼資料屬性，並從提交的表單資料中設定其值。

1. [使用封裝管理員將與本文相關的資產](assets/postdortodam.zip)匯入AEM。您將取得下列專案

   1. 工作流程模型
   1. 最適化表單已設定為提交至AEM Workflow
   1. 設定為使用PostToDam.JSON檔案的資料來源
   1. 使用資料Source的表單資料模型

1. 指向您的[瀏覽器以開啟最適化表單](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
1. 填寫表單並提交。
1. 若記錄檔案已建立並儲存，請核取Assets應用程式。


用於建立資料來源的[Swagger檔案](http://localhost:4502/conf/global/settings/cloudconfigs/fdm/postdortodam/jcr:content/swaggerFile)可供您參考
