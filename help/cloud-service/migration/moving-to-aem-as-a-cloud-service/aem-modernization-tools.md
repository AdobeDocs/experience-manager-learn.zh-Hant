---
title: 使用AEM現代化工具移至AEMas a Cloud Service
description: 了解如何使用AEM現代化工具將現有的AEM專案和內容升級以as a Cloud Service相容AEM。
version: Cloud Service
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8629
thumbnail: 336965.jpeg
exl-id: 310f492c-0095-4015-81a4-27d76f288138
source-git-commit: 3657e7798774f9cc673ff6ccd8af1a555b1d4013
workflow-type: tm+mt
source-wordcount: '325'
ht-degree: 1%

---


# AEM 現代化工具

了解如何使用AEM現代化工具將現有的AEM Sites內容升級為與AEMas a Cloud Service相容，並符合最佳實務。

>[!VIDEO](https://video.tv.adobe.com/v/336965/?quality=12&learn=on)

## 使用AEM現代化工具

![AEM現代化工具生命週期](./assets/aem-modernization-tools.png)

AEM現代化工具會自動轉換由舊版靜態範本、基礎元件和parsys組成的現有AEM頁面，以使用可編輯範本、AEM核心WCM元件和版面容器等現代化方法。

### 關鍵活動

+ 複製AEM 6.x生產環境，針對
+ 下載並安裝 [最新AEM現代化工具](https://github.com/adobe/aem-modernize-tools/releases/latest) 在AEM 6.x生產克隆上（透過封裝管理器）

+ [頁面結構轉換器](https://opensource.adobe.com/aem-modernize-tools/pages/tools/page-structure.html) 使用「配置容器」將現有頁面內容從靜態範本更新為已映射的可編輯模板
   + 使用OSGi設定定義轉換規則
   + 對現有頁面執行頁面結構轉換器

+ [元件轉換工具](https://opensource.adobe.com/aem-modernize-tools/pages/tools/component.html) 使用「配置容器」將現有頁面內容從靜態範本更新為已映射的可編輯模板
   + 透過JCR節點定義/XML定義轉換規則
   + 對現有頁面執行「元件轉換工具」

+ [政策匯入工具](https://opensource.adobe.com/aem-modernize-tools/pages/tools/policy-importer.html) 從設計配置建立策略
   + 使用JCR節點定義/XML定義轉換規則
   + 根據現有設計定義運行策略導入程式
   + 將匯入的原則套用至AEM元件和容器

+ [對話方塊轉換工具](https://opensource.adobe.com/aem-modernize-tools/pages/tools/dialog.html) 將傳統(ExtJS)和CoralUI 2型元件對話方塊轉換為CoralUI 3觸控式UI型對話方塊。
   + 對現有的ExtJS或Coral2 UI型對話方塊執行「對話轉換工具」
   + 將轉換的對話方塊同步回Git存放庫

### 其他資源

+ [下載AEM現代化工具](https://github.com/adobe/aem-modernize-tools/releases/latest)
+ [AEM現代化工具檔案](https://opensource.adobe.com/aem-modernize-tools/)
+ [AEM Gems -AEM現代化套件簡介](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/Introducing-the-AEM-Modernization-Suite.html)
