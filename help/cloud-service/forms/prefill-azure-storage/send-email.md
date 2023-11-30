---
title: 使用SendGrid傳送電子郵件
description: 觸發包含已儲存表單連結的電子郵件
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-8474
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '119'
ht-degree: 0%

---

# 傳送電子郵件

表單資料儲存在Azure Blob Storage中後，會傳送包含已儲存表單連結的電子郵件給使用者。 此電子郵件是使用SendGrid REST API傳送。

在文章資產中，提供了Swagger檔案、表單資料模型和傳送電子郵件所需的雲端服務設定。

您必須建立SendGrid帳戶，此動態範本可內嵌最適化表單中擷取的資料。


以下是動態範本中使用的html程式碼片段。 blobID引數的值是使用表單資料模型叫用服務傳遞。

```html
You can  <a href='https://forms.enablementadobe.com/content/dam/formsanddocuments/azureportalstorage/creditcardapplication/jcr:content?wcmmode=disabled&ampguid={{blobID}}'>access your application here</a> and complete it.
```


