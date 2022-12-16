---
title: 測試歡迎套件解決方案
description: 部署解決方案資產以測試解決方案
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-12-14T00:00:00Z
source-git-commit: 0e27907066c7d688549a980ccd17b3f17d74b60b
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 0%

---

# 部署並測試範例資產

[安裝歡迎套件包](assets/welcomekit.zip). 此套件包含頁面範本、自訂元件以列出資產、範例工作流程、電子郵件範本，以及要包含在歡迎套件中的範例pdf檔案。
[安裝welcome-kit套件組合](assets/welcomekit.core-1.0.0-SNAPSHOT.jar). 此套件包含用來建立頁面的程式碼，以及傳回要顯示在網頁中的資產的java類別。
[安裝範例最適化表單](assets/account-openeing-form.zip)
設定Day CQ Mail Service。 工作流程會將歡迎套件連結傳送至需要正確設定SMTP的表單提交者。
根據您的需求設定工作流程的傳送電子郵件元件
[預覽表單](http://localhost:4502/content/dam/formsanddocuments/co-operators/accountopeningform/jcr:content?wcmmode=disabled)
輸入詳細資訊，然後選擇一個或多個共同基金並提交表格。您應該收到一封包含歡迎套件連結的電子郵件


