---
title: 從HTM5表單提交生成PDF
description: 從行動表單提交產生PDF
feature: Mobile Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 91b4a134-44a7-474e-b769-fe45562105b2
last-substantial-update: 2020-01-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '544'
ht-degree: 0%

---

# 從HTM5表單提交生成PDF {#generate-pdf-from-htm-form-submission}

本文將逐步引導您完成從HTML5(亦稱為Mobile Forms)表單提交產生pdf時所涉及的步驟。 本示範也將說明將影像新增至HTML5表單，並將影像合併為最終pdf所需的步驟。


若要將提交的資料合併至xdp範本，我們會執行下列作業

編寫Servlet以處理HTML5表單提交

* 此Servlet內可取得已提交資料的保留
* 將此資料與xdp範本合併，以產生pdf
* 將pdf串流回呼叫應用程式

以下是從請求中擷取已提交資料的Servlet程式碼。 接著，會呼叫自訂documentServices .mobileFormToPDF方法以取得PDF。

```java
protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
  StringBuffer stringBuffer = new StringBuffer();
  String line = null;
  try {
   InputStreamReader isReader = new InputStreamReader(request.getInputStream(), "UTF-8");
   BufferedReader reader = new BufferedReader(isReader);
   while ((line = reader.readLine()) != null)
    stringBuffer.append(line);
  } catch (Exception e) {
   System.out.println("Error");
  }
  String xmlData = new String(stringBuffer);
  Document generatedPDF = documentServices.mobileFormToPDF(xmlData);
  try {
   
   InputStream fileInputStream = generatedPDF.getInputStream();
   response.setContentType("application/pdf");
   response.addHeader("Content-Disposition", "attachment; filename=AemFormsRocks.pdf");
   response.setContentLength((int) fileInputStream.available());
   OutputStream responseOutputStream = response.getOutputStream();
   int bytes;
   while ((bytes = fileInputStream.read()) != -1) {
    responseOutputStream.write(bytes);
   }
   responseOutputStream.flush();
   responseOutputStream.close();

  } catch (IOException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }

 }
```

若要新增影像至行動表單，並以我們已使用下列項目的pdf顯示該影像

XDP範本 — 在xdp範本中，我們新增了名為btnAddImage的影像欄位和按鈕。 下列程式碼會處理自訂設定檔中btnAddImage的click事件。 如您所見，我們會觸發檔案1點按事件。 xdp中不需要編碼即可完成此使用案例

```javascript
$(".btnAddImage").click(function(){

$("#file1").click();

});
```

[自訂設定檔](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html#CreatingCustomProfiles). 使用自訂設定檔可讓您更輕鬆操控行動表單的HTMLDOM物件。 將隱藏的檔案元素添加到HTML.jsp。 當使用者按一下「新增您的像片」時，就會觸發檔案元素的點按事件。 這可讓使用者瀏覽並選取要附加的像片。 然後使用javascript FileReader物件來取得影像的base64編碼字串。 base64影像字串以表單形式儲存在文本欄位中。 提交表單時，我們會擷取此值，並將其插入XML的img元素中。 然後，此XML會用來與xdp合併，以產生最終的pdf。

本文所使用的自訂設定檔已隨本文資產提供給您。

```javascript
function readURL(input) {
            if (input.files && input.files[0]) {
                var reader = new FileReader();
                reader.onload = function (e) {
                  window.formBridge.setFieldValue("xfa.form.topmostSubform.Page1.base64image",reader.result);
                    $('.img img').show();
                     $('.img img')
                        .attr('src', e.target.result)
                        .width(180)
                        .height(200)
                };

                reader.readAsDataURL(input.files[0]);
            }
        }
```

上述程式碼會在觸發檔案元素的click事件時執行。 第5行會擷取上傳檔案的內容為base64字串，並儲存在文字欄位中。 表單提交至我們的servlet時，就會擷取此值。

接著，我們會在AEM中設定行動表單的下列屬性（進階）

* 提交URL - http://localhost:4502/bin/handlemobileformsubmission。 這是我們的Servlet，將提交的資料與xdp範本合併
* HTML呈現設定檔 — 請務必選取「AddImageToMobileForm」。 這會觸發程式碼，將影像新增至表單。

若要在您自己的伺服器上測試此功能，請執行下列步驟：

* [部署AemFormsDocumentServices套件組合](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)

* [使用服務用戶包部署開發](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [下載並安裝與此文章相關聯的套件。](assets/pdf-from-mobile-form-submission.zip)

* 檢視的屬性頁面，以確定提交URL和HTML呈現設定檔已正確設定  [xdp](http://localhost:4502/libs/fd/fm/gui/content/forms/formmetadataeditor.html/content/dam/formsanddocuments/schengen.xdp)

* [以html預覽XDP](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)

* 新增影像至表單並提交。 你應該把影像PDF回來。
