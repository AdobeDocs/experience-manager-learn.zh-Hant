---
title: 與AEM Forms和Acrobat Sign應用
description: Acrobat Sign和AEM Forms允許自動化複雜的交易，並將合法電子簽名作為無縫數字型驗的一部分。
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
kt: 13099
last-substantial-update: 2023-04-13T00:00:00Z
exl-id: 2ff7be5b-884c-420d-9a06-f0e2a99d3ef3
source-git-commit: 4709035983a5c6705c4e807d877ee71145f48987
workflow-type: tm+mt
source-wordcount: '174'
ht-degree: 1%

---

# AEM Forms與Acrobat Sign網表


本教程將指導您完成使用從 [反應](https://react.dev/) 應用程式，並呈現生成的文檔，以便使用Acrobat Sign網表進行簽名。

以下是用例流

* 用戶在React應用中填寫表單。
* 表格資料被提交到AEM Forms端點以生成互動式通信文檔。
* 使用生成的文檔建立Acrobat Sign小部件url。
* 為用戶簽名文檔的調用應用程式提供構件url。

## 必備條件

要使用案例正常工作，您將需要以下條件：

* 帶有AEMForms附加包的伺服器
* 安 [用於Acrobat Sign應用程式的整合密鑰](https://helpx.adobe.com/sign/kb/how-to-create-an-integration-key.html)

## 後續步驟

編寫 [自定義OSGi服務以生成交互通信文檔](./create-ic-document.md) 使用文檔化的API
