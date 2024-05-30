---
title: 在最適化表單中顯示QR碼
description: 在最適化表單中顯示QR碼
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-15603
last-substantial-update: 2024-05-28T00:00:00Z
source-git-commit: e20d9f80cc7e1c6f5f6c81233d9a5178551e2fa2
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 0%

---

# QR程式碼元件範例

在最適化表單中內嵌QR程式碼可大幅提升使用者存取表單相關額外資訊的便利性和效率。

範例元件使用 [Qrcode.js](https://davidshimjs.github.io/qrcodejs/).

QRCode.js是用於建立QRCode的Javascript程式庫，它支援具有HTML5畫布的跨瀏覽器以及DOM中的表格標籤。

元件會根據元件的configuration屬性中指定的值來產生QR碼。
![影像](assets/qr-code-url.png)

下列程式碼用於qr程式碼產生器元件的body.jsp。

「url」是需要嵌入到二維碼中的url。 此URL是在QR程式碼元件的設定屬性中指定。

```java
<%@include file="/libs/foundation/global.jsp"%>
<body>
    <h2>Scan the QR Code for more information related to this form</h2>
    <div data-url="<%=properties.get("url")%>">
    </div>
    <div id="qrcode">
    </div>
</body>
```



下列程式碼會使用qr-code-generator元件之使用者端程式庫中QRCode.js程式庫的makeCode方法。產生的QR程式碼會附加至id所識別的div **&quot;qrcode&quot;**.

```javascript
$(document).ready(function()
  {
      var qrcode = new QRCode("qrcode");
      qrcode.makeCode(document.querySelector("[data-url]").getAttribute("data-url"));
      
 });
```

## 在您的本機伺服器上部署資產

* [使用「封裝管理員」下載並安裝QR程式碼元件。](assets/qrcode.zip)
* [使用「封裝管理員」下載並安裝最適化表單範例。](assets/form-with-qr-code.zip)
* [預覽表單](http://localhost:4502/content/dam/formsanddocuments/qrcode/w9form/jcr:content?wcmmode=disabled). 表格的說明區段含有QR碼。


