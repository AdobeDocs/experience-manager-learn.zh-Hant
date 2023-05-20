---
title: 將負載文檔寫入檔案系統
description: 將駐留在負載資料夾下的寫入文檔添加到檔案系統的自定義過程步驟
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-9859
exl-id: bab7c403-ba42-4a91-8c86-90b43ca6026c
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 0%

---

# 將文檔寫入檔案系統

常見使用情形是將工作流中生成的文檔寫入檔案系統。
此自定義工作流進程步驟使將工作流文檔寫入檔案系統變得容易。
自定義進程採用以下逗號分隔的參數

```java
ChangeBeneficiary.pdf,c:\confirmation
```

第一個參數是要保存到檔案系統的文檔的名稱。 第二個參數是要保存文檔的資料夾位置。 例如，在上述使用情形中，文檔將寫入 `c:\confirmation\ChangeBeneficiary.pdf`

以下螢幕抓圖顯示了需要傳遞到自定義進程步驟的參數
![寫負載檔案系統](assets/write-payload-file-system.png)

[可從此處下載自定義捆綁包](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
