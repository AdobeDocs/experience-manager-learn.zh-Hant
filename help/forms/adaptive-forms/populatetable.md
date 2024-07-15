---
title: 填入最適化表單表格
description: 將表單資料模型服務引動的結果填入最適化表單表格中
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: User
level: Intermediate
exl-id: 6e4b901a-6534-4c34-b315-2f2620b74247
last-substantial-update: 2019-06-09T00:00:00Z
duration: 45
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 0%

---

# 將表單資料模型服務引動的結果填入最適化表單表格中

[即時表單託管於此處](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
在本文中，我們將透過從表單資料模型服務引動中擷取資料，來瞭解如何填入調適型表單表格。 我們將在表格中建立攤銷排程，該表格會列出一段時間內按揭貸款上的每筆定期付款。 我們的表單資料模型會傳回攤銷結果。 會在計算按鈕的點選事件上叫用表單資料模型的服務，如熒幕擷圖所示。 如熒幕擷取畫面所示，服務呼叫的輸入和輸出引數已適當對應。 輸出對應至Row1的欄
![clickevent](assets/amortization.PNG)

Row1設定為依據服務呼叫傳回的資料而成長。 請注意此處指定的重複設定。 值–1表示表格中的列數不受限制
![列1](assets/rowconfiguration.PNG)

## 在您的伺服器上部署此專案

[依照此處指定的方式安裝Tomcat](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md)
[在您的Tomcat中部署包含在這個zip檔案中的SampleRest.war檔案](assets/sample-rest.zip)
[使用AEM封裝管理員安裝資產](assets/amortizationschedule.zip)
[開啟分期付款排程表](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
輸入適當的值，然後按一下計算
攤銷排程應填入您的表單中
