---
title: CRXDE Lite
description: CRXDE Lite是調試as a Cloud Service開發人員環境的經典但功AEM能強大的工具。 CRXDE Lite提供了一套功能，可幫助調試以檢查所有資源和屬性、操作JCR的可變部分和調查權限。
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: KT-5481
thumbnail: kt-5481.jpg
topic: Development
role: Developer
level: Beginner
exl-id: f3f2c89f-6ec1-49d3-91c7-10a42b897780
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 0%

---

# 使用AEMCRXDE Lite調試as a Cloud Service

CRXDE Lite __僅__ 可在AEMas a Cloud Service開發環境(以及本地AEMSDK)上使用。

## 訪問AEM作者的CRXDE Lite

CRXDE Lite __僅__ 可在AEMas a Cloud Service開發環境中訪問， __不__ 在Stage或Production環境中提供。

要訪問AEM作者的CRXDE Lite:

1. 登錄到AEMas a Cloud ServiceAEM作者服務。
1. 導航到「工具」>「常規」>「CRXDE Lite」

這將使用登錄AEM作者時使用的憑據和權限開啟CRXDE Lite。

## 調試內容

CRXDE Lite可直接訪問JCR。 通過CRXDE Lite可見的內容受授予用戶的權限限制，這意味著您可能無法根據您的訪問權限查看或修改JCR中的所有內容。

請注意 `/apps`。 `/libs` 和 `/oak:index` 是不可變的，這意味著任何用戶在運行時不能更改它們。 JCR中的這些位置只能通過代碼部署進行修改。

+ 使用左導航窗格導航和操作JCR結構
+ 在左側導航窗格中選擇一個節點，將在底部窗格中顯示節點屬性。
   + 可以從窗格中添加、刪除或更改屬性
+ 按兩下左導航中的檔案節點，在右上窗格中開啟檔案的內容
+ 按一下左上角的「Save All（全部保存）」按鈕以保持更改，或按一下「Save All to Revert any unsaved changes（全部保存以還原所有未保存的更改）」旁邊的下箭頭。

![CRXDE Lite — 調試內容](./assets/crxde-lite/debugging-content.png)

在as a Cloud Service開發環境中通過CRXDE Lite對AEM可變內容在運行時進行更改必須小心。
任何通過CRXDE Lite直接AEM進行的更改都可能難以跟蹤和管理。 確保通過CRXDE Lite進行的更改返回到項目AEM的可變內容包(`ui.content`)並承諾Git，以確保問題得到解決。 理想情況下，所有應用程式內容更改都源於代碼庫並通過部署AEM流入，而不是直接通過CRXDE Lite進行AEM更改。

### 調試訪問控制

CRXDE Lite提供了test和評估特定用戶或組（又稱主體）的特定節點上的訪問控制的方法。

要在CRXDE Lite中訪問Test訪問控制控制台，請導航到：

+ CRXDE Lite>工具>Test訪問控制……

![CRXDE Lite-Test訪問控制](./assets/crxde-lite/permissions__test-access-control.png)

1. 使用「路徑」欄位，選擇要計算的JCR路徑
1. 使用「承擔者」(Principal)欄位，選擇用戶或組以根據
1. 點擊Test按鈕

結果顯示如下：

+ __路徑__ 重申已評估的路徑
+ __主__ 重申已評估路徑的用戶或組
+ __承擔者__ 列出選定承擔者所屬的所有承擔者。
   + 這有助於瞭解可通過繼承提供權限的傳遞組成員資格
+ __路徑上的權限__ 列出所選主體在評估路徑上擁有的所有JCR權限

### 不支援的調試活動

以下是可以 __不__ 在CRXDE Lite中執行。

### 調試OSGi配置

無法通過CRXDE Lite查看部署的OSGi配置。 OSGi配置在項AEM目中維護 `ui.apps` 代碼包位於 `/apps/example/config.xxx`但是，在部署到AEMas a Cloud Service環境時，OSGi配置資源不會保留到JCR，因此無法通過CRXDE Lite看到。

而是使用 [開發人員控制台>配置](./developer-console.md#configurations) 查看已部署的OSGi配置。
