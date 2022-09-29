---
title: 將裝載文檔寫入檔案系統
description: 將駐留在裝載資料夾下的寫入文檔添加到檔案系統的自定義過程步驟
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-9859
exl-id: bab7c403-ba42-4a91-8c86-90b43ca6026c
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 0%

---

# 將文檔寫入檔案系統

常見的使用案例是將工作流程中產生的檔案寫入檔案系統。
此自訂工作流程處理步驟可輕鬆將工作流程檔案寫入檔案系統。
自訂程式採用下列逗號分隔引數

```java
ChangeBeneficiary.pdf,c:\confirmation
```

第一個參數是要保存到檔案系統的文檔的名稱。 第二個參數是要保存文檔的資料夾位置。 例如，在上述使用案例中，文檔被寫入 `c:\confirmation\ChangeBeneficiary.pdf`

下列螢幕擷取畫面顯示您需要傳遞至自訂程式步驟的引數
![write-payload-file-system](assets/write-payload-file-system.png)

[自訂套件可從此處下載](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
