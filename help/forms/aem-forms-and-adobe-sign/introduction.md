---
title: 搭配使用AEM Forms和Adobe Sign
description: Adobe Sign和AEM Forms可讓複雜交易自動化，並納入合法電子簽名，以提供順暢的數位體驗。
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: c60a46027cc8d71fddd41aa31dbb569e4df94823
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 0%

---

# 搭配使用AEM Forms和Adobe Sign

Adobe Sign可針對最適化表單啟用電子簽名工作流程。 電子簽名可改善處理法律、銷售、薪資、人力資源管理等領域檔案的工作流程。
AEM Forms與Adobe Sign之間的整合可讓您執行下列作業

* 使用Adaptive Forms來擷取資料並呈現自動產生的記錄檔案(DoR)以供簽名
* 根據您的PDF範本建立最適化表單。 將資料與pdf範本合併，並為簽名呈現相同的內容
* 使用「簽署檔案」工作流程元件傳送檔案以進行簽署

## 必備條件

您需要下列項目才能將Adobe Sign與AEM Forms整合：

* 啟用SSL的AEM Forms伺服器
* 有效的Adobe Sign開發人員帳戶。
* Adobe Sign API應用程式
* Adobe Sign API應用程式的認證（用戶端ID和用戶端密碼）。

