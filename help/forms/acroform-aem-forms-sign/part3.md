---
title: AEM Forms
seo-title: Merge Adaptive Form data with Acroform
description: 將Acroforms與AEM Forms整合的教程第3部分。 Test系統上的工作流和自適應表單。
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 1%

---


# Test系統上的此功能

[下載並導入此包AEM](assets/acro-form-aem-form.zip)
此包包含示例工作流和html頁，該頁允許您從上載的頂層表單建立架構。

## 配置工作流

1. [在編輯模式下開啟工作流模型](http://localhost:4502/editor.html/conf/global/settings/workflow/models/MergeAcroformData.html)。
2. 開啟MergeAcroformData步驟的配置屬性。
3. 按一下「Process（流程）」頁籤。
4. 確保您傳遞的參數是伺服器上的有效資料夾。
5. 儲存變更。

## 建立自適應窗體

1. 使用在前一步中建立的架構建立自適應表單。
2. 將幾個架構元素拖放到自適應窗體中。
3. 將自適應表單的提交操作配置為提交AEM到工作流(MergeAcroformData)。
4. **確保將資料檔案路徑指定為「Data.xml」。 這非常重要，因為示例代碼在工作流負載中查找名為Data.xml的檔案。**
5. 預覽自適應表單，填寫表單並提交。
6. 您應看到與配置工作流下的資料合併後保存到步驟4中指定的資料夾中的PDF

>[!NOTE]
>
>通過合併資料與頂層表單生成的pdf將保存為工作流負載資料夾下的pdfdocument.pdf。 然後，可以將此文檔用作工作流的一部分，用於進一步處理
