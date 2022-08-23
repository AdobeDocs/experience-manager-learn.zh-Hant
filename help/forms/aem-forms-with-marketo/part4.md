---
title: AEM Forms與Marketo（四）
description: 使用AEM Forms表單資料模型將Marketo與AEM Forms整合的教程。
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 6b44e6b2-15f7-45b2-8d21-d47f122c809d
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '317'
ht-degree: 1%

---

# 使用表單資料模型建立自適應表單

下一步是建立一個自適應表單，並將其基於在前一步中建立的表單資料模型。
用戶將輸入銷售線索ID，在開啟Marketo服務時將調用按ID獲取銷售線索。 然後將服務操作的結果映射到自適應Forms的適當欄位。

1. 建立一個自適應表單並將其基於「空白表單模板」，將其與在前一步中建立的表單資料模型相關聯。
1. 在編輯模式下開啟窗體
1. 將TextField元件和面板元件拖放到「自適應表單」中。 將TextField元件的標題「輸入潛在顧客標識」設定為「潛在顧客標識」
1. 將2個TextField元件拖放到「面板」元件上
1. 將兩個文本欄位元件的名稱和標題設定為FirstName和LastName
1. 將「Panel（面板）」元件設定為可重複元件，方法是將「Minimum（最小）」設定為1，將「Maximum（最大）」設定為–1。 這是必需的，因為Marketo服務返回一組Lead對象，並且您需要一個可重複的元件來顯示結果。 但是，在這種情況下，我們將只獲取一個Lead對象，因為我們正在按其ID搜索Lead對象。
1. 在LeadId欄位上建立規則，如下圖所示
1. 預覽表單，並在LeadID欄位和標籤輸出中輸入有效的Lead Id。 「名」和「姓」欄位應填入服務調用的結果。

以下螢幕快照說明了規則編輯器設定

![規則編輯器](assets/ruleeditor.jfif)

## 偵錯

如果您使用本文章提供的捆綁包，則可能需要啟用 [調試日誌](http://localhost:4502/system/console/slinglog) 為以下類：

+ `com.marketoandforms.core.impl.MarketoServiceImpl`
+ `com.marketoandforms.core.MarketoConfigurationService`
