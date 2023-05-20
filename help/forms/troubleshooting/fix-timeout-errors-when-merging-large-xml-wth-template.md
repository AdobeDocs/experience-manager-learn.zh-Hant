---
title: 將大型xml資料檔案與xdp模板合併時修復超時錯誤
description: 將大xml檔案與模板合併到AEM Forms
type: Troubleshooting
role: Admin
level: Intermediate
version: 6.5
feature: Output Service,Forms Service
topic: Administration
kt: 11091
exl-id: 933ec5f6-3e9c-4271-bc35-4ecaf6dbc434
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '185'
ht-degree: 0%

---

# 如何通過將大xml資料檔案與xdp模板合併來啟用pdf檔案的建立

將大型xml資料檔案與xdp模板合併時，在日誌檔案中可能會看到以下錯誤

```txt
POST /services/OutputService/GeneratePdfOutput HTTP/1.1] com.adobe.fd.output.internal.exception.OutputServiceException AEM_OUT_001_003:Unexpected Exception: client timeout reached org.omg.CORBA.TIMEOUT: client timeout reached
```

要修復上述錯誤，請執行以下操作

## 更改aries超時

* 停止服AEM務器
* 建立名為 **安裝** 安裝的crx-quickstart資料夾下AEM的
* 建立名為 **org.apache.aries.transaction.config** 安裝資料夾下的以下內容aries.transaction.timeout=&quot;1200&quot;。 您可以根據需要更改超時值。 超時值以秒為單位

>[!NOTE]
> 建立org.apache.aries.transaction配置後，可以從 [configMgr](http://localhost:4502/system/console/configMgr) 而不是編輯檔案


## 更改Jacorb ORB提供程式設定

* [開啟OSGi ConfigMgr](http://localhost:4502/system/console/configMgr)
* 搜索 **Jacorb ORB提供程式**
* 添加以下條目jacorb.connection.client.pending_reply_timeout=60000以上設定將掛起的答復超時（也稱為CORBA客戶端超時）設定為600秒。
* 保存更改
