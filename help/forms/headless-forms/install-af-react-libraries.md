---
title: 安裝必要的最適化表單react程式庫
description: 將所需的相依性新增至您的react專案
feature: Adaptive Forms
version: 6.5
kt: 13285
topic: Development
role: User
level: Intermediate
source-git-commit: 6aa3dff44a7e6f1f8ac896e30319958d84ecf57f
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 0%

---


# 安裝必要的相依性

若要開始在react專案中使用Headless調適型表單，請在react專案中安裝下列相依性

* @aemforms/af-react-components
* @aemforms/af-react-renderer

更新package.json以包含以下相依性。 在撰寫時，0.22.41是目前的版本

```json
"@aemforms/af-react-components": "^0.22.41",
"@aemforms/af-react-renderer": "^0.22.41",
```

>[!NOTE]
>
>在本教學課程中，下拉式清單和卡片版面配置是使用建立的 [材質UI程式庫](https://mui.com/). 您必須下載適當的資料UI套件，程式碼才能在您的系統上運作。

## 設定Proxy

跨原始資源共用(CORS)是一種安全性機制，可限制Web瀏覽器向應用程式託管所在的網域以外的不同網域提出請求。 當您嘗試從託管在不同網域上的API擷取資料時，可能會發生CORS錯誤。 透過設定Proxy，您可以略過CORS限制，並從React應用程式向API提出請求。 我在src資料夾中名為setUpProxy.js的檔案中使用了下列程式碼。 **請務必將目標變更為指向您的發佈執行個體。**

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

您還需要安裝並新增 **http-proxy-middleware** 模組。

## 後續步驟

[擷取表單以內嵌](./fetch-the-form.md)