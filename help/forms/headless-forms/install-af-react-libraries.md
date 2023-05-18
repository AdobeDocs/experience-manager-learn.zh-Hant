---
title: 安裝所需的最適化表單react程式庫
description: 將所需的相依性新增至您的react專案
feature: Adaptive Forms
version: 6.5
kt: 13285
topic: Development
role: User
level: Intermediate
source-git-commit: c6e83a627743c40355559d9cdbca2b70db7f23ed
workflow-type: tm+mt
source-wordcount: '173'
ht-degree: 1%

---


# 安裝所需的相依性

若要開始在react專案中使用無頭式最適化表單，請在react專案中安裝下列相依性

* @aemforms/af-react-components
* @aemforms/af-react-renderer

更新package.json以包含下列相依性。 撰寫時0.22.41為最新版本

```json
"@aemforms/af-react-components": "^0.22.41",
"@aemforms/af-react-renderer": "^0.22.41",
```

## 設定代理

跨原始資源共用(CORS)是一種安全機制，可限制網頁瀏覽器向應用程式托管網域以外的不同網域提出請求。 當您嘗試從托管於不同網域的API擷取資料時，可能會發生CORS錯誤。 透過設定代理，您可以略過CORS限制，並從React應用程式向API提出要求。 我已在src資料夾中名為setUpProxy.js的檔案中使用下列程式碼。 **請確定您變更目標以指向您的發佈例項。**

```
const { createProxyMiddleware } = require('http-proxy-middleware');
const proxy = {
    target: 'https://mypublishinstance:4503/',
    changeOrigin: true
}
module.exports = function(app) {
  app.use(
    '/adobe',
    createProxyMiddleware(proxy)
  ),
  app.use(
    '/content',
    createProxyMiddleware(proxy)
  );
};
```

您也需要安裝並新增 **http-proxy-middleware** 模組。

## 後續步驟

[擷取要內嵌的表單](./fetch-the-form.md)