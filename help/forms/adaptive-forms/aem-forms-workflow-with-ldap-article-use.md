---
title: 搭配AEM Forms工作流程使用LDAP
description: 指派AEM Forms工作流程任務給提交者的管理員
feature: Adaptive Forms, Workflow
topic: Integrations
role: Developer
version: Experience Manager 6.4, Experience Manager 6.5
level: Intermediate
exl-id: 2e9754ff-49fe-4260-b911-796bcc4fd266
last-substantial-update: 2021-09-18T00:00:00Z
duration: 111
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '508'
ht-degree: 0%

---

# 搭配AEM Forms工作流程使用LDAP

指派AEM Forms工作流程任務給提交者的管理員。

在AEM工作流程中使用最適化表單時，您會想要動態地將任務指派給表單提交者的管理員。 若要完成此使用案例，我們必須使用Ldap設定AEM。

使用LDAP設定AEM所需的步驟在[此處](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html)詳細說明。

出於本文的目的，我附加了使用Adobe Ldap設定AEM時使用的設定檔案。 這些檔案包含在封裝中，可使用封裝管理員匯入這些檔案。

在下方熒幕擷圖中，我們擷取屬於特定成本中心的所有使用者。 如果您想要擷取LDAP中的所有使用者，則不可使用額外的篩選器。

![LDAP組態](assets/costcenterldap.gif)

在下方熒幕擷圖中，我們將群組指派給從LDAP擷取到AEM的使用者。 請注意指派給匯入使用者的表單 — 使用者群組。 使用者必須是此群組的成員，才能與AEM Forms互動。 我們也會將管理員屬性儲存在AEM中的設定檔/管理員節點下。

![同步處理程式](assets/synchandler.gif)

設定LDAP並將使用者匯入AEM後，我們便會建立工作流程，將任務指派給提交者的管理員。 為了撰寫本文章，我們已開發簡單的單步驟核准工作流程。

工作流程的第一步是將initialstep的值設定為「否」。 最適化表單中的商業規則將停用「提交者詳細資料」面板，並根據初始步驟值顯示「核准者」面板。

第二個步驟會將工作指派給提交者的管理員。 我們使用自訂程式碼來取得提交者的管理員。

![指派工作](assets/assigntask.gif)

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

程式碼片段負責擷取管理員ID，並將工作指派給管理員。

我們掌握啟動工作流程的人員。 然後我們取得管理員屬性的值。

視管理員屬性儲存在LDAP中的方式而定，您可能需要進行一些字串操作才能取得管理員ID。

請參閱本文章以實作您自己的[ParticipantChooser 。](https://helpx.adobe.com/experience-manager/using/dynamic-steps.html)

若要在系統上測試此專案(若為Adobe員工，您可立即使用此範例)

* [下載並部署setvalue組合](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。 這是用於設定管理員屬性的自訂OSGI套件。
* [下載並安裝DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [使用封裝管理員](assets/aem-forms-ldap.zip)，將與本文相關的Assets匯入至AEM。此封裝包含LDAP組態檔、工作流程以及最適化表單。
* 使用適當的LDAP憑證以您的LDAP設定AEM。
* 使用您的LDAP憑證登入AEM。
* 開啟[timeoffrequestform](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* 填寫表單並提交。
* 提交者的經理應取得表單以供檢閱。

>[!NOTE]
>
>此用於擷取管理員名稱的自訂程式碼已針對Adobe LDAP進行測試。 如果您要針對不同的LDAP執行此程式碼，您必須修改或撰寫您自己的getParticipant實作，才能取得管理員名稱。
