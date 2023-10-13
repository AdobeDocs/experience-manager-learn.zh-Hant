---
title: 從自訂提交中擷取回應
description: 成功提交表單時擷取自訂回應
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 13520
exl-id: e5f76d6a-2ea8-4909-9cfb-b673077cf8fd
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: tm+mt
source-wordcount: '51'
ht-degree: 0%

---

# 從回應中擷取json物件

當您將Headless最適化表單提交至自訂提交處理常式時，可擷取提交處理常式傳回的資料，並將其顯示在您的react應用程式中。 下列程式碼片段顯示

```javascript
// associate event handler for the onSubmitSuccess event
<AdaptiveForm mappings={extendMappings} onSubmitSuccess={onSuccess} formJson={selectedForm}/>
```

```javascript
// extract the json returned by the custom submit service
const onSuccess=(action) =>{
        let body = action.payload?.body;
        let FirstName = JSON.parse(body.metadata.json).firstName;
        setThankYouMessage(FirstName+" your request has been received");
        
      }
```
