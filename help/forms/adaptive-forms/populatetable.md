---
title: '填入最適化表單表格 '
seo-title: 填入最適化表單表格
description: 用表單資料模型服務調用的結果填充自適應表單表
seo-description: 用表單資料模型服務調用的結果填充自適應表單表
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: 449202af47b6bbcd9f860d5c5391d1f7096d489e
workflow-type: tm+mt
source-wordcount: '254'
ht-degree: 0%

---


# 用表單資料模型服務調用的結果填充自適應表單表

[即時表單位於此處](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)。在本文中，我們探討從表單資料模型服務調用中提取資料來填充自適應表單表。 我們將在一個表格中建立一個攤銷時間表，其中列出按揭的定期付款。 攤銷結果由我們的表單資料模型傳回。 表單資料模型的服務會在計算按鈕的點按事件上呼叫，如螢幕擷取所示。 如螢幕抓圖所示，適當地映射服務調用的輸入和輸出參數。 輸出映射到Row1clickevent的列![。](assets/amortization.PNG)

Row1會根據服務呼叫傳回的資料而設定成成長。 請注意此處指定的重複設定。 值-1表示表![Row1中的行數不限](assets/rowconfiguration.PNG)

## 將它部署在您的伺服器上

[按此處指定的方式安裝](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md)[Tomcat部署SampleRest.war檔案使用AEM套件管理器安裝](https://forms.enablementadobe.com/content/DemoServerBundles/SampleRest.war)資產開啟攤銷排程表[](assets/amortizationschedule.zip)[](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)Enter the appropriate value and click on calculate攤銷排程應填入您的表單

