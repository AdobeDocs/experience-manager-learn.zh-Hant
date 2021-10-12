---
title: AEM Forms與Marketo（第4部分）
description: 使用AEM Forms表單資料模型整合AEM Forms與Marketo的教學課程。
feature: Adaptive Forms, Form Data Model
version: 6.3,6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 6b44e6b2-15f7-45b2-8d21-d47f122c809d
source-git-commit: 020852f16de0cdb1e17e19ad989dabf37b7f61f5
workflow-type: tm+mt
source-wordcount: '317'
ht-degree: 1%

---

# 使用表單資料模型建立最適化表單

下一步是建立最適化表單，並以先前步驟中建立的表單資料模型為基礎。
使用者會輸入銷售機會ID，並在Tab上顯示Marketo服務以依ID取得銷售機會時，將會叫用。 然後，服務操作的結果將映射到自適應Forms的適當欄位。

1. 建立最適化表單並以「空白表單範本」為基礎，將其與先前步驟中建立的表單資料模型建立關聯。
1. 在編輯模式中開啟表單
1. 將TextField元件和面板元件拖放至最適化表單。 將TextField元件的標題設定為「Enter Lead Id」，並將其名稱設定為「LeadId」
1. 將2個TextField元件拖放至「面板」元件
1. 將2個文本欄位元件的名稱和標題設定為FirstName和LastName
1. 將「最小值為1」和「最大值為–1」設定為可重複元件。 由於Marketo服務傳回銷售機會物件陣列，且您需要可重複的元件來顯示結果，因此此為必要項目。 但是，在此情況下，我們將只返回一個Lead對象，因為我們正在按其ID搜索Lead對象。
1. 在LeadId欄位上建立規則，如下圖所示
1. 預覽表單，在LeadID欄位中輸入有效的Lead ID，然後將標籤選出。 名字和姓氏欄位應填入服務呼叫的結果。

以下螢幕擷圖說明規則編輯器設定

![規則編輯器](assets/ruleeditor.jfif)

## 除錯

如果您使用本文提供的套件組合，則可能要為以下類啟用[debug logs](http://localhost:4502/system/console/slinglog):

+ `com.marketoandforms.core.impl.MarketoServiceImpl`
+ `com.marketoandforms.core.MarketoConfigurationService`
