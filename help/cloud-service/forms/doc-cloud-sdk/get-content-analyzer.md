---
title: 建立內容分析器
description: 建立JSON部分，其中包含有關REST呼叫的輸入引數資訊。
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
thumbnail: 7836.jpg
jira: KT-7836
exl-id: 548a21b9-5487-4b48-9782-19b537a48f98
duration: 23
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '55'
ht-degree: 0%

---

# 建立分析器請求

建立定義下列專案的JSON片段：

+ 輸入
+ 引數
+ 輸出。

此[表單引數的詳細資料可在此取得。](https://documentcloud.adobe.com/document-services/index.html#post-createPDF)

下列範常式式碼會產生所有Office 365檔案型別的JSON片段。

```java
package com.aemforms.doccloud.core.impl;

import com.google.gson.JsonObject;

public class GetContentAnalyser {
    public static String getContentAnalyserRequest(String fileName)
    {
        JsonObject outerWrapper = new JsonObject();
        
        
        JsonObject documentIn = new JsonObject();
        documentIn.addProperty("cpf:location", "InputFile0");

        if(fileName.endsWith(".pptx"))
        {
            documentIn.addProperty("dc:format","application/vnd.openxmlformats-officedocument.presentationml.presentation");
        }

        if(fileName.endsWith(".docx"))
        {
            
            documentIn.addProperty("dc:format","application/vnd.openxmlformats-officedocument.wordprocessingml.document");
        }
        if(fileName.endsWith(".xlsx"))
        {
            documentIn.addProperty("dc:format","application/vnd.openxmlformats-officedocument.spreadsheetml.sheet");
        }
        if(fileName.endsWith(".xml"))
        {
            documentIn.addProperty("dc:format","text/plain");
        }
        if(fileName.endsWith(".jpg"))
        {
            documentIn.addProperty("dc:format","image/jpeg");
        }

        JsonObject cpfinputs = new JsonObject();
        cpfinputs.add("documentIn", documentIn);
        
        
        //documentInWrapper.add("documentIn",documentIn);
        JsonObject cpfengine = new JsonObject();
        cpfengine.addProperty("repo:assetId", "urn:aaid:cpf:Service-1538ece812254acaac2a07799503a430");
        
        JsonObject documentOut = new JsonObject();
        
        documentOut.addProperty("cpf:location", "multipartLabelOut");
        documentOut.addProperty("dc:format", "application/pdf");
        JsonObject documentOutWrapper = new JsonObject();
        documentOutWrapper.add("documentOut",documentOut);
    
        outerWrapper.add("cpf:inputs",cpfinputs);
        outerWrapper.add("cpf:engine", cpfengine);
        outerWrapper.add("cpf:outputs", documentOutWrapper);
        System.out.println("The content Analyser is "+outerWrapper.toString());
        
        return outerWrapper.toString();
        
        
        
        
        
    }

}
```
