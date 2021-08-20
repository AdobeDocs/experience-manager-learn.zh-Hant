---
title: 在AEM Forms OSGi中設定Reader擴充功能
description: 將Reader擴充功能憑證新增至AEM Forms OSGi中的信任存放區
feature: Reader擴充功能
audience: developer
type: Tutorial
version: 6.4,6.5
topic: 管理
role: Admin
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '212'
ht-degree: 0%

---


# 添加Reader擴展憑據{#configuring-reader-extension-osgi}

DocAssurance服務可將使用權應用於PDF文檔。 若要對PDF檔案套用使用權限，請設定憑證。

## 為fd-service用戶建立密鑰庫

讀取器擴展證書與fd服務用戶相關聯。 要向fd-service用戶添加憑據，請執行以下步驟。 如果已為fd-service用戶建立密鑰庫，請跳過此部分

* 以管理員身分登入您的AEM Author例項
* 轉到「工具」 — 「安全」 — 「用戶」
* 向下捲動使用者清單，直到找到fd-service使用者帳戶為止
* 按一下fd-service用戶
* 按一下金鑰存放區索引標籤
* 按一下建立KeyStore
* 設定KeyStore訪問密碼並保存您的設定以建立KeyStore密碼

### 將憑據添加到fd-service用戶密鑰庫

請觀看視頻，向fd-service用戶添加憑據

>[!VIDEO](https://video.tv.adobe.com/v/335849?quality=9&learn=on)


列出pfx檔案詳細資訊的命令為。 以下命令假定您位於與pfx檔案相同的目錄中。

**keytool -v -list -storetype pkcs12 -keystore  &lt;name of=&quot;&quot; your=&quot;&quot;>**

例如keytool -v -list -storetype pkcs12 -keystore 1005566.pfx，其中1005566.pfx是我的pfx檔案的名稱













