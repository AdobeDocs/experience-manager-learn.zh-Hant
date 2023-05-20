---
title: 從HTM5表單提交生成PDF
description: 從移動表單提交生成PDF
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

本文將引導您完成從HTML5(亦稱移動Forms)表單提交中生成pdf所涉及的步驟。 本演示還將說明將影像添加到HTML5窗體並將影像合併到最終pdf中所需的步驟。


要將提交的資料合併到xdp模板中，我們將執行以下操作

編寫Servlet來處理HTML5表單提交

* 在此Servlet中獲取已提交資料
* 將此資料與xdp模板合併以生成pdf
* 將pdf流回調用應用程式

以下是從請求中提取已提交資料的Servlet代碼。 然後，它調用自定義documentServices .mobileFormToPDF方法以獲取pdf。

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

要將影像添加到移動表單並在pdf中顯示該影像，我們使用了以下內容

XDP模板 — 在xdp模板中，我們添加了一個名為btnAddImage的影像欄位和按鈕。 以下代碼處理我們的自定義配置檔案中btnAddImage的click事件。 正如您所看到的，我們觸發了檔案1 click事件。 xdp中不需要編碼即可完成此使用情形

```javascript
$(".btnAddImage").click(function(){

$("#file1").click();

});
```

[自定義配置檔案](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html#CreatingCustomProfiles)。 使用自定義配置檔案，可以更輕鬆地操作移動表單的HTMLDOM對象。 隱藏檔案元素將添加到HTML.jsp中。 當用戶按一下「添加照片」時，我們將觸發檔案元素的按一下事件。 這允許用戶瀏覽並選擇要附加的照片。 然後使用javascript FileReader對象獲取影像的base64編碼字串。 base64影像字串以窗體形式儲存在文本欄位中。 提交表單時，我們提取此值並將其插入XML的img元素中。 然後，此XML將用於與xdp合併以生成最終pdf。

已將用於此文章的自定義配置檔案作為本文資產的一部分提供給您。

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

在觸發檔案元素的click事件時執行上述代碼。 第5行將上傳檔案的內容提取為base64字串，並儲存在文本欄位中。 然後，當表單提交到我們的servlet時，將提取此值。

然後，我們將配置我們的移動表單的以下屬性（高級）AEM

* 提交URL - http://localhost:4502/bin/handlemobileformsubmission。 這是我們的Servlet，它將提交的資料與xdp模板合併
* HTML呈現配置檔案 — 確保選擇「AddImageToMobileForm」。 這將觸發向表單添加影像的代碼。

要在您自己的伺服器上test此功能，請執行以下步驟：

* [部署AemFormsDocumentServices捆綁包](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)

* [部署使用服務用戶包進行開發](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [下載並安裝與此文章關聯的包。](assets/pdf-from-mobile-form-submission.zip)

* 通過查看URL的屬性頁，確保正確設定提交URL和HTML呈現配置檔案  [xdp](http://localhost:4502/libs/fd/fm/gui/content/forms/formmetadataeditor.html/content/dam/formsanddocuments/schengen.xdp)

* [將XDP以html格式預覽](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)

* 將影像添加到表單並提交。 你應該把PDF帶回來。
