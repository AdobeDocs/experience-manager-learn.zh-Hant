---
title: 填入最適化表單表格
description: 將表單資料模型服務叫用的結果填入適用性表單表格
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: User
level: Intermediate
exl-id: 6e4b901a-6534-4c34-b315-2f2620b74247
last-substantial-update: 2019-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '240'
ht-degree: 0%

---

# 將表單資料模型服務呼叫的結果填入最適化表單表格

[即時表單托管於此處](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
本文探討如何從表單資料模型服務呼叫中擷取資料，以填入最適化表單表格。 我們將在表格中建立攤銷時間表，列出一筆抵押貸款的每筆定期付款。 攤銷結果由我們的表單資料模型返回。 如螢幕擷取所示，表單資料模型的服務會在計算按鈕的點按事件上叫用。 如螢幕擷取畫面所示，適當地映射服務呼叫的輸入和輸出參數。 輸出映射到行1的列
![cleckevent](assets/amortization.PNG)

Row1會根據服務呼叫傳回的資料而設定為成長。 請注意此處指定的重複設定。 值–1表示表中的行數不限
![列1](assets/rowconfiguration.PNG)

## 在伺服器上部署

[按此處指定的方式安裝Tomcat](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md)
[在Tomcat中部署此zip檔案中包含的SampleRest.war檔案](assets/sample-rest.zip)
[安裝資產 ](assets/amortizationschedule.zip) 使用AEM套件管理器
[開啟攤銷計畫表單](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
輸入相應值，然後按一下「計算攤銷計畫」將填入您的窗體中
