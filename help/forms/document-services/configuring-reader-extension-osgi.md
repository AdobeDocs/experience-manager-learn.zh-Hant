---
title: 在AEM FormsOSGi中配置Reader擴展
description: 將Reader擴展憑據添加到AEM FormsOSGi中的信任儲存
feature: Reader Extensions
audience: developer
type: Tutorial
version: 6.4,6.5
topic: Administration
role: Admin
level: Beginner
exl-id: 1f16acfd-e8fd-4b0d-85c4-ed860def6d02
last-substantial-update: 2020-08-01T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 0%

---

# 添加Reader擴展憑據{#configuring-reader-extension-osgi}

DocAssurance服務可以將使用權限應用於PDF文檔。 要對PDF文檔應用使用權限，請配置證書。

## 為fd-service用戶建立密鑰庫

讀取器擴展證書與fd-service用戶相關聯。 要向fd-service用戶添加憑據，請執行以下步驟。 如果已為fd-service用戶建立密鑰庫，請跳過此部分

* 以管理員身份登錄到AEM作者實例
* 轉到工具 — 安全 — 用戶
* 向下滾動用戶清單，直到找到fd-service用戶帳戶
* 按一下fd-service用戶
* 按一下密鑰庫頁籤
* 按一下建立密鑰儲存
* 設定KeyStore訪問密碼並保存設定以建立KeyStore密碼

### 向fd-service用戶密鑰庫添加憑據

請按照視頻將憑據添加到fd-service用戶

>[!VIDEO](https://video.tv.adobe.com/v/335849?quality=12&learn=on)


列出pfx檔案詳細資訊的命令是。 以下命令假定您與pfx檔案位於同一目錄中。

**keytool -v -list -storetype pkcs12 -keystore &lt;name of=&quot;&quot; your=&quot;&quot; pfx=&quot;&quot; file=&quot;&quot;>**

例如keytool -v -list -storetype pkcs12 -keystore 1005566.pfx，其中1005566.pfx是我的pfx檔案的名稱
