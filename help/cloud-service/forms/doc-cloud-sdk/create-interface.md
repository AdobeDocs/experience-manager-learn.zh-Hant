---
title: 建立服務介面
description: 在您要公開的介面中定義方法
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
thumbnail: 7825.jpg
jira: KT-7825
exl-id: f262013b-aaf1-43d1-84b8-6173942c3415
duration: 7
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '24'
ht-degree: 4%

---


# 介面

使用下列2個方法定義建立介面。

```java
package com.aemforms.doccloud.core;

import java.io.InputStream;

import com.adobe.aemfd.docmanager.Document;

public interface DocumentCloudSDKService {	
	public Document getPDF(String location,String accessToken,String fileName);
	
    public Document createPDFFromInputStream(InputStream is,String fileName);
}
```
