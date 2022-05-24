---
title: 將雲服務配置和表單資料模型推送到雲實例
description: 建立基於Azure儲存表單資料模型的自適應表單並將其推送到雲實例。
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: Development
kt: 9006
exl-id: 77c00a35-43bf-485f-ac12-0fffb307dc16
source-git-commit: 2ac0f6b3964590e5443700f730a3fc02cb3f63bc
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 0%

---

# 在項目中包括雲服務配置

建立名為「FormTutorial」的配置容器以保存你的雲服務配置通過提供Azure儲存帳戶詳細資訊和Azure訪問密鑰，在「FormTutorial」容器中為名為「FormsCSAndAzureBlob」的Azure儲存建立雲服務配置。

在AEMIntelliJ中開啟項目。 確保在ui.content項目中添加資料夾FormTutorial，如下所示
![雲服務配置](assets/cloud-services-configuration.png)

確保在ui.content項目的filter.xml中添加以下條目

```xml
<filter root="/conf/FormTutorial" mode="replace"/>
```

![過濾器 — xml](assets/ui-content-filter.png)

## 在項目中包括表單資料模型

根據您在前一步中建立的雲服務配置建立表單資料模型。 要在項目中包括表單資料模型，請在intelliJ中在項目中創AEM建相應的資料夾結構。 例如，我的表單資料模型位於名為註冊的資料夾中
![fdm內容](assets/ui-content-fdm.png)

在ui.content項目的filter.xml中包含相應條目

```xml
<filter root="/content/dam/formsanddocuments-fdm/registrations" mode="replace"/>
```


>[!NOTE]
>
>現在，使用雲管理器構建和部署項目時，必須在雲服務配置中重新輸入您的Azure訪問密鑰。 為避免重新輸入訪問密鑰，建議使用環境變數建立上下文感知配置，如 [下一篇文章](./context-aware-fdm.md)
