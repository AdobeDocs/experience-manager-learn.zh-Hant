---
title: 在AEM Forms OSGi中設定Reader擴充功能
description: 將Reader擴展憑據添加到AEM Forms OSGi中的信任儲存
feature: Reader擴充功能
feature-set: Reader Extensions
topics: development
audience: developer
doc-type: Tutorial
activity: implement
version: 6.4,6.5
topic: 管理
role: Admin
level: Beginner
source-git-commit: 55a6ff5d01898b994aee60f214126c5c18a06a5e
workflow-type: tm+mt
source-wordcount: '158'
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











