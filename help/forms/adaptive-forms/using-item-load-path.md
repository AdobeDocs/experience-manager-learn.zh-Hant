---
title: 使用項載入路徑填充下拉清單
description: 配置並填充下拉清單以從crx節點讀取值
feature: Adaptive Forms
version: 6.4,6.5
kt: 10961
topic: Development
role: Developer
level: Beginner
source-git-commit: 614db8b03a823b60846ab8ccfa8fbc29a41f7791
workflow-type: tm+mt
source-wordcount: '175'
ht-degree: 0%

---

# 物料裝載屬性在AEM Forms

使用項載入路徑屬性配置和填充下拉清單。
「項目載入路徑」欄位允許作者提供一個URL，從中載入下拉清單中可用的選項。
要在crx中建立此類節點，請執行以下步驟

* 登錄到crx
* 建立名為assets的節點（您可以根據需要命名此節點），在內容下鍵入sling:folder。
* 儲存
* 按一下新建立的資產節點並設定其屬性，如下所示
* 您需要建立名為assettypes的String類型屬性（您可以根據需求將其命名為），該屬性具有多值。 提供所需值並保存。

![項目載入路徑](assets/item-load-path-crx.png)

要在下拉清單中載入這些值，請在項載入路徑屬性中提供以下路徑  **/content/assets/assettypes**

示例包可以 [從此處下載](assets/item-load-path-package.zip)
