---
title: 將雲端服務設定和表單資料模型推送至雲端例項
description: 根據Azure儲存表單資料模型建立最適化表單並推送至雲端例項。
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: Development
kt: 9006
source-git-commit: 8484897297940ab28619c4b1af5362a5937eadfa
workflow-type: tm+mt
source-wordcount: '202'
ht-degree: 0%

---


# 將雲端服務設定納入您的專案

建立名為「FormsTutorial」的配置容器以保存雲服務配置在「FormsTutorial」容器中為名為「在Azure中儲存表單提交」的Azure儲存建立雲服務配置。 提供Azure儲存帳戶詳細資訊和帳戶金鑰

在IntelliJ中開啟您的AEM專案。 請務必新增資料夾FormTutorial，如ui.content專案中所示
![cloud-services-configuration](assets/cloud-services-configuration.png)

請務必在ui.content專案的filter.xml中新增下列項目

```xml
<filter root="/conf/FormTutorial" mode="replace"/>
```

![filter-xml](assets/ui-content-filter.png)

## 在專案中加入表單資料模型

根據您在先前步驟中建立的雲端服務設定，建立表單資料模型。 若要在專案中加入表單資料模型，請在intelliJ的AEM專案中建立適當的資料夾結構。 例如，我的表單資料模型位於名為註冊的資料夾中
![fdm-content](assets/ui-content-fdm.png)

在ui.content專案的filter.xml中加入適當的項目

```xml
<filter root="/content/dam/formsanddocuments-fdm/registrations" mode="replace"/>
```


>[!NOTE]
>
>現在當您建置和部署專案時，專案將會根據雲端例項中可用的雲端服務組態，提供表單資料模型
