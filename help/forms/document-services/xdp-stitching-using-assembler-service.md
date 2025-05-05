---
title: 使用組合器服務的XDP拼接
description: 在AEM Forms中使用組合器服務來拼接xdp
feature: Assembler
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2022-12-19T00:00:00Z
exl-id: e116038f-7d86-41ee-b1b0-7b8569121d6d
duration: 91
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '346'
ht-degree: 0%

---

# 使用組合器服務的XDP拼接

本文提供相關資產，用於示範使用組合器服務彙整xdp檔案的能力。
下列jsp程式碼是用來在master.xdp檔案中插入名為&#x200B;**address**&#x200B;的xdp檔案中名為&#x200B;**address**&#x200B;的子表單。 產生的xdp會儲存在AEM安裝的根資料夾中。

組合器服務需仰賴有效的DDX檔案來說明對PDF檔案的操作。 您可以在此參閱[DDX參考檔案](assets/ddxRef.pdf)。第40頁包含有關xdp彙整的資訊。

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

下面列出要將片段插入另一個xdp的DDX檔案。 DDX從address.xdp將子表單&#x200B;**位址**&#x200B;插入到master.xdp中稱為&#x200B;**位址**&#x200B;的插入點。 名稱為&#x200B;**stitched.xdp**&#x200B;的結果檔案已儲存至檔案系統。

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

若要讓此功能在您的AEM伺服器上運作

* 將[XDP拼接封裝](assets/xdp-stitching.zip)下載到您的本機系統。
* 使用[封裝管理員](http://localhost:4502/crx/packmgr/index.jsp)上傳及安裝封裝
* [擷取此zip檔案的內容](assets/xdp-and-ddx.zip)以取得範例xdp和DDX檔案

**安裝套件後，您必須在Adobe Granite CSRF篩選器中允許列出下列URL。**

1. 請依照下列步驟操作，將上述路徑加入允許清單。
1. [登入configMgr](http://localhost:4502/system/console/configMgr)
1. 搜尋Adobe Granite CSRF篩選器
1. 在排除的區段中新增以下路徑並儲存`/content/AemFormsSamples/assemblerservice`
1. 搜尋「Sling查閱者篩選器」
1. 勾選「允許空白」核取方塊。 （此設定僅供測試之用）
測試範常式式碼的方法有很多種。 最快捷、最輕鬆的方式就是使用Postman應用程式。 Postman可讓您向伺服器發出POST要求。 在您的系統上安裝Postman app 。
啟動應用程式並輸入以下URL以測試匯出資料API
http://localhost:4502/content/AemFormsSamples/assemblerservice.html

提供下列在熒幕擷取畫面中指定的輸入引數。 您可以使用先前下載的範例檔案，
![xdp-stitch-postman](assets/xdp-stitching-postman.png)

>[!NOTE]
>
>請確定您的AEM Forms安裝已完成。 您的所有套件組合都必須處於作用中狀態。
>
