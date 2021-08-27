---
title: '填入最適化表單表格 '
description: 將表單資料模型服務叫用的結果填入適用性表單表格
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: User
level: Intermediate
source-git-commit: 0049c9fd864bd4dd4f8c33b1e40e94aad3ffc5b9
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 0%

---


# 將表單資料模型服務呼叫的結果填入最適化表單表格

[此處托管即](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
時表單本文探討如何從表單資料模型服務呼叫擷取資料，以填入最適化表單表格。我們將在表格中建立攤銷時間表，列出一筆抵押貸款的每筆定期付款。 攤銷結果由我們的表單資料模型返回。 如螢幕擷取所示，表單資料模型的服務會在計算按鈕的點按事件上叫用。 如螢幕擷取畫面所示，適當地映射服務呼叫的輸入和輸出參數。 輸出映射到行1的列
![clickevent](assets/amortization.PNG)

Row1會根據服務呼叫傳回的資料而設定為成長。 請注意此處指定的重複設定。 值–1表示表中的行數不限
![Row1](assets/rowconfiguration.PNG)

## 在您的伺服器上部署

[按此處指定的方](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md)
[式安裝Tomcat此zip檔案中包含的SampleRest.war檔案](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/assets/common-osgi-bundles/sample-rest.zip)
[使用AEM套件管理器安裝 ](assets/amortizationschedule.zip) 資產開啟攤銷排程表
[](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
單輸入適當值，然後按一下計算應填入您的表單中的攤銷排程

