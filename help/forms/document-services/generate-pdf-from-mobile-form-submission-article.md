---
title: 從HTM5表單提交產生PDF
seo-title: 從HTML5表單提交產生PDF
description: 從Mobile Form Submission產生PDF
seo-description: 從Mobile Form Submission產生PDF
uuid: 61f07029-d440-44ec-98bc-f2b5eef92b59
feature: 行動表單
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 816f1a75-6ceb-457b-ba18-daf229eed057
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '583'
ht-degree: 0%

---


# 從HTM5表單提交{#generate-pdf-from-htm-form-submission}產生PDF

本文將引導您逐步瞭解從HTML5(又稱為行動Forms)表單提交產生pdf時所涉及的步驟。 此示範程式也將說明將影像新增至HTML5表單並將影像合併為最終pdf所需的步驟。

若要即時展示此功能，請造訪[範例伺服器](https://forms.enablementadobe.com/content/samples/samples.html?query=0)，並搜尋「行動表單至PDF」。

要將提交的資料合併到xdp模板中，我們將執行以下操作

編寫servlet來處理HTML5表單提交

* 在此servlet內獲取已提交資料
* 將此資料與xdp範本合併，以產生pdf
* 將pdf串流回呼叫應用程式

以下是從請求中擷取已提交資料的servlet程式碼。 然後，它會呼叫自訂documentServices .mobileFormToPDF方法，以取得pdf。

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

若要將影像新增至行動表單，並在PDF中顯示該影像，我們使用下列功能

XDP模板——在xdp模板中，我們添加了一個名為btnAddImage的影像欄位和按鈕。 下列程式碼可處理自訂描述檔中btnAddImage的click事件。 如您所見，我們會觸發file1 click事件。 xdp中不需要編寫程式碼即可完成此使用案例

```javascript
$(".btnAddImage").click(function(){

$("#file1").click();

});
```

[自訂設定檔](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html#CreatingCustomProfiles)。使用自訂描述檔，可更輕鬆地控制行動表單的HTML DOM物件。 隱藏的檔案元素會新增至HTML.jsp。 當使用者按一下「新增像片」時，我們會觸發檔案元素的點按事件。 這可讓使用者瀏覽並選取要附加的像片。 然後，我們使用javascript FileReader物件來取得影像的base64編碼字串。 base64影像字串會儲存在表單的文字欄位中。 提交表單時，我們會擷取此值，並將它插入XML的img元素中。 然後，此XML會用來與xdp合併，以產生最終的pdf。

本文使用的自訂描述檔已提供給您，做為本文資產的一部分。

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

當我們觸發檔案元素的click事件時，會執行上述程式碼。 第5行會將上傳檔案的內容擷取為base64字串，並儲存在文字欄位中。 然後，當表單提交到我們的servlet時，會提取此值。

然後，我們會在

* 提交URL - http://localhost:4502/bin/handlemobileformsubmission。 這是我們的servlet，它將提交的資料與xdp模板合併
* HTML演算描述檔——請確定您選取「AddImageToMobileForm」。 這會觸發程式碼，以新增影像至表單。

若要在您自己的伺服器上測試此功能，請遵循下列步驟：

* [部署AemFormsDocumentServices套件](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)

* [部署使用服務使用者套件進行開發](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [下載並安裝與此文章相關的套件。](assets/pdf-from-mobile-form-submission.zip)

* 檢視[xdp](http://localhost:4502/libs/fd/fm/gui/content/forms/formmetadataeditor.html/content/dam/formsanddocuments/schengen.xdp)的屬性頁面，確定已正確設定提交URL和HTML Render描述檔

* [將XDP預覽為html](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)

* 新增影像至表單並送出。 您應將PDF放回影像中。

