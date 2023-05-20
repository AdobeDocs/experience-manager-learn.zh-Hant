---
title: 安裝所需的自適應表單反應庫
description: 將所需的依賴項添加到您的反應項目
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


# 安裝所需的依賴項

要開始在反應項目中使用無頭自適應表單，請在反應項目中安裝以下依賴項

* @aemforms/af反應元件
* @aemforms/af react-renderer

更新package.json以包含以下依賴關係。 寫0.22.41時是當前版本

```json
"@aemforms/af-react-components": "^0.22.41",
"@aemforms/af-react-renderer": "^0.22.41",
```

## 設定代理

跨源資源共用(CORS)是一種安全機制，它限制Web瀏覽器向應用程式所托管的域以外的域發出請求。 當您嘗試從承載在不同域上的API中獲取資料時，可能會出現CORS錯誤。 通過設定代理，您可以繞過CORS限制，從React應用程式向API發出請求。 我在src資料夾中名為setUpProxy.js的檔案中使用了以下代碼。 **確保更改目標以指向發佈實例。**

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

您還需要安裝和添加 **http代理中間件** 模組。

## 後續步驟

[提取要嵌入的表單](./fetch-the-form.md)