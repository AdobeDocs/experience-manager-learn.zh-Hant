---
title: Workflow[AEMPart4]中的變數
description: 在工作流中使用XML、JSON、ArrayList和Document類型的變AEM量
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: 269e43f7-24cf-4786-9439-f51bfe91d39c
source-git-commit: 4b47daf82e27f6bea4be30e3cdd132f497f4c609
workflow-type: tm+mt
source-wordcount: '456'
ht-degree: 0%

---

# 工作流中的ArrayListAEM變數

在AEM Forms6.5中引入了ArrayList類型的變數。使用ArrayList變數的常見用例是定義要在AssignTask中使用的自定義路由。

要在工作流中使用ArrayList變AEM量，需要建立自適應表單，該表單在提交的資料中生成重複元素。 通常的做法是定義包含陣列元素的架構。 為本文的目的，我建立了一個包含陣列元素的簡單JSON架構。 使用案例是員工填寫費用報表。 在費用報表中，我們將捕獲提交者的經理名稱和經理的經理姓名。 管理器的名稱儲存在名為managerchain的陣列中。 下面的螢幕快照顯示了支出報表表單和Adaptive Forms提交中的資料。

![支出報表](assets/expensereport.jpg)

以下是自適應表單提交中的資料。 自適應表單基於JSON架構，綁定到架構的資料儲存在afBoundData元素的資料元素下。 管理器鏈是一個陣列，我們需要用管理器鏈陣列內對象的名稱元素填充ArrayList。

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

要初始化子類型字串的ArrayList變數，可以使用JSON點符號或XPath映射模式。 以下螢幕快照顯示您使用JSON點符號填充名為CustomRoutes的ArrayList變數。 確保指向陣列對象中的元素，如下面的螢幕快照所示。 我們正在用managerchain陣列對象的名稱填充CustomRoutes ArrayList。
然後，CustomRoutes ArrayList用於填充AssignTask元件中的路由
![自定義路由](assets/arraylist.jpg)
一旦CustomRoutes ArrayList變數用已提交資料中的值初始化，AssignTask元件的Routes就會使用CustomRoutes變數填充。 下面的螢幕快照顯示了AssignTask中的自定義路由
![任務](assets/customactions.jpg)

要在系統上test此工作流，請執行以下步驟

* 下載ArrayListVariable.zip檔案並將其保存到檔案系統
* [導入ZIP檔案](assets/arraylistvariable.zip) 使用包管AEM理器
* [開啟TravelExpenseReport表單](http://localhost:4502/content/dam/formsanddocuments/helpx/travelexpensereport/jcr:content?wcmmode=disabled)
* 輸入兩個費用和兩個經理的姓名
* 按一下提交按鈕
* [開啟收件箱](http://localhost:4502/aem/inbox)
* 您應看到標題為「分配給費用管理員」的新任務
* 開啟與任務關聯的表單
* 您應看到兩個具有管理器名稱的自定義路由
   [瀏覽ReviewExpenseReportWorkflow。](http://localhost:4502/editor.html/conf/global/settings/workflow/models/ReviewExpenseReport.html) 此工作流使用ArrayList變數、JSON類型變數、或拆分元件中的規則編輯器
