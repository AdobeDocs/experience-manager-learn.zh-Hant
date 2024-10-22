---
title: 在AEM Forms中使用樣式系統
description: 建置主題專案
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Formsas a Cloud Service" before-title="false"
jira: KT-16276
source-git-commit: a0de7eaa391749b6b0d90e7cf3e363c2d5a232b5
workflow-type: tm+mt
source-wordcount: '229'
ht-degree: 0%

---


# 測試變更

根據&#x200B;**&quot;Blank with Core Components&quot;**範本建立調適型表單。 將3個按鈕拖放至表單上，並標示為「企業」、「行銷」和「預設」。
選取如下所示的油漆筆刷，將適當的樣式變體指派給「公司」和「行銷」按鈕。

![樣式](assets/marketing-variation.png)

第三個按鈕將套用預設樣式。

## 建置主題專案

下一步是建置主題專案。 導覽至您的佈景主題專案的根資料夾，然後執行命令&#x200B;_**npm run build**_，如下面的熒幕擷取所示

![建置佈景主題](assets/build-theme.png)

成功建立主題專案後，您就可以測試變更了。

## 測試您的css的快速輕鬆方法

* 開啟主題專案的dist資料夾下的theme.css檔案。選取並複製整個檔案內容。
* 預覽在先前步驟中建立的表單。
* 以滑鼠右鍵按一下其中一個按鈕，然後選取「Inspect」以開啟開發人員主控台。
* 在開發人員主控台中，按一下theme.css以開啟theme.css
* 使用CTR-A選取並刪除theme.css的整個內容，然後按一下刪除按鈕。
* 複製並貼上您在上一步中建立的theme.css內容。
* 按鈕應該會以適當的樣式更新，如下所示。

![最終按鈕](assets/final-state-buttons.png)

