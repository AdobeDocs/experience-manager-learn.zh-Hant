---
title: 使用LDAP搭配AemForms Workflow
seo-title: 使用LDAP搭配AemForms Workflow
description: 將AEM Forms工作流任務分配給提交者的經理
seo-description: 將AEM Forms工作流任務分配給提交者的經理
feature: 適用性Forms，工作流程
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
uuid: 3e32c3a7-387f-4652-8a94-4e6aa6cd5ab8
discoiquuid: 671872b3-3de0-40da-9691-f8b7e88a9443
topic: 開發
role: Admin
level: Intermediate
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '547'
ht-degree: 0%

---


# 將LDAP與AEM Forms工作流程搭配使用

將AEM Forms工作流任務分配給提交者的經理。

在AEM工作流程中使用適用性表單時，您會想要以動態方式將任務指派給表單提交者的管理員。 若要完成此使用案例，我們必須使用Ldap來設定AEM。

在[此處詳細說明使用LDAP配置AEM所需的步驟。](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html)

為了本文的目的，我正在附加配置檔案，用於使用AdobeLdap配置AEM。 這些檔案包含在套件中，可使用套件管理器匯入。

在下面的螢幕截圖中，我們將提取屬於特定成本中心的所有用戶。 如果要獲取LDAP中的所有用戶，則不能使用額外的篩選器。

![LDAP配置](assets/costcenterldap.gif)

在下面的螢幕擷取中，我們會將群組指派給從LDAP擷取至AEM的使用者。 注意指派給匯入使用者的表單使用者群組。 使用者必須是此群組的成員，才能與AEM Forms互動。 我們也會將manager屬性儲存在AEM的設定檔/管理員節點下。

![辛錢德勒](assets/synchandler.gif)

配置了LDAP並將用戶導入AEM後，我們可以建立一個工作流，該工作流將任務分配給提交者的管理器。 為了本文的目的，我們開發了簡單的一步式核准工作流程。

工作流程的第一步將初始步驟的值設定為「否」。 適用性表單中的業務規則將禁用「提交者詳細資訊」面板，並根據初始步驟值顯示「批准者」面板。

第二步將任務分配給提交者的經理。 我們使用自定義代碼獲取提交者的管理器。

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

程式碼片段負責擷取管理員id，並將任務指派給管理員。

我們會取得啟動工作流程的人員。 然後，我們將獲取管理器屬性的值。

根據manager屬性在LDAP中的儲存方式，您可能必須執行一些字串操作才能獲取manager id。

請閱讀本文章以實作您自己的[參與者選擇器。](https://helpx.adobe.com/experience-manager/using/dynamic-steps.html)

要在您的系統上測試此示例(對於Adobe員工，可以立即使用此示例)

* [下載並部署setvalue套件組合](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。這是用於設定管理器屬性的自訂OSGI套件組合。
* [下載並安裝DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [使用套件管理器將與本文相關聯的資產匯入AEM](assets/aem-forms-ldap.zip)。此套件包含的包括LDAP組態檔、工作流程和最適化表單。
* 使用適當的LDAP憑證，以您的LDAP設定AEM。
* 使用您的LDAP憑證登入AEM。
* 開啟[timeoffrequestform](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* 填寫表格並提交。
* 提交者的經理應該收到表格以供審核。

>[!NOTE]
>
>此用於提取管理器名稱的自定義代碼已針對AdobeLDAP進行測試。 如果要對不同的LDAP執行此代碼，則必須修改或編寫您自己的getParticipant實施以獲取管理員的名稱。
