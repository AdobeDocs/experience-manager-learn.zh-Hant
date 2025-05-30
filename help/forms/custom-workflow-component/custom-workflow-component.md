---
title: 建立工作流程元件以將表單附件儲存至檔案系統
description: 使用自訂工作流程元件將最適化表單附件寫入檔案系統
feature: Workflow
version: Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2021-11-28T00:00:00Z
exl-id: acc701ec-b57d-4c20-8f97-a5a69bb180cd
duration: 70
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '360'
ht-degree: 0%

---

# 自訂工作流程元件

本教學課程適用於需要建立自訂工作流程元件的AEM Forms客戶。 工作流程元件將設定為執行上一步驟中撰寫的程式碼。 工作流程元件能夠指定程式碼的流程引數。 在本文中，我們將探索與程式碼相關聯的工作流程元件。


[下載自訂工作流程元件](assets/saveFiles.zip)
使用封裝管理員[&#128279;](http://localhost:4502/crx/packmgr/index.jsp)匯入工作流程元件

自訂工作流程元件位於/apps/AEMFormsDemoListings/workflowcomponent/SaveFiles

選取SaveFiles節點並檢查其屬性

**componentGroup** — 此屬性的值決定工作流程元件的類別。

**jcr：Title** — 這是工作流程元件的標題。

**sling：resourceSuperType**&#x200B;此屬性的值將決定此元件的繼承。 在此案例中，我們繼承自流程元件


![component-properties](assets/component-properties1.png)

## cq：dialog

對話方塊可用來允許作者與元件互動。 cq：dialog位於SaveFiles節點下
![cq-dialog](assets/cq-dialog.png)

專案節點底下的節點代表元件標籤，作者會透過這些標籤與元件互動。 「常用」和「流程」標籤會隱藏。 「一般」和「引數」標籤可見。

流程的流程引數位於processargs節點下

![process-args](assets/process-arguments.png)

作者會指定引數，如下方熒幕擷取畫面所示
![工作流程元件](assets/custom-workflow-component.png)

這些值會儲存為中繼資料節點的屬性。 例如，值&#x200B;**c：\formsattachments**&#x200B;會儲存在中繼資料節點的屬性saveToLocation中
![儲存位置](assets/save-to-location.png)

## cq：editConfig

cq：EditConfig只是元件根目錄下主要型別為cq：EditConfig且名稱為cq：editConfig的節點
在元件節點底下新增cq：EditConfig型別的cq：editConfig節點（cq：Component型別），即可設定元件的編輯行為

![edit-config](assets/cq-edit-config.png)

cq：formParameters （節點型別nt：unstructured）：定義新增至對話方塊表單的其他引數。


注意cq：formParameters節點的屬性
![from-parameters-properties](assets/form-parameters-properties.png)

PROCESS屬性的值表示將與工作流程元件相關聯的Java程式碼。
