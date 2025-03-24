---
title: 使用最新原型更新雲端服務專案
description: 使用最新原型更新AEM Forms雲端服務專案
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: AEM Project Archetype
jira: KT-9534
exl-id: c2cd9c52-6f00-4cfe-a972-665093990e5d
duration: 67
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '322'
ht-degree: 0%

---

# 從舊的aem原型移轉

若要使用最新的maven原型更新您現有的AEM Forms專案，您必須手動將程式碼/設定等從舊專案複製到新專案。

依照下列步驟，將使用原型30建立的專案移轉至原型33專案

## 使用最新原型建立maven專案

* 開啟命令提示字元並瀏覽至c：\cloudmanager
* 使用最新原型建立maven專案。
* 在命令提示字元視窗中複製並貼上[文字檔](assets/creating-maven-project.txt)的內容。 您可能必須根據[最新版本](https://github.com/adobe/aem-project-archetype/releases)變更DarchetypeVersion=33。 原型33包含新的AEM Forms主題。
由於我們正在cloudmanager資料夾中建立新的maven專案，而該資料夾已具有aem-banking-application專案，因此您應將**DartifactId**&#x200B;從aem-banking-application變更為其他內容。 我已在本文中使用aem-banking-application1。

>[!NOTE]
>
>如果您依原樣部署此新專案，雲端服務執行個體將不會有HandleFormSubmission和SubmitToAEMServlet。 這是因為每次您使用Cloud Manager部署專案時，`/apps`資料夾下的任何專案都會被刪除和覆寫。

## 複製您的Java程式碼

成功建立專案後，您就可以開始將程式碼/設定等從舊專案複製到此新專案

* 從```C:\CloudManager\aem-banking-application\core\src\main\java\com\aem\bankingapplication\core\servlets```複製HandleFormSubmission servlet
至
  ```C:\CloudManager\aem-banking-application1\core\src\main\java\com\aem\bankingapplication\core\servlets```

* 複製自訂提交來源
  從aem-banking-application到aem-banking-application1專案的```C:\CloudManager\aem-banking-application\ui.apps\src\main\content\jcr_root\apps\bankingapplication\SubmitToAEMServlet```

* 將新專案匯入IntelliJ

* 更新aem-banking-application1專案ui.apps模組中的filter.xml，以包含以下行
  ```<filter root="/apps/bankingapplication/SubmitToAEMServlet"/>```

將所有程式碼複製到新專案後，您就可以將此專案推送到Cloud Manager。

>[!NOTE]
>
>若要將內容(最適化Forms、表單資料模型等)同步到新專案中，您必須在IntelliJ專案中建立適當的資料夾結構，然後使用存放庫工具的「取得」命令將您的IntelliJ專案與AEM執行個體同步。
