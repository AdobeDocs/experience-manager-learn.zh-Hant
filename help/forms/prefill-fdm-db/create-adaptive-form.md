---
title: 建立最適化表單
description: 建立並設定最適化表單，以使用表單資料模型的預填服務
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5813
thumbnail: kt-5813.jpg
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '606'
ht-degree: 1%

---


# 建立最適化表單

目前，我們已建立下列

* 包含2個表的資料庫- `newhire`和`beneficiaries`
* 已設定Apache Sling Connection Pooled DataSource
* 基於RDBMS的表單資料模型

下一步是建立並配置最適化表單以使用表單資料模型。  若要搶先開始，您可以下載並匯入[範例表格。 ](assets/fdm-demo-af.zip)範例表單中有一個區段可顯示員工詳細資料，另一個區段則可列出員工的受益人。

## 將表單與表單資料模型建立關聯

本課程提供的範例表單與任何表單資料模型都沒有關聯。 若要設定表單以使用表單資料模型，我們必須執行下列動作：

* 選擇FDMDemo表單
* 按一下&#x200B;_屬性_->_表單模型_
* 從下拉式清單中選取「表單資料模型」
* 搜尋並選取在上一課中建立的表單資料模型。
* 按一下&#x200B;_保存並關閉_

## 設定預填服務

第一步是建立表單的預先填寫服務關聯。 若要建立預填服務的關聯，請遵循下列步驟

* 選擇`FDMDemo`表單
* 按一下&#x200B;_編輯_&#x200B;以在編輯模式下開啟表單
* 在內容階層中選取「表單容器」，然後按一下扳手圖示以開啟其屬性表單
* 從「預填充服務」下拉清單中選擇「表單資料模型預填充服務」__
* 按一下藍☑色以儲存變更

* ![預填充服務](assets/fdm-prefill.png)

## 配置員工詳細資訊

下一步是將「最適化表單」的文字欄位系結至「表單資料模型」元素。 您必須開啟以下欄位的屬性表，並設定其bindRef，如下所示


| 欄位名稱 | 綁定參照 |
|------------|--------------------|
| 名字 | /newhire/FirstName |
| 姓氏 | /newhire/lastName |

>[!NOTE]
>
>您可以自由新增其他文字欄位，並將它們系結至適當的表單資料模型元素

## 配置受益人表

下一步是以表格形式顯示員工的受益人。 提供的樣例表格包含4列和單行。 我們需要根據受益者人數來設定表格。

* 在編輯模式中開啟表格。
* 展開根面板->您的受益人->表
* 選擇「行1」(Row1)，然後按一下扳手錶徵圖以開啟其屬性表。
* 將「綁定參考」設定為&#x200B;**/newhire/GetEmployeeWennerities**
* 將「重複設定——最小計數」設為1,「最大計數」設為5。
* 您的Row1設定應如下方的螢幕擷取畫面
   ![row-configure](assets/configure-row.PNG)
* 按一下藍色☑以儲存變更

## 系結列儲存格

最後，我們需要將行單元格綁定到形成資料模型元素。

* 展開根面板->您的受益人->表->行1
* 按照下表設定每個行單元格的綁定引用

| 列儲存格 | 繫結參考 |
|------------|----------------------------------------------|
| 名字 | /newhire/GetEmployeeWennerities/firstname |
| 姓氏 | /newhire/GetEmployeeWenserients/lastname |
| 關係 | /newhire/GetEmployeeWennerities/relation |
| 百分比 | /newhire/GetEmployeeWennerities/perception |

* 按一下藍色☑以儲存變更

## 測試您的表格

我們現在需要在URL中開啟具有適當empID的表格。 下列2個連結將填入表格中，並填入資料庫的資訊
[Form With empID=207](http://localhost:4502/content/dam/formsanddocuments/fdmdemo/jcr:content?wcmmode=disabled&amp;empID=207)
[具有empID=208](http://localhost:4502/content/dam/formsanddocuments/fdmdemo/jcr:content?wcmmode=disabled&amp;empID=208)的表單

## 疑難排解

我的表格空白，沒有任何資料

* 請確定表單資料模型傳回的結果正確。
* 表單與正確的表單資料模型相關聯
* 檢查欄位系結
* 檢查stdout日誌檔案。 您應該會看到empID正寫入檔案。如果您看不到此值，表單可能不會使用所提供的自訂範本。

未填入表格

* 檢查Row1綁定
* 請確定已正確設定列1的重複設定（最小值=1，最大值= 5或更多）

