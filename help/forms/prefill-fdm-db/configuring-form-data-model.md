---
title: 配置表單資料模型
description: 基於RDBMS資料源建立表單資料模型
feature: Adaptive Forms
version: 6.4,6.5
kt: 5812
thumbnail: kt-5812.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 5fa4638f-9faa-40e0-a20d-fdde3dbb528a
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: tm+mt
source-wordcount: '498'
ht-degree: 0%

---

# 配置表單資料模型

## Apache Sling連接池化資料源

建立RDBMS支援的表單資料模型的第一步是配置Apache Sling連接池化資料源。 要配置資料源，請執行下列步驟：

* 將瀏覽器指向 [configMgr](http://localhost:4502/system/console/configMgr)
* 搜索 **Apache Sling連接池化資料源**
* 添加新條目並提供螢幕快照中所示的值。
* ![資料源](assets/data-source.png)
* 保存更改

>[!NOTE]
>JDBC連接URI、用戶名和密碼將根據MySQL資料庫配置而更改。


## 建立表單資料模型

* 將瀏覽器指向 [資料整合](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm)
* 按一下 _建立_->_窗體資料模型_
* 提供有意義的名稱和標題以形成資料模型，例如 **員工**
* 按一下 _下一個_
* 選擇在前一節（論壇）中建立的資料源
* 按一下 _建立_->編輯以在編輯模式下開啟新建立的表單資料模型
* 展開 _論壇_ 節點，以查看employee方案。 展開employee節點以查看2個表

## 將圖元添加到模型

* 確保已展開員工節點
* 選擇新實體和受益實體，然後按一下 _添加選定項_

## 將讀取服務添加到新實體

* 選擇新實體
* 按一下 _編輯屬性_
* 從「讀取服務」下拉清單中選擇「獲取」
* 按一下+表徵圖將參數添加到獲取服務
* 指定螢幕截圖中顯示的值
* ![獲取服務](assets/get-service.png)
>[!NOTE]
> get服務需要映射到新實體的empID列的值。傳遞此值的方法有多種，在本教程中，empID通過名為empID的請求參數。
* 按一下 _完成_ 保存get服務的參數
* 按一下 _完成_ 保存對表單資料模型的更改

## 添加2個實體之間的關聯

不會在表單資料模型中自動建立在資料庫實體之間定義的關聯。 實體之間的關聯需要使用表單資料模型編輯器來定義。 每個新實體都可以有一個或多個受益人，我們需要定義新實體和受益實體之間的一對多關聯。
以下步驟將引導您完成建立一對多關聯的過程

* 選擇新實體並按一下 _添加關聯_
* 為關聯和其他屬性提供有意義的標題和標識符，如下面的螢幕快照所示
   ![關聯](assets/association-entities-1.png)

* 按一下 _編輯_ 表徵圖

* 指定此螢幕抓圖中顯示的值
* ![關聯2](assets/association-entities.png)
* **我們使用受益人和新實體的empID列連結兩個實體。**
* 按一下 _完成_ 保存更改

## Test窗體資料模型

我們的表單資料模型 **_得_** 接受empID並返回nwhire及其受益人的詳細資訊的服務。 要test獲取服務，請按照下面列出的步驟操作。

* 選擇新實體
* 按一下 _Test模型對象_
* 提供有效的empID並按一下 _Test_
* 您應該得到如下螢幕抓圖所示的結果
* ![test-fdm](assets/test-form-data-model.png)

## 後續步驟

[從URL獲取empID](./get-request-parameter.md)