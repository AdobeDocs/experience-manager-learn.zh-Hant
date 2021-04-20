---
title: WorkflowAEM中的變數[Part4]
seo-title: WorkflowAEM中的變數[Part4]
description: 在AEM工作流程中使用xml,json,arraylist,document類型的變數
seo-description: 在AEM工作流程中使用xml,json,arraylist,document類型的變數
feature: Workflow
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '475'
ht-degree: 0%

---


# 工作流中的ArrayListAEM變數

在AEM Forms6.5中引入了ArrayList類型的變數。使用ArrayList變數的常見使用案例是定義要用於AssignTask的自定義路由。

為了在工作流中使用ArrayList變AEM量，您需要建立一個最適化表單，該表單在提交的資料中生成重複元素。 常見的做法是定義包含陣列元素的方案。 為了本文，我已建立包含陣列元素的簡單JSON結構描述。 使用案例是員工填寫費用報表。 在費用報表中，我們將捕獲提交者的經理名稱和經理的經理名稱。 管理器的名稱儲存在名為managerchain的陣列中。 以下螢幕截圖顯示費用報表表單和來自Adaptive Forms提交的資料。

![開銷報告](assets/expensereport.jpg)

以下是來自最適化表單提交的資料。 最適化表單是以JSON結構描述為基礎，系結至結構描述的資料會儲存在afBoundData元素的資料元素下。 managerchain是一個陣列，我們需要在ArrayList中填充該對象的名稱元素。

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

若要初始化子類型字串的ArrayList變數，您可以使用JSON Dot Notation或XPath對應模式。 下列螢幕擷取顯示您使用JSON點記法填入名為CustomRoutes的ArrayList變數。 請確定您指向的是陣列物件中的元素，如下方螢幕擷取所示。 我們將用managerchain陣列對象的名稱填充CustomRoutes ArrayList。
然後， CustomRoutes ArrayList用於在AssignTask元件中填充路由
![customroutes](assets/arraylist.jpg)
使用提交資料中的值初始化CustomRoutes ArrayList變數後，AssignTask元件的路由將使用CustomRoutes變數填充。 以下螢幕抓圖顯示AssignTask中的自定義路由
![asingtask](assets/customactions.jpg)

若要在您的系統上測試此工作流程，請依照下列步驟進行

* 下載ArrayListVariable.zip檔案並儲存至您的檔案系統
* [使用套件管](assets/arraylistvariable.zip) 理員匯入郵AEM遞區號檔案
* [開啟TravelExpenseReport表單](http://localhost:4502/content/dam/formsanddocuments/helpx/travelexpensereport/jcr:content?wcmmode=disabled)
* 輸入一些費用和2名經理的姓名
* 按一下提交按鈕
* [開啟您的收件匣](http://localhost:4502/aem/inbox)
* 您應看到名為「指派給費用管理員」的新任務
* 開啟與任務關聯的表單
* 您應看到兩個具有管理員名稱的自訂路由
   [探索ReviewExpenseReportWorkflow。](http://localhost:4502/editor.html/conf/global/settings/workflow/models/ReviewExpenseReport.html) 此工作流程使用ArrayList變數、JSON類型變數、Or-Split元件中的規則編輯器
