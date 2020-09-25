---
title: 搭配Aem Forms工作流程使用LDAP
seo-title: 搭配Aem Forms工作流程使用LDAP
description: 將AEM Forms工作流程任務指派給提交者的管理員
seo-description: 將AEM Forms工作流程任務指派給提交者的管理員
feature: adaptive-forms,workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
uuid: 3e32c3a7-387f-4652-8a94-4e6aa6cd5ab8
discoiquuid: 671872b3-3de0-40da-9691-f8b7e88a9443
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '543'
ht-degree: 0%

---


# 搭配AEM Forms工作流程使用LDAP

將AEM Forms工作流程任務指派給提交者的管理員。

在AEM工作流程中使用「最適化表單」時，您會想要動態指派工作給表單提交者的管理員。 若要完成此使用案例，我們必須使用Ldap來設定AEM。

此處詳細說明使用LDAP設定AEM所需 [的步驟。](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html)

就本文而言，我要附加用於使用Adobe Ldap設定AEM的設定檔案。 這些檔案包含在包中，可使用包管理器導入。

在下面的螢幕擷取中，我們會擷取屬於特定成本中心的所有使用者。 如果要獲取LDAP中的所有用戶，則不能使用額外的篩選器。

![LDAP配置](assets/costcenterldap.gif)

在下方的螢幕擷取中，我們將群組指派給從LDAP擷取到AEM的使用者。 請注意指派給已匯入使用者的表單使用者群組。 使用者必須是此群組的成員，才能與AEM Forms互動。 我們也會在AEM的描述檔／管理員節點下方儲存manager屬性。

![Synchandler](assets/synchandler.gif)

在您設定LDAP並將使用者匯入AEM後，我們可以建立工作流程，將工作指派給提交者的管理員。 為此，我們開發了一個簡單的單步驟審批工作流程。

工作流的第一個步驟將初始步驟的值設定為「否」。 最適化表單中的業務規則將禁用「提交者詳細資訊」面板，並根據初始步驟值顯示「批准者」面板。

第二個步驟將任務分配給提交者的管理員。 我們使用自定義代碼獲取提交者的管理員。

![分派工作](assets/assigntask.gif)

```java
public String getParticipant(WorkItem workItem, WorkflowSession wfSession, MetaDataMap arg2) throws WorkflowException{
resourceResolver = wfSession.adaptTo(ResourceResolver.class);
UserManager userManager = resourceResolver.adaptTo(UserManager.class);
Authorizable workflowInitiator = userManager.getAuthorizable(workItem.getWorkflow().getInitiator());
.
.
String managerPorperty = workflowInitiator.getProperty("profile/manager")[0].getString();
.
.

}
```

程式碼片段負責擷取管理員ID並指派工作給管理員。

我們瞭解啟動工作流程的人員。 然後，我們將獲取manager屬性的值。

根據manager屬性在LDAP中的儲存方式，您可能需要執行一些字串操作來獲取管理器ID。

請閱讀本文以實作您自己的「參與者選擇 [ 器」。](https://helpx.adobe.com/experience-manager/using/dynamic-steps.html)

若要在您的系統上測試此範例（若是Adobe員工，您可立即使用此範例）

* [下載並部署setvalue組合](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。 這是用於設定管理器屬性的自定義OSGI包。
* [下載並安裝DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [使用套件管理員將與本文相關的「資產」匯入AEM](assets/aem-forms-ldap.zip)。本套件中包含LDAP設定檔、工作流程和最適化表單。
* 使用適當的LDAP憑證搭配您的LDAP設定AEM。
* 使用您的LDAP認證登入AEM。
* 開啟時間 [offrequestform](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* 填寫表格並送出。
* 提交者的管理員應該收到表單以供審核。

>[!NOTE]
>
>此自訂程式碼已針對Adobe LDAP測試，以擷取管理員名稱。 如果要針對不同的LDAP執行此代碼，則必須修改或寫入您自己的getParticipant實施以獲取管理員的名稱。
