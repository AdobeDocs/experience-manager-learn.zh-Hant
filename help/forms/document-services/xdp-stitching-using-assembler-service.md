---
title: 使用組合器服務進行XDP拼接
description: 使用AEM Forms中的組合器服務拼接xdp
feature: Assembler
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2022-12-19T00:00:00Z
source-git-commit: 8f17e98c56c78824e8850402e3b79b3d47901c0b
workflow-type: tm+mt
source-wordcount: '357'
ht-degree: 1%

---

# 使用組合器服務進行XDP拼接

本文提供相關資產，以展示使用組合器服務拼接xdp檔案的能力。
以下jsp代碼被寫入以插入名為 **地址** 從名為address的xdp文檔到名為的插入點 **地址** 在master.xdp文檔中。 產生的xdp會儲存在AEM安裝的根資料夾中。

組合器服務依賴有效的DDX文檔來描述PDF文檔的操作。 您可以參閱 [此處提供DDX參考文檔](assets/ddxRef.pdf).第40頁包含xdp匯整的相關資訊。

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

下方列出可將片段插入其他xdp的DDX檔案。 DDX插入子表單  **地址** 從address.xdp進入名為 **地址** 在master.xdp中。 生成的文檔名為 **sitted.xdp** 會儲存至檔案系統。

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

* 下載 [XDP匯整套件](assets/xdp-stitching.zip) 到本地系統。
* 使用上傳並安裝套件 [封裝管理員](http://localhost:4502/crx/packmgr/index.jsp)
* [解壓縮此zip檔案的內容](assets/xdp-and-ddx.zip) 獲取示例xdp和DDX檔案

**安裝套件後，您必須在AdobeGranite CSRF篩選器中允許列出下列URL。**

1. 請依照下列步驟允許列出上述路徑。
1. [登入configMgr](http://localhost:4502/system/console/configMgr)
1. 搜尋AdobeGranite CSRF篩選器
1. 在排除的區段中新增下列路徑並儲存 `/content/AemFormsSamples/assemblerservice`
1. 搜尋「Sling反向連結篩選器」
1. 勾選「允許空白」核取方塊。 （此設定應僅用於測試用途）有許多方法可測試范常式式碼。 最快、最簡單的方式是使用Postman應用程式。 Postman可讓您向伺服器提出POST請求。 在您的系統上安裝Postman應用程式。
啟動應用程式並輸入下列URL以測試匯出資料API http://localhost:4502/content/AemFormsSamples/assemblerservice.html

提供下列輸入參數，如螢幕擷取畫面中所指定。 您可以使用先前下載的示例文檔，
![xdp-stitch-postman](assets/xdp-stitching-postman.png)

>[!NOTE]
>
>確認AEM Forms安裝完成。 您的所有套件組合都必須處於作用中狀態。
