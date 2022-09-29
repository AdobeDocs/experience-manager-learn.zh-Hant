---
title: 使用表單資料模型來發佈二進位資料
description: 使用表單資料模型將二進位資料張貼至AEM DAM
feature: Workflow
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 9c62a7d6-8846-424c-97b8-2e6e3c1501ec
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '489'
ht-degree: 0%

---

# 使用表單資料模型來發佈二進位資料{#using-form-data-model-to-post-binary-data}

從AEM Forms 6.4開始，現在起，我們可以叫用表單資料模型服務，作為AEM工作流程中的步驟。 本文將引導您了解使用表單資料模型服務張貼記錄檔案的範例使用案例。

使用案例如下：

1. 使用者填寫並提交最適化表單。
1. 最適化表單已設定為產生記錄檔案。
1. 提交此最適化表單時，會觸發AEM工作流程，使用叫用表單資料模型服務將記錄檔案POST至AEM DAM。

![postdam](assets/posttodamshot1.png)

表單資料模型標籤 — 屬性

在「服務輸入」(Service Input)頁簽中，我們映射以下內容

* 檔案（需要儲存的二進位物件），其中包含相對於裝載的DOR.pdf屬性。 這表示提交適用性表單時，產生的記錄檔案會儲存在與工作流程裝載相關的名為DOR.pdf的檔案中。**請確定此DOR.pdf與您在設定適用性表單的提交屬性時提供的相同。**

* fileName — 這是二進位物件儲存在DAM中的名稱。 因此，您希望動態產生此屬性，以便每個檔案名稱在每次提交時都是唯一的。 為此，我們使用工作流中的流程步驟來建立名為filename的元資料屬性，並將其值設定為提交表單的人員的成員名稱和帳號的組合。 例如，如果該人員的成員名稱是John Jacobs，而其帳號是9846，則檔案名稱為John Jacobs_9846.pdf

![fdmserviceinput](assets/fdminputservice.png)

服務輸入

>[!NOTE]
>
>疑難排解提示 — 如果由於某些原因未在DAM中建立DOR.pdf，請按一下「 [此處](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2Fglobal%2Fsettings%2Fcloudconfigs%2Ffdm%2Fpostdortodam). 這些是AEM驗證設定，預設為管理員/管理員。

若要在您的伺服器上測試此功能，請遵循下列步驟：

1.[部署Developmentwithserviceuser套件組合](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [下載並部署setvalue套件組合](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar).此自訂OSGI套件組合可用來建立中繼資料屬性，並從提交的表單資料設定其值。

1. [匯入資產](assets/postdortodam.zip) 使用套件管理程式將本文關聯至AEM。您將取得下列內容

   1. 工作流程模型
   1. 設定為提交至AEM工作流程的適用性表單
   1. 設定為使用PostToDam.JSON檔案的資料來源
   1. 使用資料來源的表單資料模型

1. 指向您的 [開啟最適化表單的瀏覽器](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
1. 填寫表格並提交。
1. 如果已建立並儲存記錄檔案，請核取資產應用程式。


[Swagger檔案](http://localhost:4502/conf/global/settings/cloudconfigs/fdm/postdortodam/jcr:content/swaggerFile) 建立資料來源時使用，可供您參考
