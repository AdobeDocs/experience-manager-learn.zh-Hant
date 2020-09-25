---
title: 在表單提交時觸發AEM工作流程
description: 設定「最適化表單」以觸發AEM工作流程。
sub-product: 表單
feature: workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
kt: 5407
thumbnail: kt-5407.jpg
translation-type: tm+mt
source-git-commit: 738e356c4e72e0c3518bb5fd4ad6a076522e9f5c
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 0%

---


# 設定最適化表單以觸發AEM工作流程

* 下載最 [適化表單](assets/time-off-application.zip)
* 瀏覽至表 [單和檔案](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 按一下「建立->檔案上傳」。 選擇步驟1中下載的表單
* 在編輯 [模式中開啟最適化表單](http://localhost:4502/editor.html/content/forms/af/timeofapplication.html)。
* 開啟內容檔案總管
   ![內容檔案總管](assets/af-workflow-submission.PNG)
* 選擇「表單容器」節點並開啟其配置屬性
   ![提交](assets/af-workflow-submission1.PNG)
* 展開「提交」面板
* 如上述螢幕擷取畫面中所指定，設定表單的提交動作。
   _請務必記下「資料檔案路徑」欄位中指定的值。 此值必須與您在工作流程「指派任務」元件的預先填入區段中指定的值相符。_

現在當您填寫並提交最適化表單時，會觸發與表單的提交動作相關的工作流程。
