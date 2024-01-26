---
title: 安裝所需的自適應表單react程式庫
description: 將所需的相依性新增到您的react專案
feature: Adaptive Forms
version: 6.5
jira: KT-13285
topic: Development
role: User
level: Intermediate
exl-id: 0ed44016-d52a-4980-a0b1-06da149c3cb1
duration: 56
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 0%

---

# 安裝必要的相依性

若要開始在react專案中使用Headless調適型表單，請在react專案中安裝以下相依性

* @aemforms/af-react-components
* @aemforms/af-react-renderer

更新package.json以包含以下相依性。 在寫入時，0.22.41是目前的版本

```json
"@aemforms/af-react-components": "^0.22.41",
"@aemforms/af-react-renderer": "^0.22.41",
```

>[!NOTE]
>
>本教學課程中的下拉式清單及卡片版面配置是使用建立的 [材質UI程式庫](https://mui.com/). 您需要下載適當的素材UI套件，讓程式碼在您的系統上正常運作。

## 設定Proxy

跨原始資源共用(CORS)是一種安全性機制，可限制網頁瀏覽器向應用程式託管所在的網域以外的不同網域提出請求。 當您嘗試從託管在不同網域上的API擷取資料時，可能會發生CORS錯誤。 透過設定Proxy，您可以略過CORS限制，並從React應用程式向API提出請求。 我在src資料夾中名為setUpProxy.js的檔案中使用了下列程式碼。 **請確定您將目標變更為指向您的發佈執行個體。**

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
