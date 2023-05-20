---
title: 填充自適應表單表
description: 使用表單資料模型服務調用的結果填充自適應表單表
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

# 使用表單資料模型服務調用的結果填充自適應表單表

[此處托管即時表單](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
本文通過從表單資料模型服務調用中提取資料來研究如何填充自適應表單表。 我們將在一個表中建立一個攤銷時間表，列出按期償還的每筆抵押貸款。 攤銷結果由我們的表單資料模型返回。 如螢幕快照所示，在「計算」按鈕的按一下事件上調用「表單資料模型」的服務。 服務調用的輸入和輸出參數被適當地映射，如螢幕抓圖所示。 輸出映射到Row1的列
![克拉克文](assets/amortization.PNG)

根據服務呼叫返回的資料，將行1配置為增長。 請注意此處指定的重複設定。 值–1表示表中行數不限
![行1](assets/rowconfiguration.PNG)

## 在您的伺服器上部署此

[按此處指定的方式安裝Tomcat](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md)
[部署Tomcat中此zip檔案中包含的SampleRest.war檔案](assets/sample-rest.zip)
[安裝資產 ](assets/amortizationschedule.zip) 使用包AEM管理器
[開啟攤銷計畫表](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
輸入相應的值，然後按一下計算應在窗體中填寫攤銷計畫
