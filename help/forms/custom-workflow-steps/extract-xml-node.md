---
title: 從提交的資料xml中提取節點
description: 將駐留在裝載資料夾下的寫入文檔添加到檔案系統的自定義過程步驟
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-9860
exl-id: 5282034f-275a-479d-aacb-fc5387da793d
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '179'
ht-degree: 0%

---

# 從提交的資料xml中提取節點

此自定義進程步驟是通過從另一個xml文檔中提取節點來建立新xml文檔。 當您想要將提交的資料與xdp範本合併，以產生pdf時，需要使用此功能。 例如，當您提交最適化表單時，需要與xdp範本合併的資料位於資料元素內。 在這種情況下，您需要通過提取適當的資料元素來建立另一個xml文檔。

下列螢幕擷取畫面顯示您需要傳遞至自訂程式步驟的引數
![過程步驟](assets/create-xml-process-step.png)
以下是參數
* Data.xml — 要從中提取節點的xml檔案
* datatomerge.xml — 使用提取的節點建立的新xml
* /afData/afUnboundData/data — 要提取的節點


下列螢幕擷取畫面會顯示正在裝載資料夾下建立的datamerge.xml
![create-xml](assets/create-xml.png)

[自訂套件可從此處下載](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
