---
title: 使用最新原型更新雲服務項目
description: 用最新原型更新AEM Forms雲服務項目
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

# 從舊原型遷移

要使用最新的主原型更新現有的AEM Forms項目，您必須手動將代碼/配置等從舊項目複製到新項目。

按照以下步驟將使用原型30建立的項目遷移到原型33項目

## 使用最新原型建立Maven項目

* 開啟命令提示符並導航到c:\cloudmanager
* 使用最新原型建立maven項目。
* 複製和貼上 [文本檔案](assets/creating-maven-project.txt) 命令提示符窗口中。 您可能必鬚根據 [最新版本](https://github.com/adobe/aem-project-archetype/releases)。 原型33包括新的AEM Forms主題。
由於我們正在cloudmanager資料夾中建立新的maven項目，而該資料夾已經包含銀行應用程式項目，因此您應更改 **達蒂法克** 從銀行應用到不同的東西。 本文使用了銀行應用程式1。

>[!NOTE]
>
>如果按原樣部署此新項目，則雲服務實例將沒有HandleFormSubmission和SubmitToAEMServlet。 這是因為每次您使用Cloud Manager部署項目時， `/apps` 資料夾被刪除並覆蓋。

## 複製Java代碼

成功建立項目後，您就可以開始將代碼/配置等從舊項目複製到此新項目

* 從中複製HandleFormSubmission Servlet ```C:\CloudManager\aem-banking-application\core\src\main\java\com\aem\bankingapplication\core\servlets```
至

   ```C:\CloudManager\aem-banking-application1\core\src\main\java\com\aem\bankingapplication\core\servlets```

* 複製自
   ```C:\CloudManager\aem-banking-application\ui.apps\src\main\content\jcr_root\apps\bankingapplication\SubmitToAEMServlet``` 從aem-banking-application到aem-banking-application1項目

* 將新項目導入到IntelliJ

* 更新aem-banking-application1項目的ui.apps模組中的filter.xml，以包括以下行
   ```<filter root="/apps/bankingapplication/SubmitToAEMServlet"/>```

將所有代碼複製到新項目後，您可以將此項目推送到雲管理器。

>[!NOTE]
>
>要將內容(自適應Forms、表單資料模型等)同步到新項目中，您必須在IntelliJ項目中建立相應的資料夾結構，然後使用回購工具的「獲取」命令將AEMIntelliJ項目與實例同步。
