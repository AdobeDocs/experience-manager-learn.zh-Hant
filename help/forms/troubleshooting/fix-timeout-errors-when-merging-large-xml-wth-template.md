---
title: 修正將大型xml資料檔案與xdp範本合併時的逾時錯誤
description: 在AEM Forms中將大型xml檔案與範本合併
type: Troubleshooting
role: Admin
level: Intermediate
version: Experience Manager 6.5
feature: Output Service,Forms Service
topic: Administration
jira: KT-11091
exl-id: 933ec5f6-3e9c-4271-bc35-4ecaf6dbc434
duration: 37
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '178'
ht-degree: 0%

---

# 如何將大型xml資料檔案與xdp範本合併，以建立pdf檔案

將大型xml資料檔案與xdp範本合併時，您可能會在記錄檔中看到以下錯誤

```txt
POST /services/OutputService/GeneratePdfOutput HTTP/1.1] com.adobe.fd.output.internal.exception.OutputServiceException AEM_OUT_001_003:Unexpected Exception: client timeout reached org.omg.CORBA.TIMEOUT: client timeout reached
```

若要修正上述錯誤，請執行下列動作

## 變更aries逾時

* 停止AEM伺服器
* 在AEM安裝的crx-quickstart資料夾下建立名為&#x200B;**install**&#x200B;的資料夾
* 建立名為&#x200B;**org.apache.aries.transaction.config**&#x200B;的檔案，其內容如下
aries.transaction.timeout=&quot;1200&quot;
在「安裝資料夾」下。 您可以依需求變更逾時值。 逾時值以秒為單位

>[!NOTE]
> 建立org.apache.aries.transaction設定後，您就可以從[configMgr](http://localhost:4502/system/console/configMgr)編輯交易逾時值，而不用編輯檔案


## 變更Jacorb ORB提供者設定

* [開啟OSGi ConfigMgr](http://localhost:4502/system/console/configMgr)
* 搜尋&#x200B;**Jacorb ORB提供者**
* 新增以下專案
jacorb.connection.client.pending_reply_timeout=600000
上述設定會將擱置中的回覆逾時（也稱為CORBA使用者端逾時）設為600秒。
* 儲存您的變更
