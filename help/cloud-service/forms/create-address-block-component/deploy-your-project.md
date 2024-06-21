---
title: 建立地址元件
description: 在AEM FormsCloud Service中建立新的位址核心元件
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15752
source-git-commit: a8fc8fa19ae19e27b07fa81fc931eca51cb982a1
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 0%

---

# 部署您的專案

開始將專案部署至您的AEM FormsCloud Service之前，建議先將專案部署至本機雲端就緒的AEM Forms執行個體。

## 將變更與您的AEM專案同步

啟動IntelliJ並導覽至 ``ui.apps`` 資料夾，如下所示
![intellij](assets/intellij.png)

按一下右鍵 ``adaptiveForm`` 節點並選取新增 | 封裝請務必新增名稱 **addressblock** 至封裝

以滑鼠右鍵按一下新建立的封裝 ``addressblock`` 並選取 ``repo | Get Command`` 如下所示
![repo-sync](assets/sync-repo.png)

這會將專案與您的本機雲端就緒AEM Forms執行個體同步。 您可以驗證.content.xml檔案以確認屬性
![同步之後](assets/after-sync.png)

## 將專案部署到您的本機執行個體

啟動新的命令提示視窗，並瀏覽至專案的根資料夾，然後使用下面顯示的命令建置專案
![部署](assets/build-project.png)

成功部署專案後，現在可以在最適化表單中使用位址元件

## 將專案部署到雲端環境

如果您的本機開發環境一切正常，下一步就是部署至 [使用cloud manager的雲端例項。](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/push-project-to-cloud-manager-git)



