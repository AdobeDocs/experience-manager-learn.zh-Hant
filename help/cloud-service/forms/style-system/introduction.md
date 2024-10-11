---
title: 在AEM Forms中使用樣式系統
description: 建立按鈕元件的樣式變化
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Formsas a Cloud Service" before-title="false"
jira: KT-16276
source-git-commit: 86d282b426402c9ad6be84e9db92598d0dc54f85
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 1%

---

# 簡介

Adobe Experience Manager (AEM)中的樣式系統可讓使用者建立元件的多個視覺變體，然後選取在製作表單時要使用的樣式。 如此可讓元件更具彈性且可重複使用，而不需為每個樣式建立自訂元件。

本文會協助您建立按鈕元件的變數，並在您使用Cloud Manager將變更推送至雲端例項之前，測試本機雲端就緒環境中的變數。

此熒幕擷取畫面顯示表單作者可用的按鈕元件的2種樣式變化。


![按鈕變化](assets/button-variations.png)

## 先決條件

* 具有核心元件的AEM Forms雲端就緒例項。
* 複製主題：複製主題時，您必須很熟悉。 為了這個教學課程的目的，我們已複製[畫盤主題](https://github.com/adobe/aem-forms-theme-easel)。 您可以複製任何可用的主題以符合您的需求。

* 安裝最新版的Apache Maven。 Apache Maven是常用於Java™專案的組建自動化工具。 安裝最新版本可確保您擁有佈景主題自訂的必要相依性。
* 安裝純文字編輯器。 例如，Microsoft® Visual Studio Code。 使用純文字編輯器(例如Microsoft® Visual Studio Code)可提供方便使用的環境，用於編輯和修改佈景主題檔案。



## 後續步驟

[建立樣式原則](./style-policy.md)
