---
title: 將AEM Forms與Acrobat Sign
description: Acrobat Sign和AEM Forms允許自動化複雜的交易，並將合法電子簽名作為無縫數字型驗的一部分。
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 0be61f04-c3ed-4ecb-bab7-73fd308c14e0
last-substantial-update: 2020-03-20T00:00:00Z
source-git-commit: 81b96f59450448a3d5b17a61aa025acd60d0cce1
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 11%

---

# 將AEM Forms與Acrobat Sign

Acrobat Sign支援自適應表單的電子簽名工作流。 電子簽名有助於改善處理法律、銷售、薪資、人力資源管理及許多領域文件的工作流程。AEM Forms和Acrobat Sign的整合將允許您執行以下操作

* 使用自適應Forms捕獲資料並為簽名顯示自動生成的記錄文檔(DoR)
* 根據您的PDF模板建立自適應Forms。 將資料與pdf模板合併，並為簽名顯示該資料
* 使用「簽名文檔」工作流元件發送文檔以供簽名

## 必備條件

您需要以下條件將Acrobat Sign與AEM Forms整合：

* 啟用SSL的AEM Forms伺服器
* 一個活動的Acrobat Sign開發者帳戶。
* Acrobat SignAPI應用程式
* Acrobat SignAPI應用程式的憑據（客戶端ID和客戶端密碼）。
