---
title: 正在將雲端服務設定和表單資料模型推送至雲端例項
description: 根據Azure儲存體表單資料模型建立最適化表單並推播至雲端例項。
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Developer Tools
jira: KT-9006
exl-id: 77c00a35-43bf-485f-ac12-0fffb307dc16
duration: 51
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 0%

---

# 在您的專案中包含雲端服務設定

建立名為&#39;FormTutorial&#39;的設定容器以儲存您的雲端服務設定透過提供Azure儲存體帳戶詳細資料和Azure存取金鑰，在&#39;FormTutorial&#39;容器中為Azure儲存體建立名為&#39;FormsCSAndAzureBlob&#39;的雲端服務設定。

在IntelliJ中開啟您的AEM專案。 請務必新增資料夾FormTutorial，如ui.content專案中所示
![cloud-services-configuration](assets/cloud-services-configuration.png)

請務必在ui.content專案的filter.xml中新增以下專案

```xml
<filter root="/conf/FormTutorial" mode="replace"/>
```

![filter-xml](assets/ui-content-filter.png)

## 在您的專案中包含表單資料模型

根據您在上一步建立的雲端服務設定，建立表單資料模型。 若要將表單資料模型納入您的專案中，請在intelliJ的AEM專案中建立適當的資料夾結構。 例如，我的表單資料模型位於名為「註冊」的資料夾中
![fdm-content](assets/ui-content-fdm.png)

在ui.content專案的filter.xml中包含適當的專案

```xml
<filter root="/content/dam/formsanddocuments-fdm/registrations" mode="replace"/>
```


>[!NOTE]
>
>現在，當您使用Cloud Manager建置和部署專案時，必須在雲端服務設定中重新輸入您的Azure存取金鑰。 若要避免重新輸入存取索引鍵，建議使用環境變數建立內容感知設定，如中所述。 [下一篇文章](./context-aware-fdm.md)

## 後續步驟

[建立內容感知設定](./context-aware-fdm.md)
