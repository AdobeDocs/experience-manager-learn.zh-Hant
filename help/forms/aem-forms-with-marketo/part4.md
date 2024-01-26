---
title: AEM Forms與Marketo （第4部分）
description: 教學課程使用AEM Forms表單資料模型將AEM Forms與Marketo整合。
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="整合" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: 6b44e6b2-15f7-45b2-8d21-d47f122c809d
duration: 84
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 0%

---

# 使用表單資料模型建立最適化表單

下一步是建立最適化表單，並以先前步驟建立的表單資料模型為基礎。
使用者輸入銷售機會ID，並叫用以Tab鍵瀏覽Marketo服務以依ID取得銷售機會。 然後，服務作業的結果會對應至最適化Forms的適當欄位。

1. 建立最適化表單，並以空白表單範本為基礎，將其與先前步驟中建立的表單資料模型建立關聯。
1. 在編輯模式下開啟表單
1. 將TextField元件和Panel元件拖放到最適化表單上。 將TextField元件的標題設為「輸入銷售機會Id」，並將名稱設為「LeadId」
1. 將2個TextField元件拖放到Panel元件上
1. 將2個Textfield元件的「名稱」和「標題」設定為「名字」和「姓氏」
1. 將最小值設定為1，最大值設定為–1，將面板元件設定為可重複的元件。 這是Marketo服務傳回一系列Lead物件時的必要專案，而且您必須具備可重複的元件才能顯示結果。 不過，在此案例中，我們只得到一個Lead物件，因為我們依據Lead物件的ID來搜尋它。
1. 在LeadId欄位上建立規則，如下圖所示
1. 預覽表單，並在LeadID欄位中輸入有效的Lead ID，然後取出標籤。 [名字]和[姓氏]欄位應填入服務電話的結果。

下列熒幕擷圖說明規則編輯器設定

![規則編輯器](assets/ruleeditor.jfif)

## 偵錯

如果您使用本文提供的套件組合，建議您啟用 [偵錯記錄](http://localhost:4502/system/console/slinglog) 下列類別：

+ `com.marketoandforms.core.impl.MarketoServiceImpl`
+ `com.marketoandforms.core.MarketoConfigurationService`

## 恭喜

您已成功使用AEM Forms表單資料模型將AEM Forms與Marketo整合。