---
title: 將AEM Forms與Adobe Sign搭配使用
description: Adobe Sign和AEM Forms可讓複雜的交易自動化，並納入合法的電子簽名，以提供順暢的數位體驗。
feature: 適用性Forms,Adobe Sign
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
topic: 開發
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '163'
ht-degree: 1%

---

# 將AEM Forms與Adobe Sign搭配使用

Adobe Sign可啟用最適化表單的電子簽名工作流程。 電子簽名改進了處理法律、銷售、工資、人力資源管理等許多領域的文檔的工作流。
AEM Forms與Adobe Sign的整合可讓您執行下列作業

* 使用適用性Forms來擷取資料並呈現自動產生的記錄檔案(DoR)以供簽署
* 根據PDF範本建立最適化Forms。 將資料與PDF範本合併，並提供給簽名
* 使用「簽署檔案」工作流程元件傳送要簽署的檔案

## 必備條件

若要整合Adobe Sign與AEM Forms，您需要下列項目：

* 啟用SSL的AEM Forms伺服器
* 有效的Adobe Sign開發人員帳戶。
* Adobe Sign API應用程式
* Adobe Sign API應用程式的憑證（用戶端ID和用戶端密碼）。

