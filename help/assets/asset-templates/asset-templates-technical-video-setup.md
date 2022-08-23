---
title: 設定資產模板，包括AEM Assets和InDesign Server
description: 資產模板使營銷人員能夠建立、管理和交付用於數字和打印的數字資產。 與InDesign伺服器整合後，使用資產模板建立營銷手冊、名片、傳單、廣告和明信片將會更加輕鬆。 本節將介AEM紹InDesign伺服器配置。
version: 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
exl-id: 5b764d86-8ced-46ed-838e-4bd2e75fd64c
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '154'
ht-degree: 0%

---

# 設定資產模板，包括AEM Assets和InDesign Server{#set-up-asset-templates-with-aem-assets-and-indesign-server}

資產模板使營銷人員能夠建立、管理和交付用於數字和打印的數字資產。 與InDesign伺服器整合後，使用資產模板建立營銷手冊、名片、傳單、廣告和明信片將會更加輕鬆。 本節將介AEM紹InDesign伺服器配置。

>[!VIDEO](https://video.tv.adobe.com/v/17069/?quality=9&learn=on)

>[!NOTE]
>
>AEM **必須** 在上載INDD模板時連接到正在運行的InDesign伺服器。 INDD檔案的初始處理部分需要InDesign伺服器。

## 下載InDesign Server試用版 {#download-indesign-server-trial}

下載 [InDesign Server試用下載網站](https://www.adobeprerelease.com/)

## 啟動InDesign Server {#starting-indesign-server}

```shell
# macOS command

$ /Applications/Adobe\ InDesign\ CC\ Server\ 2017/InDesignServer -port 8080
```
