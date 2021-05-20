---
title: AEM Workflow[Part4]中的變數
seo-title: AEM Workflow[Part4]中的變數
description: 在aem工作流程中使用xml,json,arraylist,document類型的變數
seo-description: 在aem工作流程中使用xml,json,arraylist,document類型的變數
feature: 工作流程
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
topic: 開發
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '473'
ht-degree: 0%

---


# AEM工作流程中的ArrayList變數

AEM Forms 6.5引入了ArrayList類型的變數。使用ArrayList變數的常見使用案例是定義要用於AssignTask的自定義路由。

若要在AEM工作流程中使用ArrayList變數，您需要建立適用性表單，該表單會在提交的資料中產生重複元素。 常見的作法是定義包含陣列元素的架構。 為了本文的目的，我已建立了包含陣列元素的簡單JSON結構描述。 使用案例是員工填寫費用報表。 在費用報表中，我們將捕獲提交者的經理名稱和經理的經理名稱。 管理員的名稱儲存在名為managerchain的陣列中。 下面的螢幕截圖顯示費用報表表單，以及適用性Forms提交中的資料。

![支出報告](assets/expensereport.jpg)

以下是最適化表單提交的資料。 適用性表單以JSON結構描述為基礎，系結至結構描述的資料會儲存在afBoundData元素的資料元素下。 managerchain是陣列，我們需要用managerchain陣列內對象的名稱元素填充ArrayList。

```json
{
    "afData": {
        "afUnboundData": {
            "data": {
                "numericbox_2762582281554154833426": 700
            }
        },
        "afBoundData": {
            "data": {
                "Employee": {
                    "Name": "Conrad Simms",
                    "Department": "IT",
                    "managerchain": [{
                        "name": "Gloria Rios"
                    }, {
                        "name": "John Jacobs"
                    }]
                },
                "expense": [{
                    "description": "Hotel",
                    "amount": 300
                }, {
                    "description": "Air Fare",
                    "amount": 400
                }]
            }
        },
        "afSubmissionInfo": {
            "computedMetaInfo": {},
            "stateOverrides": {},
            "signers": {},
            "afPath": "/content/dam/formsanddocuments/helpx/travelexpensereport",
            "afSubmissionTime": "20190402102953"
            }
        }
}
```

要初始化子類型字串的ArrayList變數，可以使用JSON點記號或XPath映射模式。 以下螢幕截圖顯示您使用JSON點記號填入名為CustomRoutes的ArrayList變數。 請確定您指向陣列物件中的元素，如下方螢幕擷取所示。 我們正在使用managerchain陣列對象的名稱填充CustomRoutes ArrayList。
然後， CustomRoutes ArrayList將用於填充AssignTask元件中的路由
![customroutes](assets/arraylist.jpg)
使用提交資料中的值初始化CustomRoutes ArrayList變數後， AssignTask元件的Routes將使用CustomRoutes變數填充。 下面的螢幕截圖顯示AssignTask中的自定義路由
![asingtask](assets/customactions.jpg)

要在您的系統上測試此工作流，請執行以下步驟

* 下載ArrayListVariable.zip檔案並儲存至您的檔案系統
* [使用AEM套](assets/arraylistvariable.zip) 件管理器匯入zip檔案
* [開啟TravelExpenseReport表單](http://localhost:4502/content/dam/formsanddocuments/helpx/travelexpensereport/jcr:content?wcmmode=disabled)
* 輸入一些費用和2名經理的姓名
* 點擊提交按鈕
* [開啟收件匣](http://localhost:4502/aem/inbox)
* 您應該會看到名為「分配給費用管理員」的新任務
* 開啟與任務關聯的表單
* 您應該會看到兩個具有管理員名稱的自訂路由
   [探索ReviewExpenseReportWorkflow。](http://localhost:4502/editor.html/conf/global/settings/workflow/models/ReviewExpenseReport.html) 此工作流程使用ArrayList變數、JSON類型變數、或分割元件中的規則編輯器
