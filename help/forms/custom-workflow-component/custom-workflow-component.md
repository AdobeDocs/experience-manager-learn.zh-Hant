---
title: 建立工作流元件以將表單附件保存到檔案系統
description: 使用自訂工作流程元件將適用性表單附件寫入檔案系統
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2021-11-28T00:00:00Z
source-git-commit: 09b00a7edf2f4c90c6cb2178161c6d7e0c9432e8
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 1%

---

# 自訂工作流程元件

本教學課程適用於需要建立自訂工作流程元件的AEM Forms客戶。 工作流程元件將設定為執行上一步中寫入的程式碼。 工作流元件可以指定代碼的進程參數。 本文將探討與程式碼相關聯的工作流程元件。


[下載自訂工作流程元件](assets/saveFiles.zip)
匯入工作流程元件 [使用套件管理器](http://localhost:4502/crx/packmgr/index.jsp)

自定義工作流元件位於/apps/AEMFormsDemoListings/workflowcomponent/SaveFiles中

選擇SaveFiles節點並檢查其屬性

**componentGroup**  — 此屬性的值決定了工作流元件的類別。

**jcr:Title**  — 這是工作流程元件的標題。

**sling:resourceSuperType** 此屬性的值將決定此元件的繼承。 在此情況下，我們將從流程元件中繼承


![component-properties](assets/component-properties1.png)

## cq:dialog

對話方塊可讓作者與元件互動。 cq:dialog位於SaveFiles節點下
![cq-dialog](assets/cq-dialog.png)

項目節點下的節點代表元件的索引標籤，供作者與元件互動。 公共頁簽和進程頁簽被隱藏。 「常見」和「參數」頁簽可見。

進程的進程參數位於processargs節點下

![process-args](assets/process-arguments.png)

作者會指定引數，如下方螢幕擷取畫面所示
![工作流程元件](assets/custom-workflow-component.png)

這些值會儲存為中繼資料節點的屬性。 例如，值 **c:\formsattachments** 將儲存在元資料節點的saveToLocation屬性中
![儲存位置](assets/save-to-location.png)

## cq:editConfig

cq:EditConfig只是主要類型cq:EditConfig的節點，元件根下的名稱cq:editConfig元件的編輯行為是通過在元件節點（類型cq:Component）下添加cq:editConfig類型的cq:editConfig節點來配置的

![edit-config](assets/cq-edit-config.png)

cq:formParameters（節點類型nt:unstructured）:會定義新增至對話方塊表單的其他參數。


請注意cq:formParameters節點的屬性
![from-parameters-properties](assets/form-parameters-properties.png)

屬性PROCESS的值指示將與工作流元件關聯的Java代碼。






