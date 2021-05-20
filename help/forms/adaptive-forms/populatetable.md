---
title: '填入最適化表單表格 '
seo-title: 填入最適化表單表格
description: 將表單資料模型服務叫用的結果填入適用性表單表格
seo-description: 將表單資料模型服務叫用的結果填入適用性表單表格
feature: 適用性表單
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: 開發
role: Business Practitioner
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '257'
ht-degree: 1%

---


# 將表單資料模型服務呼叫的結果填入最適化表單表格

[此處托管即](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
時表單本文探討如何從表單資料模型服務呼叫擷取資料，以填入最適化表單表格。我們將在表格中建立攤銷時間表，列出一筆抵押貸款的每筆定期付款。 攤銷結果由我們的表單資料模型返回。 如螢幕擷取所示，表單資料模型的服務會在計算按鈕的點按事件上叫用。 如螢幕擷取畫面所示，適當地映射服務呼叫的輸入和輸出參數。 輸出映射到行1的列
![clickevent](assets/amortization.PNG)

Row1會根據服務呼叫傳回的資料而設定為成長。 請注意此處指定的重複設定。 值–1表示表中的行數不限
![Row1](assets/rowconfiguration.PNG)

## 在您的伺服器上部署

[按此處指定的方](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md)
[式安裝Tomcat部署SampleRest.war](https://forms.enablementadobe.com/content/DemoServerBundles/SampleRest.war)
[檔案使用AEM套件管 ](assets/amortizationschedule.zip) 理器安裝資產開啟攤銷排程
[](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
表單輸入適當值，然後按一下計算應在您的表單中填入攤銷排程

