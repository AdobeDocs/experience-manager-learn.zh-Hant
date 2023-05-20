---
title: 建立自適應窗體
description: 建立並配置自適應表單以使用表單資料模型的預填充服務
feature: Adaptive Forms
version: 6.4,6.5
kt: 5813
thumbnail: kt-5813.jpg
topic: Development
role: User
level: Beginner
exl-id: c8d4eed8-9e2b-458c-90d8-832fc9e0ad3f
source-git-commit: 4b47daf82e27f6bea4be30e3cdd132f497f4c609
workflow-type: tm+mt
source-wordcount: '606'
ht-degree: 1%

---

# 建立自適應窗體

到目前為止，我們創造了以下

* 包含2個表的資料庫 —  `newhire` 和 `beneficiaries`
* 已配置Apache Sling連接池化資料源
* 基於RDBMS的表單資料模型

下一步是建立並配置一個自適應表單以使用表單資料模型。  為了搶先，你可以 [下載和導入](assets/fdm-demo-af.zip) 樣式。 示例表單中有一個部分用於顯示員工詳細資訊，另一個部分用於列出員工的受益人。

## 將表單與表單資料模型關聯

本課程提供的示例表單與任何表單資料模型都不關聯。 要將表單配置為使用表單資料模型，我們需要執行以下操作：

* 選擇FDMDemo窗體
* 按一下 _屬性_->_窗體模型_
* 從下拉清單中選擇「表單資料模型」
* 搜索並選擇在上一課中建立的表單資料模型。
* 按一下 _保存並關閉_

## 配置預填充服務

第一步是關聯表單的預填服務。 要關聯預填服務，請執行以下步驟

* 選擇 `FDMDemo` 表格
* 按一下 _編輯_ 在編輯模式下開啟窗體
* 在內容層次結構中選擇「表單容器」，然後按一下扳手錶徵圖以開啟其屬性工作表
* 選擇 _表單資料模型預填充服務_ 從「預填充服務」下拉清單
* 按一下藍色☑保存您所做的更改

* ![預填充服務](assets/fdm-prefill.png)

## 配置員工詳細資訊

下一步是將「自適應表單」的文本欄位綁定到「表單資料模型」元素。 您必須開啟以下欄位的屬性表並設定其bindRef，如下所示


| 欄位名稱 | 綁定引用 |
|------------|--------------------|
| 名字 | /newhire/FirstName |
| 姓氏 | /newhire/lastName |

>[!NOTE]
>
>可以隨意添加其他文本欄位並將其綁定到適當的表單資料模型元素

## 配置受益人表

下一步是以表格形式顯示員工的受益人。 提供的示例表單具有4列和單行的表。 我們需要根據受益人的數量來配置表以增長。

* 在編輯模式下開啟窗體。
* 展開根面板 — >您的受益人 — >表
* 選擇「行1」(Row1)，然後按一下扳手錶徵圖以開啟其屬性表。
* 將綁定引用設定為 **/newhire/GetEmployee受益人**
* 將「Repeat Settings - Minimum Count（重複設定 — 最小計數）」設定為1，將「Maximum Count（最大計數）」設定為5。
* 您的Row1配置應像下面的螢幕抓圖
   ![行配置](assets/configure-row.PNG)
* 按一下藍色☑保存更改

## 綁定行單元格

最後，我們需要將行單元格綁定到資料模型元素。

* 展開根面板 — >您的受益人 — >表 — >行1
* 按下表設定每個行單元格的綁定引用

| 行單元格 | 繫結參考 |
|------------|----------------------------------------------|
| 名字 | /newhire/GetEmployeeWneliters/firstname |
| 姓氏 | /newhire/GetEmployeeWneliters/lastname |
| 關係 | /newhire/GetEmployeeWensiers/關係 |
| 百分比 | /newhire/GetEmployee受益人/百分比 |

* 按一下藍色☑保存更改

## Test窗體

現在，我們需要在url中開啟具有相應empID的窗體。 以下2個連結將用資料庫中的資訊填充表單
[empID=207的窗體](http://localhost:4502/content/dam/formsanddocuments/fdmdemo/jcr:content?wcmmode=disabled&amp;empID=207)
[empID=208的窗體](http://localhost:4502/content/dam/formsanddocuments/fdmdemo/jcr:content?wcmmode=disabled&amp;empID=208)

## 疑難排解

我的表單為空，沒有任何資料

* 確保表單資料模型返回正確的結果。
* 表單與正確的表單資料模型關聯
* 檢查欄位綁定
* 檢查stdout日誌檔案。 您應看到empID正在寫入檔案。如果您未看到此值，則表單可能未使用提供的自定義模板。

未填充表

* 檢查Row1綁定
* 確保正確設定行1的重複設定（最小值=1，最大值= 5或更多）
