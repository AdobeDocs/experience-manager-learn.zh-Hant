---
title: 將LDAP與AEM Forms工作流一起使用
description: 將AEM Forms工作流任務分配給提交者的經理
feature: Adaptive Forms, Workflow
topic: Integrations
role: Developer
version: 6.4,6.5
level: Intermediate
exl-id: 2e9754ff-49fe-4260-b911-796bcc4fd266
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '528'
ht-degree: 0%

---

# 將LDAP與AEM Forms工作流一起使用

將AEM Forms工作流任務分配給提交者的經理。

在工作流中使AEM用自適應表單時，您需要將任務動態分配給表單提交者的管理器。 要完成此使用情形，我們必須使用LdapAEM進行配置。

使用LDAP配置AEM所需的步驟在中介紹 [詳細資訊。](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html)

為了本文的目的，我要附加配置使用AdobeLDAPAEM時使用的配置檔案。 這些檔案包含在可使用包管理器導入的包中。

在下面的螢幕截圖中，我們將提取屬於特定成本中心的所有用戶。 如果要獲取LDAP中的所有用戶，則不能使用額外的篩選器。

![LDAP配置](assets/costcenterldap.gif)

在下面的螢幕抓圖中，我們將組分配給從LDAP中提取到的用戶AEM。 注意分配給導入用戶的表單用戶組。 用戶需要是此組的成員才能與AEM Forms進行交互。 我們還將manager屬性儲存在中的profile/manager節點下AEM。

![辛錢德勒](assets/synchandler.gif)

配置LDAP並將用戶導入到中後AEM，我們可以建立一個工作流，該工作流將任務分配給提交者的管理器。 為此，我們開發了一個簡單的一步式審批流程。

工作流中的第一步將初始步驟的值設定為「否」。 自適應表單中的業務規則將禁用「提交者詳細資訊」面板，並根據初始步驟值顯示「批准者」面板。

第二步將任務分配給提交者的經理。 我們使用自定義代碼獲取提交者的經理。

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

代碼段負責獲取管理器ID並將任務分配給管理器。

我們會找到啟動工作流的人員。 然後獲取manager屬性的值。

根據LDAP中儲存管理器屬性的方式，您可能必須執行一些字串操作才能獲取管理器ID。

請閱讀本文以實施您自己的 [  參與者選擇器。](https://helpx.adobe.com/experience-manager/using/dynamic-steps.html)

要在系統上test此示例(對於Adobe員工，您可以在開箱後使用此示例)

* [下載並部署setvalue包](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。 這是用於設定管理器屬性的自定義OSGI捆綁包。
* [下載並安裝DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [使用包管理器將與此文章關聯AEM的資產導入](assets/aem-forms-ldap.zip).作為此包的一部分，包括LDAP配置檔案、工作流和自適應表單。
* 使用AEM適當的LDAP憑據配置LDAP。
* 使用AEMLDAP憑據登錄。
* 開啟 [時間請求表](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* 填寫表單並提交。
* 提交者的經理應獲得要審閱的表單。

>[!NOTE]
>
>此用於提取管理器名稱的自定義代碼已針對AdobeLDAP進行測試。 如果您正在對其他LDAP執行此代碼，則必須修改或寫入您自己的getParticipant實現以獲取管理器的名稱。
