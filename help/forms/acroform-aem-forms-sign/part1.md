---
title: Acroforms與AEM Forms
seo-title: Merge Adaptive Form data with Acroform
description: 整合Acroforms與AEM Forms的第1部分。 使用Acroform建立最適化表單並合併資料以取得PDF。
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 0%

---


# 建立Acroform

Acroforms是使用Acrobat建立的表單。 您可以使用Acrobat從頭建立新表單，或取用在Microsoft Word中建立的現有表單，然後使用Acrobat將其轉換為Acroform。 需要執行下列步驟，將在Microsoft Word中建立的表單轉換為Acroform。

* 使用Acrobat的開啟Word文檔
* 使用Acrobat準備表單工具識別表單上的表單欄位。
* 儲存PDF。 請確定檔案名稱中沒有空格。


>[!VIDEO](https://video.tv.adobe.com/v/22575?quality=12&learn=on)

>[!NOTE]
>
>如果您想要將可填寫的頂端傳送給使用Acrobat Sign進行簽署的，請據以為欄位命名。 例如，您可以為欄位命名 **Sig_es_:signer1:簽名**. 這是Acrobat Sign理解的語法。

>[!NOTE]
>
>如果要傳送以XFA為基礎的檔案，您需要平面化檔案，且檔案中的Acrobat Sign簽章標籤必須顯示為靜態文字。

[Acrobat Sign文字標籤檔案](https://helpx.adobe.com/sign/using/text-tag.html)

>[!NOTE]
>
>請確定頂端檔案名稱中沒有任何空格。 目前的范常式式碼不會處理空格。
>
>表單欄位名稱只能包含下列項目：
>
>* 單一空間
>* 單底線
>* 英數字元

