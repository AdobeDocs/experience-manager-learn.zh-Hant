---
title: Acroforms與AEM Forms
seo-title: 將最適化表單資料與Acroform合併
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '204'
ht-degree: 0%

---


# 建立Acroform

Acroforms是使用Acrobat建立的表格。 您可以使用Acrobat從頭開始建立新表格，或使用Microsoft Word中建立的現有表格，然後使用Acrobat將它轉換為Acrobat。 必須遵循下列步驟，才能將在Microsoft Word中建立的表單轉換為Acroform。

* 使用Acrobat開啟Word檔案
* 使用Acrobat準備表單工具來識別表單上的表單欄位。
* 儲存PDF。 請確定檔案名稱中沒有空格。


>[!VIDEO](https://video.tv.adobe.com/v/22575?quality=9&learn=on)

>[!NOTE]
>
>如果您想要傳送可填寫的Adobe Sign Acrobat表單以進行簽署，請依此為欄位命名。 例如，您可以為欄位命 **名為Sig_es_:signer1:signature**。 這是Adobe Sign瞭解的語法。

>[!NOTE]
>
>如果您要傳送以XFA為基礎的檔案，則需要平面化檔案，而Adobe Sign簽名標籤必須以靜態文字的形式出現在檔案中。

[Adobe Sign文字標籤檔案](https://helpx.adobe.com/sign/using/text-tag.html)

>[!NOTE]
>
>請確定acroform檔案名稱中沒有空格。 目前的范常式式碼不處理空格。
>
>表單欄位名稱只能包含下列項目：
>
>* 單一空間
>* 單一底線
>* 字母數字字元

