---
title: AEM Forms與馬克托（四）
seo-title: AEM Forms與馬克托（四）
description: 教學課程，將AEM Forms與Marketo整合，使用AEM Forms表單資料模型。
seo-description: 教學課程，將AEM Forms與Marketo整合，使用AEM Forms表單資料模型。
feature: 「適應性Forms，表單資料模型」
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
topic: 開發
role: 開發人員
level: 經驗豐富
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '316'
ht-degree: 0%

---


# 使用表單資料模型建立最適化表單

下一步是建立最適化表單，並基於在前一步驟中建立的表單資料模型。
使用者將輸入銷售線索ID，並在Marketo服務上跳出時，會呼叫依ID取得銷售線索。 然後將服務操作的結果映射到自適應Forms的適當欄位。

1. 建立最適化表單並以「空白表單範本」為基礎，將其與先前步驟中建立的表單資料模型建立關聯。
1. 在編輯模式中開啟表格
1. 將TextField元件和面板元件拖放到最適化表單上。 將TextField元件的標題設定為「輸入銷售線索ID」，並將其名稱設定為「銷售線索ID」
1. 將2個TextField元件拖放至面板元件
1. 將2個文本欄位元件的名稱和標題設定為FirstName和LastName
1. 將「面板」元件設定為可重複元件，方法是將「最小值」設定為1,「最大值」設定為-1。 當Marketo服務傳回Lead Objects陣列，且您需要具備可重複的元件來顯示結果時，這是必要項。 但是，在這種情況下，我們將只返回一個Lead對象，因為我們正在按其ID搜索Lead對象。
1. 在LeadId欄位上建立規則，如下圖所示
1. 預覽表單，並在「銷售線索ID」欄位中輸入有效的銷售線索ID，然後將其選出。 「名字」和「姓氏」欄位應填入服務呼叫的結果。

下列螢幕擷取說明規則編輯器設定

![規則編輯器](assets/ruleeditor.jfif)
