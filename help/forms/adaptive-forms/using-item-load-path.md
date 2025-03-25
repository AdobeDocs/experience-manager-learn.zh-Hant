---
title: 使用專案載入路徑以填入下拉式清單
description: 設定並填入下拉式清單，以從crx節點讀取值
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-10961
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-20T00:00:00Z
thumbnail: item-load.jpg
exl-id: 89c486c8-95c3-4cd4-bf8e-a1b3558f17d6
duration: 34
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 0%

---

# AEM Forms中的專案載入屬性

使用專案載入路徑屬性來設定及填入下拉式清單。
專案載入路徑欄位可讓作者提供一個URL，從中載入下拉式清單中可用的選項。
若要在crx中建立這類節點，請遵循下列步驟操作：
* 登入crx
* 建立名為assets的節點（您可以視需求命名此節點），在「內容」下輸入sling：folder。
* 儲存
* 按一下新建立的資產節點並設定其屬性，如下所示
* 您將需要建立一個名為assettypes的字串型別屬性（您可以視需要加以命名）。請確定屬性是多值。 提供您想要的值並儲存。
  ![專案載入路徑](assets/item-load-path-crx.png)

若要在下拉式清單中載入這些值，請在專案載入路徑屬性&#x200B;**/content/assets/assettypes**&#x200B;中提供下列路徑

範例套件可以[從這裡](assets/item-load-path-package.zip)下載
