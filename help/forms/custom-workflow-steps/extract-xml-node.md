---
title: 將負載文檔寫入檔案系統
description: 將駐留在負載資料夾下的寫入文檔添加到檔案系統的自定義過程步驟
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-9860
source-git-commit: 160471fdc34439da6c312d65b252eaa941b7c7a2
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 0%

---

# 從提交的資料xml中提取節點

此自定義流程步驟是通過從另一個xml文檔中提取節點來建立新xml文檔。 如果要將已提交的資料與xdp模板合併以生成pdf，則需要使用此功能。 例如，在提交自適應表單時，需要與xdp模板合併的資料位於資料元素內。 在這種情況下，您需要通過提取相應的資料元素來建立另一個xml文檔。

以下螢幕抓圖顯示了需要傳遞到自定義進程步驟的參數
![過程步驟](assets/create-xml-process-step.png)
以下是參數
* Data.xml — 要從中提取節點的xml檔案
* datatomerge.xml — 使用提取的節點建立的新xml
* /afData/afUnboundData/data — 要提取的節點


下面的螢幕抓圖顯示在負載資料夾下建立的datamerge.xml
![建立xml](assets/create-xml.png)

[可從此處下載自定義捆綁包](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)