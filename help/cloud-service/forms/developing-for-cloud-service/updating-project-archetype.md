---
title: 使用最新原型更新雲端服務專案
description: 使用最新原型更新AEM Forms雲端服務專案
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 9534
exl-id: c2cd9c52-6f00-4cfe-a972-665093990e5d
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 0%

---

# 從舊aem原型移轉

若要使用最新的Maven原型來更新現有的AEM Forms專案，您必須手動將程式碼/設定等從舊專案複製到新專案。

依照下列步驟，將使用原型30建立的專案移轉至原型33專案

## 使用最新原型建立Maven專案

* 開啟命令提示符並導航到c:\cloudmanager
* 使用最新的原型建立Maven專案。
* 複製並貼上 [文字檔案](assets/creating-maven-project.txt) 在命令提示符窗口中。 您可能必鬚根據 [最新版本](https://github.com/adobe/aem-project-archetype/releases). 原型33包含新的AEM Forms主題。
由於我們要在已有aem-banking-application專案的cloudmanager資料夾中建立新的maven專案，因此您應變更 **DartifactId** 從aem-banking-application轉換為其他不同。 本文已使用aem-banking-application1。

>[!NOTE]
>
>如果您以雲端服務例項的方式部署此新專案，將沒有HandleFormSubmission和SubmitToAEMServlet。 這是因為每次您使用Cloud Manager部署專案時， `/apps` 資料夾被刪除和覆蓋。

## 複製您的Java程式碼

成功建立專案後，您就可以開始將程式碼/設定等從舊專案複製到這個新專案

* 從複製HandleFormSubmission servlet ```C:\CloudManager\aem-banking-application\core\src\main\java\com\aem\bankingapplication\core\servlets```
to

   ```C:\CloudManager\aem-banking-application1\core\src\main\java\com\aem\bankingapplication\core\servlets```

* 複製CustomSubmit自
   ```C:\CloudManager\aem-banking-application\ui.apps\src\main\content\jcr_root\apps\bankingapplication\SubmitToAEMServlet``` 從aem-banking-application至aem-banking-application1專案

* 將新專案匯入IntelliJ

* 更新aem-banking-application1專案的ui.apps模組中的filter.xml，加入下列行
   ```<filter root="/apps/bankingapplication/SubmitToAEMServlet"/>```

將所有程式碼複製到新專案後，即可將此專案推送至cloud manager。

>[!NOTE]
>
>若要將內容(適用性Forms、表單資料模型等)同步至您的新專案，您必須在IntelliJ專案中建立適當的資料夾結構，然後使用存放庫工具的Get命令，將您的IntelliJ專案與AEM例項同步。
