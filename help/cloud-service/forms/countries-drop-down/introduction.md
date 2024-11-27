---
title: 建立國家/地區下拉式清單元件
description: 根據aem forms核心下拉式清單元件，建立國家/地區下拉式清單元件。
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Formsas a Cloud Service" before-title="false"
jira: KT-16517
source-git-commit: f9a1fb40aabb6fdc1157e1f2576f9c0d9cf1b099
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 1%

---

# 根據下拉式元件建立國家/地區下拉式元件

在Adobe Experience Manager (AEM)中建立新的核心元件是令人興奮的過程，涉及幾個步驟，包括定義元件結構、自訂對話方塊以及實施動態功能的Sling模型。

在本教學課程結束時，您將已掌握如何：

* 建立並使用Sling模型以動態擷取資料。
* 透過新增欄位和隱藏其他欄位來自訂cq-dialog。
* 定義專為重複使用量身打造的強大元件結構。

名為「國家/地區」的元件可讓使用者選取大陸，並在下拉式清單中動態填入與所選大陸對應的國家/地區。 這將以現成可用的下拉式清單元件為基礎，針對此特定使用案例進行增強。

讓我們開始深入探討，並建立此動態且功能強大的元件！

## 先決條件

在Adobe Experience Manager (AEM)中建立新的核心元件需要符合數個先決條件，才能確保順利的開發程式。 以下是您開始使用前所需的專案：

* AEM開發環境：可在當地執行之功能雲端就緒安裝
* 存取AEM開發工具，例如Visual Studio Code或IntelliJ
* 使用最新原型的MAven設定和AEM專案
* AEM概念的基本知識
* 基本工具和設定，例如Git存放庫、JDK的正確版本


## 後續步驟

[建立元件結構](./component.md)
