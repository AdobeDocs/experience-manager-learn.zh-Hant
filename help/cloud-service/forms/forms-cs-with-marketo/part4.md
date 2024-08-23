---
title: AEM Forms與Marketo （第4部分）
description: 教學課程使用AEM Forms表單資料模型將AEM Forms與Marketo整合。
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="整合" type="positive"
badgeVersions: label="AEM Formsas a Cloud Service" before-title="false"
exl-id: 6b44e6b2-15f7-45b2-8d21-d47f122c809d
duration: 68
source-git-commit: 426020f59c7103829b7b7b74acb0ddb7159b39fa
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 0%

---

# 測試整合

我們將建立簡單的表單擷取，並從Market顯示Lead物件，以測試整合。
>[!NOTE]
>
>此功能已根據基礎元件在表單上測試。

## 建立最適化表單

1. 建立最適化表單，並以空白表單範本為基礎，將其與先前步驟中建立的表單資料模型建立關聯。
1. 在編輯模式中開啟表單。
1. 將TextField元件和Panel元件拖放到最適化表單上。 將TextField元件的標題設為「輸入銷售機會Id」，並將名稱設為「LeadId」
1. 將2個TextField元件拖放到Panel元件上
1. 將2個Textfield元件的「名稱」和「標題」設定為「名字」和「姓氏」
1. 將最小值設定為1，最大值設定為–1，將面板元件設定為可重複的元件。 這是Marketo服務傳回一系列Lead物件時的必要專案，而且您必須具備可重複的元件才能顯示結果。 不過，在此案例中，我們只得到一個Lead物件，因為我們依據Lead物件的ID來搜尋它。
1. 在LeadId欄位上建立規則，如下圖所示
1. 預覽表單，並在LeadID欄位中輸入有效的Lead ID，然後取出標籤。 [名字]和[姓氏]欄位應填入服務電話的結果。

下列熒幕擷圖說明規則編輯器設定

![ruleeditor](assets/ruleeditor.png)


## 恭喜

您已成功使用AEM Forms表單資料模型將AEM Forms與Marketo整合。