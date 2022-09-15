---
title: 修正將大型xml資料檔案與xdp範本合併時的逾時錯誤
description: 在AEM Forms中合併大型xml檔案與範本
type: Troubleshooting
role: Admin
level: Intermediate
version: 6.5
feature: Output Service,Forms Service
topic: Administration
kt: 11091
source-git-commit: 164741ce5ae7d00f904365589438c2eaaf1e05db
workflow-type: tm+mt
source-wordcount: '185'
ht-degree: 0%

---

# 如何將大型xml資料檔案與xdp範本合併，以啟用pdf檔案的建立

將大型xml資料檔案與xdp範本合併時，您可能會在記錄檔中看到下列錯誤

```txt
POST /services/OutputService/GeneratePdfOutput HTTP/1.1] com.adobe.fd.output.internal.exception.OutputServiceException AEM_OUT_001_003:Unexpected Exception: client timeout reached org.omg.CORBA.TIMEOUT: client timeout reached
```

若要修正上述錯誤，請執行下列動作

## 變更程式庫逾時

* 停止AEM Server
* 建立名為 **安裝** 在AEM安裝的crx-quickstart資料夾下
* 建立檔案，稱為 **org.apache.aries.transaction.config** 安裝資料夾下方的以下內容aries.transaction.timeout=&quot;1200&quot;。 您可以根據需求變更逾時值。 逾時值以秒為單位

>[!NOTE]
> 建立org.apache.aries.transaction設定後，您就可以從 [configMgr](http://localhost:4502/system/console/configMgr) 而不是編輯檔案


## 更改Jacorb ORB提供程式設定

* [開啟OSGi ConfigMgr](http://localhost:4502/system/console/configMgr)
* 搜尋 **Jacorb ORB提供程式**
* 添加以下條目jacorb.connection.client.pending_reply_timeout=600000上述設定將掛起的答復超時（也稱為CORBA客戶端超時）設定為600秒。
* 儲存您的變更
