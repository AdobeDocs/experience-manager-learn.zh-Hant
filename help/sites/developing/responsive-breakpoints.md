---
title: 響應斷點
description: 瞭解如何為響應頁面編輯器配置AEM新的響應斷點。
version: Cloud Service
feature: Page Editor
topic: Mobile, Development
role: Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-01-05T00:00:00Z
kt: 11664
thumbnail: kt-11664.jpeg
exl-id: 8b48c28f-ba7f-4255-be96-a7ce18ca208b
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 0%

---

# 響應斷點

瞭解如何為響應頁面編輯器配置AEM新的響應斷點。

## 建立CSS斷點

首先，在響應網格CSSAEM中建立響應站點所AEM附加的媒體斷點。

在 `/ui.apps/src/main/content/jcr_root/apps/[app name]/clientlibs/clientlib-grid/less/grid.less` 檔案，建立要與mobile模擬器一起使用的斷點。 記錄 `max-width` 將CSS斷點映射到響應頁面編輯器AEM斷點。

![建立新的響應斷點](./assets/responsive-breakpoints/create-new-breakpoints.jpg)

## 自定義模板的斷點

開啟 `ui.content/src/main/content/jcr_root/conf/<app name>/settings/wcm/templates/page-content/structure/.content.xml` 檔案和更新 `cq:responsive/breakpoints` 定義。 每個 [CSS斷點](#create-new-css-breakpoints) 應在下面有相應的節點 `breakpoints` 和 `width` 屬性設定為CSS斷點的 `max-width`。

![自定義模板的響應斷點](./assets/responsive-breakpoints/customize-template-breakpoints.jpg)

## 建立模擬器

必AEM須定義模擬器，使作者能夠在頁面編輯器中選擇要編輯的響應視圖。

在下面建立模擬器節點 `/ui.apps/src/main/content/jcr_root/apps/<app name>/emulators`

比如說， `/ui.apps/src/main/content/jcr_root/apps/wknd-examples/emulators/phone-landscape`。 從複製引用模擬器節點 `/libs/wcm/mobile/components/emulators` CRXDE Lite並更新副本以加快節點定義。

![建立新模擬器](./assets/responsive-breakpoints/create-new-emulators.jpg)

## 建立設備組

將模擬器分組到 [在頁面編輯器中AEM使用](#update-the-templates-device-group)。

建立 `/apps/settings/mobile/groups/<name of device group>` 節點結構 `/ui.apps/src/main/content/jcr_root`。

![建立新設備組](./assets/responsive-breakpoints/create-new-device-group.jpg)

建立 `.content.xml` 檔案 `/apps/settings/mobile/groups/<device group name>` 並使用與以下代碼類似的代碼定義新模擬器：

![建立新設備](./assets/responsive-breakpoints/create-new-device.jpg)

## 更新模板的設備組

最後，將設備組映射回頁面模板，以便模擬器在頁面編輯器中可用於從此模板建立的頁面。

開啟 `ui.content/src/main/content/jcr_root/conf/[app name]/settings/wcm/templates/page-content/structure/.content.xml` 檔案並更新 `cq:deviceGroups` 引用新移動組的屬性(例如， `cq:deviceGroups="[mobile/groups/customdevices]"`)
