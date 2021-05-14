---
title: 建立服務介面
description: 在介面中定義您要公開的方法
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: 開發
thumbnail: 7825.jpg
kt: 7825
source-git-commit: 84499d5a7c8adac87196f08c6328e8cb428c0130
workflow-type: tm+mt
source-wordcount: '24'
ht-degree: 8%

---

# 介面

使用下列2種方法定義建立介面。

```java
package com.aemforms.doccloud.core;

import java.io.InputStream;

import com.adobe.aemfd.docmanager.Document;

public interface DocumentCloudSDKService {
	
	public Document getPDF(String location,String accessToken,String fileName);
	public Document createPDFFromInputStream(InputStream is,String fileName);

}


}
```
