---
title: 使用表單資料模型發佈二進位資料
description: 使用表單資料模型AEM將二進位資料發佈到DAM
feature: Workflow
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 9c62a7d6-8846-424c-97b8-2e6e3c1501ec
last-substantial-update: 2021-01-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '489'
ht-degree: 0%

---

# 使用表單資料模型發佈二進位資料{#using-form-data-model-to-post-binary-data}

從AEM Forms6.4開始，我們現在能夠調用表單資料模型服務作為工作流中的一AEM個步驟。 本文將引導您瞭解使用表單資料模型服務過帳記錄文檔的示例使用情形。

使用案例如下：

1. 用戶填充並提交自適應表單。
1. 自適應表單被配置為生成記錄文檔。
1. 在提交此自適應表AEM單時，將觸發工作流，該工作流將使用調用表單資料模型服務將記錄文檔POST到AEMDAM。

![後壩](assets/posttodamshot1.png)

「表單資料模型」頁籤 — 屬性

在「服務輸入」(Service Input)頁籤中，我們將映射以下

* 檔案（需要儲存的二進位對象），與負載相關，具有DOR.pdf屬性。 這意味著，在提交自適應表單時，生成的記錄文檔會儲存在與工作流負載相關的名為DOR.pdf的檔案中。**確保此DOR.pdf與配置Adaptive Form&#39;s submission屬性時提供的相同。**

* fileName — 這是二進位對象儲存在DAM中的名稱。 因此，您希望動態生成此屬性，以便每個fileName在每次提交時都是唯一的。 為此，我們使用工作流中的流程步驟建立名為filename的元資料屬性，並將其值設定為提交表單的人員的「成員名稱」和「帳戶編號」的組合。 例如，如果該人員的成員名是John Jacobs，而他的帳號是9846，則檔案名將是John Jacobs_9846.pdf

![fdmserviceinput](assets/fdminputservice.png)

服務輸入

>[!NOTE]
>
>故障排除提示 — 如果由於某種原因未在DAM中建立DOR.pdf，請通過按一下重置資料源驗證設定 [這裡](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2Fglobal%2Fsettings%2Fcloudconfigs%2Ffdm%2Fpostdortodam)。 這些是驗AEM證設定，預設為admin/admin。

要在伺服器上test此功能，請執行以下步驟：

1。[部署Developingwithserviceuser捆綁包](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [下載並部署setvalue包](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar).此自定義OSGI捆綁包用於建立元資料屬性並從提交的表單資料中設定其值。

1. [導入資產](assets/postdortodam.zip) 與本文關聯，AEM使用包管理器。您將獲得以下

   1. 工作流程模型
   1. 配置為提交到工作流的自適AEM應表單
   1. 已配置為使用PostToDam.JSON檔案的資料源
   1. 使用資料源的表單資料模型

1. 指向 [瀏覽器以開啟自適應窗體](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
1. 填寫表單並提交。
1. 如果建立並儲存了記錄文檔，請檢查Assets應用程式。


[Swagger檔案](http://localhost:4502/conf/global/settings/cloudconfigs/fdm/postdortodam/jcr:content/swaggerFile) 用於建立資料源的可供參考
