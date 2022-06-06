---
title: '將表單附件插入資料庫 '
description: 使用工作流在資料庫中插AEM入表單附件。
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Experienced
kt: 10488
source-git-commit: f4e86059d29acf402de5242f033a25f913febf36
workflow-type: tm+mt
source-wordcount: '351'
ht-degree: 1%

---

# 在資料庫中插入表單附件

本文將介紹在MySQL資料庫中儲存表單附件的使用案例。

客戶的一個常見要求是將捕獲的表單資料和表單附件儲存在資料庫表中。
要完成此使用情形，請執行以下步驟

## 建立資料庫表以保存表單資料和附件

建立名為newhire的表以保存表單資料。 注意類型的列名圖片 **隆布洛** 儲存窗體附件
![表模式](assets/insert-picture-table.png)

## 建立表單資料模型

建立了表單資料模型以與MySQL資料庫通信。 您需要建立以下

* [中的JDBC資料源AEM](./data-integration-technical-video-setup.md)
* [基於JDBC資料源的表單資料模型](./jdbc-data-model-technical-video-use.md)

## 建立工作流

將自適應表單配置為提交到AEM工作流，您可以選擇將表單附件保存在工作流變數中，或將附件保存在負載下的指定資料夾中。 對於此用例，我們需要將附件保存在「文檔的ArrayList」類型的工作流變數中。 從此ArrayList中，我們需要提取第一項並初始化文檔變數。 調用的工作流變數 **文檔清單** 和 **員工照片** 的子菜單。
當提交自適應表單以觸發工作流時，工作流中的一個步驟將使用ECMA指令碼初始化employeePhoto變數。 以下是ECMA指令碼代碼

```javascript
log.info("executing script now...");
var metaDataMap = graniteWorkItem.getWorkflow().getWorkflowData().getMetaDataMap();
var listOfAttachments = [];
// Make sure you have a workflow variable caled listOfDocuments defined
listOfAttachments = metaDataMap.get("listOfDocuments");
log.info("$$$  got listOfAttachments");
//Make sure you have a workflow variable caled employeePhoto defined
var employeePhoto = listOfAttachments[0];
metaDataMap.put("employeePhoto", employeePhoto);
log.info("Employee Photo updated");
```

工作流的下一步是使用「調用表單資料模型」服務元件將資料和表單附件插入表格。
![插入 — pic](assets/fdm-insert-pic.png)
[可以從此處下載包含示例ecma指令碼的完整工作流](assets/add-new-employee.zip)。

>[!NOTE]
> 您必須建立新的基於JDBC的表單資料模型，並在工作流中使用該表單資料模型

## 建立自適應窗體

根據在前一步中建立的表單資料模型建立自適應表單。 拖放窗體上的窗體資料模型元素。 配置表單提交以觸發工作流並指定以下屬性，如下面螢幕抓圖所示
![表單附件](assets/form-attachments.png)