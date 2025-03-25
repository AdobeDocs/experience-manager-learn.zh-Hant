---
title: 在AEM Forms OSGi中設定Reader擴充功能
description: 將Reader擴充功能認證新增至AEM Forms OSGi中的信任存放區
feature: Reader Extensions
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
topic: Administration
role: Admin
level: Beginner
exl-id: 1f16acfd-e8fd-4b0d-85c4-ed860def6d02
last-substantial-update: 2020-08-01T00:00:00Z
duration: 308
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 0%

---

# 新增Reader擴充功能認證{#configuring-reader-extension-osgi}

DocAssurance服務可套用使用許可權至PDF檔案。 若要套用使用許可權至PDF檔案，請設定憑證。

## 為fd-service使用者建立金鑰存放區

Reader延伸模組認證與fd-service使用者相關聯。 若要將認證新增至fd-service使用者，請遵循下列步驟。 如果您已經為fd-service使用者建立金鑰存放區，請略過本節

* 以管理員身分登入您的AEM作者執行個體
* 移至Tools-Security-Users
* 向下捲動使用者清單，直到您找到fd-service使用者帳戶為止
* 按一下fd-service使用者
* 按一下金鑰存放區索引標籤
* 按一下「建立金鑰存放區」
* 設定KeyStore存取密碼並儲存您的設定以建立KeyStore密碼

### 將認證新增至fd-service使用者金鑰存放區

請觀看影片，將認證新增至fd服務使用者

>[!VIDEO](https://video.tv.adobe.com/v/335849?quality=12&learn=on)


列出pfx檔案詳細資訊的指令為。 以下命令假設您與pfx檔案位於相同的目錄中。

**keytool -v -list -storetype pkcs12 -keystore &lt;您的.pfx檔案名稱>**

例如keytool -v -list -storetype pkcs12 -keystore 1005566.pfx，其中1005566.pfx是我的pfx檔案名稱
