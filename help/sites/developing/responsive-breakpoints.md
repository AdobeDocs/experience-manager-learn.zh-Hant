---
title: 回應式中斷點
description: 了解如何為AEM回應式頁面編輯器設定新的回應式中斷點。
version: Cloud Service
feature: Page Editor
topic: Mobile, Development
role: Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-01-05T00:00:00Z
kt: 11664
thumbnail: kt-11664.jpeg
source-git-commit: c82965636ddeef7dc165e0bea079c99f1a16e0ca
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 0%

---


# 回應式中斷點

了解如何為AEM回應式頁面編輯器設定新的回應式中斷點。

## 建立CSS斷點

首先，在AEM回應式格線CSS中建立回應式AEM網站所遵守的媒體中斷點。

在 `/ui.apps/src/main/content/jcr_root/apps/[app name]/clientlibs/clientlib-grid/less/grid.less` 檔案中，建立要與行動模擬器一起使用的斷點。 請注意 `max-width` 對於每個斷點，因為這會將CSS斷點映射到AEM響應式頁面編輯器斷點。

![建立新的回應式斷點](./assets/responsive-breakpoints/create-new-breakpoints.jpg)

## 自訂範本的斷點

開啟 `ui.content/src/main/content/jcr_root/conf/<app name>/settings/wcm/templates/page-content/structure/.content.xml` 檔案和更新 `cq:responsive/breakpoints` 新斷點節點定義。 每個 [CSS斷點](#create-new-css-breakpoints) 應在下面有相應的節點 `breakpoints` 與 `width` 屬性設定為CSS斷點的 `max-width`.

![自訂範本的回應式中斷點](./assets/responsive-breakpoints/customize-template-breakpoints.jpg)

## 建立模擬器

AEM模擬器必須定義，讓作者能夠選取要在頁面編輯器中編輯的回應式檢視。

在下建立模擬器節點 `/ui.apps/src/main/content/jcr_root/apps/<app name>/emulators`

例如， `/ui.apps/src/main/content/jcr_root/apps/wknd-examples/emulators/phone-landscape`. 從複製引用模擬器節點 `/libs/wcm/mobile/components/emulators` CRXDE Lite並更新復本，以加速節點定義。

![建立新的模擬器](./assets/responsive-breakpoints/create-new-emulators.jpg)

## 建立設備組

將模擬器分組到 [在AEM頁面編輯器中使用](#update-the-templates-device-group).

建立 `/apps/settings/mobile/groups/<name of device group>` 節點結構 `/ui.apps/src/main/content/jcr_root`.

![建立新設備組](./assets/responsive-breakpoints/create-new-device-group.jpg)

建立 `.content.xml` 檔案 `/apps/settings/mobile/groups/<device group name>` 並使用類似以下程式碼來定義新模擬器：

![建立新設備](./assets/responsive-breakpoints/create-new-device.jpg)

## 更新模板的設備組

最後，將裝置群組對應回頁面範本，以便在頁面編輯器中，針對此範本建立的頁面提供模擬器。

開啟 `ui.content/src/main/content/jcr_root/conf/[app name]/settings/wcm/templates/page-content/structure/.content.xml` 檔案和更新 `cq:deviceGroups` 屬性以參考新的行動群組(例如 `cq:deviceGroups="[mobile/groups/customdevices]"`)
