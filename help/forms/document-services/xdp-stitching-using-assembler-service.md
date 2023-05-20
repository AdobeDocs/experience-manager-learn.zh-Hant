---
title: 使用匯編器服務進行XDP拼接
description: 利用AEM Forms匯編服務縫製xdp
feature: Assembler
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2022-12-19T00:00:00Z
exl-id: e116038f-7d86-41ee-b1b0-7b8569121d6d
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '357'
ht-degree: 1%

---

# 使用匯編器服務進行XDP拼接

本文為您提供了一些資源，用於演示使用匯編器服務編輯xdp文檔的能力。
已編寫以下jsp代碼以插入名為 **地址** 從名為address.xdp的xdp文檔到名為 **地址** 中。 生成的xdp已保存在安裝的根資料夾中AEM。

匯編程式服務依賴有效的DDX文檔來描述PDF文檔的操作。 您可以參考 [此處為DDX參考文檔](assets/ddxRef.pdf).第40頁包含有關xdp拼接的資訊。

```java
    javax.servlet.http.Part ddxFile = request.getPart("xdpstitching.ddx");
    System.out.println("Got DDX");
    java.io.InputStream ddxIS = ddxFile.getInputStream();
    com.adobe.aemfd.docmanager.Document ddxDocument = new com.adobe.aemfd.docmanager.Document(ddxIS);
    javax.servlet.http.Part masterXdpPart = request.getPart("masterxdp.xdp");
    System.out.println("Got master xdp");
    java.io.InputStream masterXdpPartIS = masterXdpPart.getInputStream();
    com.adobe.aemfd.docmanager.Document masterXdpDocument = new com.adobe.aemfd.docmanager.Document(masterXdpPartIS);

    javax.servlet.http.Part fragmentXDPPart = request.getPart("fragment.xdp");
    System.out.println("Got fragment.xdp");
    java.io.InputStream fragmentXDPPartIS = fragmentXDPPart.getInputStream();
    com.adobe.aemfd.docmanager.Document fragmentXdpDocument = new com.adobe.aemfd.docmanager.Document(fragmentXDPPartIS);

    java.util.Map < String, Object > mapOfDocuments = new java.util.HashMap < String, Object > ();
    mapOfDocuments.put("master.xdp", masterXdpDocument);
    mapOfDocuments.put("address.xdp", fragmentXdpDocument);
    com.adobe.fd.assembler.service.AssemblerService assemblerService = sling.getService(com.adobe.fd.assembler.service.AssemblerService.class);
    if (assemblerService != null)
      System.out.println("Got assembler service");

    com.adobe.fd.assembler.client.AssemblerOptionSpec aoSpec = new com.adobe.fd.assembler.client.AssemblerOptionSpec();
    aoSpec.setFailOnError(true);

    com.adobe.fd.assembler.client.AssemblerResult assemblerResult = assemblerService.invoke(ddxDocument, mapOfDocuments, aoSpec);
    com.adobe.aemfd.docmanager.Document finalXDP = assemblerResult.getDocuments().get("stitched.xdp");
    finalXDP.copyToFile(new java.io.File("stitched.xdp"));
```

下面列出了將片段插入到另一個xdp的DDX檔案。 DDX插入子窗體  **地址** 從address.xdp到名為 **地址** 在master.xdp中。 名為的結果文檔 **縫合的.xdp** 保存到檔案系統。

```xml
<?xml version="1.0" encoding="UTF-8"?> 
<DDX xmlns="http://ns.adobe.com/DDX/1.0/"> 
        <XDP result="stitched.xdp"> 
           <XDP source="master.xdp"> 
            <XDPContent insertionPoint="address" source="address.xdp" fragment="address"/> 
         </XDP> 
        </XDP>         
</DDX>
```

使此功能在伺服器上AEM工作

* 下載 [XDP縫合包](assets/xdp-stitching.zip) 到本地系統。
* 使用 [軟體包管理器](http://localhost:4502/crx/packmgr/index.jsp)
* [解壓此zip檔案的內容](assets/xdp-and-ddx.zip) 獲取示例xdp和DDX檔案

**安裝軟體包後，必須允許在AdobeGranite CSRF過濾器中列出以下URL。**

1. 請按照下面所述的步驟列出上述路徑。
1. [登錄到configMgr](http://localhost:4502/system/console/configMgr)
1. Adobe花崗岩CSRF濾波器的研究
1. 在排除的節中添加以下路徑並保存 `/content/AemFormsSamples/assemblerservice`
1. 搜索「Sling引用者篩選器」
1. 選中「允許空」複選框。 （此設定應僅用於測試目的）有多種test示例代碼的方法。 最快捷最簡單的方法是使用Postman應用。 Postman允許您向伺服器發出POST請求。 在系統上安裝Postman應用。
啟動應用並輸入以下URL以test導出資料API http://localhost:4502/content/AemFormsSamples/assemblerservice.html

提供螢幕抓圖中指定的以下輸入參數。 你可以使用之前下載的示例文檔，
![西迪普 — 斯蒂奇 — 郵遞員](assets/xdp-stitching-postman.png)

>[!NOTE]
>
>確保AEM Forms安裝完成。 您的所有捆綁包都需要處於活動狀態。
