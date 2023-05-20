---
title: 測試歡迎套件解決方案
description: 部署解決方案資產以test解決方案
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-12-14T00:00:00Z
exl-id: 07a1a9fc-7224-4e2d-8b6d-d935b1125653
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 0%

---

# 部署和test示例資產

[安裝歡迎套件包](assets/welcomekit.zip)。 此軟體包包含頁面模板、自定義元件以列出資產、示例工作流、電子郵件模板和要包含在歡迎工具包中的示例pdf文檔。
[安裝歡迎套件包](assets/welcomekit.core-1.0.0-SNAPSHOT.jar)。 此捆綁包包含用於建立頁面的代碼和用於返回要在網頁中顯示的資產的java類。
[安裝示例自適應窗體](assets/account-openeing-form.zip)
配置第CQ天郵件服務。 工作組將歡迎套件連結發送到需要正確配置SMTP的表單提交器。
根據您的要求配置工作流的「發送電子郵件」元件
[預覽窗體](http://localhost:4502/content/dam/formsanddocuments/co-operators/accountopeningform/jcr:content?wcmmode=disabled)
輸入詳細資訊並選擇一個或多個共同基金並提交表單您應該收到一封電子郵件，其中包含指向歡迎套件的連結
