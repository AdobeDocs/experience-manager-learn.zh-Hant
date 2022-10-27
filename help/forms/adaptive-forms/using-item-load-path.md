---
title: 使用項目載入路徑來填入下拉式清單
description: 設定並填入下拉式清單以從crx節點讀取值
feature: Adaptive Forms
version: 6.4,6.5
kt: 10961
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-20T00:00:00Z
thumbnail: item-load.jpg
source-git-commit: e1c16ff347f5f398c7bc47233049427eeffa2aab
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 0%

---

# AEM Forms中的項目載入屬性

使用項目載入路徑屬性來設定並填入下拉式清單。
「項目載入路徑」欄位可讓作者提供URL，從中載入下拉式清單中可用的選項。
要在crx中建立這樣的節點，請遵循以下步驟：
* 登入crx
* 建立名為assets的節點（您可以根據需求為此節點命名），在「內容」下方輸入sling:folder。
* 儲存
* 按一下新建立的資產節點，並設定其屬性，如下所示
* 您需要建立名為assettypes的字串類型屬性（您可以根據需求為其命名）。請確定屬性為多值。 提供您想要的值並儲存。
   ![item-load-path](assets/item-load-path-crx.png)

若要在下拉式清單中載入這些值，請在項目載入路徑屬性中提供下列路徑  **/content/assets/assetttypes**

範例套件可以是 [從此處下載](assets/item-load-path-package.zip)
