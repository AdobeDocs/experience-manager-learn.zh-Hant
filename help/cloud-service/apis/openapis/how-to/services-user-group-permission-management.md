---
title: 產品設定檔與服務使用者群組許可權管理
description: 瞭解如何在AEM as a Cloud Service中管理產品設定檔和服務使用者群組的許可權。
version: Experience Manager as a Cloud Service
feature: Developing
topic: Development, Architecture, Content Management
role: Developer, Leader
level: Beginner
doc-type: Article
jira: KT-17429
thumbnail: KT-17429.jpeg
last-substantial-update: 2025-02-28T00:00:00Z
duration: 0
exl-id: 3230a8e7-6342-4497-9163-1898700f29a4
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '593'
ht-degree: 0%

---

# 產品設定檔與服務使用者群組許可權管理

瞭解如何在AEM as a Cloud Service中管理「產品設定檔」和「服務」使用者群組的許可權。

在本教學課程中，您將學習：

- 產品設定檔及其與「服務」的關聯。
- 正在更新Services使用者群組的許可權。

## 背景

使用AEM API時，您必須將&#x200B;_產品設定檔_&#x200B;指派給Adobe Developer Console （或ADC）專案中的&#x200B;_認證_。 _產品設定檔_ （及相關服務）提供認證的&#x200B;_許可權或授權_，以存取AEM資源。 在以下熒幕擷圖中，您可以看到AEM Assets作者API的&#x200B;_認證_&#x200B;和&#x200B;_產品設定檔_：

![認證和產品設定檔](../assets/how-to/API-Credentials-Product-Profile.png)

產品設定檔與一或多個&#x200B;_服務_&#x200B;相關聯。 在AEM as a Cloud Service中，_服務_&#x200B;代表具有預先定義之存放庫節點存取控制清單(ACL)的使用者群組，允許精細的許可權管理。

![技術帳戶使用者產品設定檔](../assets/s2s/technical-account-user-product-profile.png)

成功引發API後，會在AEM作者服務中建立代表ADC專案憑證的使用者，以及符合產品設定檔和服務設定的使用者群組。

![技術帳戶使用者成員資格](../assets/s2s/technical-account-user-membership.png)

在上述案例中，使用者`1323d2...`是在AEM Author服務中建立的，並且是使用者群組`AEM Assets Collaborator Users - Service`和`AEM Assets Collaborator Users - author - Program XXX - Environment XXX`的成員。

## 更新Services使用者群組許可權

大部分&#x200B;_服務_&#x200B;透過與&#x200B;_服務_&#x200B;具有相同名稱的AEM執行個體中的使用者群組，提供對AEM資源的&#x200B;_讀取_&#x200B;許可權。

有時憑證（亦稱為技術帳戶使用者）需要其他許可權，例如AEM資源的&#x200B;_建立、更新、刪除_ (CUD)。 在這種情況下，您可以在AEM執行個體中更新&#x200B;_服務_&#x200B;使用者群組的許可權。

例如，當AEM Assets Author API呼叫收到非GET要求的[403錯誤](../use-cases/invoke-api-using-oauth-s2s.md#403-error-for-non-get-requests)時，您可以在AEM執行個體中更新&#x200B;_AEM Assets Collaborator Users - Service_&#x200B;使用者群組的許可權。

使用許可權使用者介面或[Sling存放庫初始化](https://sling.apache.org/documentation/bundles/repository-initialization.html)指令碼，您可以在AEM執行個體中更新現成使用者群組的許可權。

### 使用許可權使用者介面更新許可權

若要使用許可權使用者介面更新服務使用者群組（例如`AEM Assets Collaborator Users - Service`）的許可權，請遵循下列步驟：

- 導覽至AEM執行個體中的&#x200B;**工具** > **安全性** > **許可權**。

- 搜尋服務使用者群組（例如`AEM Assets Collaborator Users - Service`）。

  ![搜尋使用者群組](../assets/how-to/search-user-group.png)

- 按一下&#x200B;**新增ACE**，為使用者群組新增存取控制專案(ACE)。

  ![新增ACE](../assets/how-to/add-ace.png)

### 使用存放庫初始化指令碼更新許可權

若要使用存放庫初始化指令碼更新服務使用者群組（例如`AEM Assets Collaborator Users - Service`）的許可權，請遵循下列步驟：

- 在您最愛的IDE中開啟AEM專案。

- 導覽至`ui.config`模組

- 在`ui.config/src/main/content/jcr_root/apps/<PROJECT-NAME>/osgiconfig/config.author`中建立名為`org.apache.sling.jcr.repoinit.RepositoryInitializer-services-group-acl-update.cfg.json`的檔案，其內容如下：

  ```json
  {
      "scripts": [
          "set ACL for \"AEM Assets Collaborator Users - Service\" (ACLOptions=ignoreMissingPrincipal)",
          "    allow jcr:read,jcr:versionManagement,crx:replicate,rep:write on /content/dam",
          "end"
      ]
  }
  ```

- 提交變更並將其推播到存放庫。

- 使用[Cloud Manager完整棧疊管道](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines#full-stack-pipeline)將變更部署到AEM執行個體。

- 您也可以使用&#x200B;**許可權**&#x200B;檢視來驗證使用者群組的許可權。 導覽至AEM執行個體中的&#x200B;**工具** > **安全性** > **許可權**。

  ![許可權檢視](../assets/how-to/permissions-view.png)

### 驗證許可權

使用上述任何方法更新許可權後，PATCH更新資產中繼資料的要求現在應該可以正常運作，不會出現任何問題。

![PATCH要求](../assets/how-to/patch-request.png)

## 摘要

您已瞭解如何在AEM as a Cloud Service中管理產品設定檔和服務使用者群組的許可權。 您可以使用許可權使用者介面或存放庫初始化指令碼，更新AEM例項中服務使用者群組的許可權。
