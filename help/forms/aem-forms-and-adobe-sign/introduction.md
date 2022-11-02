---
title: 將AEM Forms與Acrobat Sign搭配使用
description: Acrobat Sign和AEM Forms可讓複雜的交易自動化，並納入合法的電子簽名，以提供順暢的數位體驗。
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

# 將AEM Forms與Acrobat Sign搭配使用

Acrobat Sign可啟用最適化表單的電子簽名工作流程。 電子簽名有助於改善處理法律、銷售、薪資、人力資源管理及許多領域文件的工作流程。AEM Forms與Acrobat Sign的整合可讓您執行下列作業

* 使用適用性Forms來擷取資料並呈現自動產生的記錄檔案(DoR)以供簽署
* 根據您的PDF範本建立適用性Forms。 將資料與PDF範本合併，並提供給簽名
* 使用「簽署檔案」工作流程元件傳送要簽署的檔案

## 必備條件

若要整合Acrobat Sign與AEM Forms，您需要下列項目：

* 啟用SSL的AEM Forms伺服器
* 有效的Acrobat Sign開發人員帳戶。
* Acrobat Sign API應用程式
* Acrobat Sign API應用程式的憑證（用戶端ID和用戶端密碼）。
