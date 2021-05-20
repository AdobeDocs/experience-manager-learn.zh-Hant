---
title: CRXDE Lite
description: 'CRXDE Lite是傳統但功能強大的工具，可作為Cloud Service開發人員環境除錯AEM。 CRXDE Lite提供一套功能，可協助偵錯以檢查所有資源和屬性、控制JCR的可變部分及調查權限。 '
feature: 開發人員工具
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: KT-5481
thumbnail: kt-5481.jpg
topic: 開發
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '616'
ht-degree: 0%

---


# 將AEM作為Cloud Service進行CRXDE Lite

CRXDE Lite __僅__&#x200B;適用於AEM作為Cloud Service開發環境(以及本機AEM SDK)。

## 存取AEM作者的CRXDE Lite

CRXDE Lite __僅__&#x200B;可在AEM作為Cloud Service開發環境存取，且&#x200B;__不__&#x200B;可在預備或生產環境中存取。

若要存取AEM作者的CRXDE Lite:

1. 以Cloud ServiceAEM作者服務身分登入AEM。
1. 導覽至「工具>一般>CRXDE Lite」

這會使用用來登入AEM作者的憑證和權限開啟CRXDE Lite。

## 為內容除錯

CRXDE Lite可直接存取JCR。 透過CRXDE Lite顯示的內容受授予使用者的權限限制，這表示根據您的存取權限，您可能無法查看或修改JCR中的所有內容。

請注意，`/apps`、`/libs`和`/oak:index`不可變，這表示任何使用者無法在執行階段變更它們。 JCR中的這些位置只能透過程式碼部署加以修改。

+ 使用左側導航窗格導航和操作JCR結構
+ 在左側導航窗格中選擇節點時，會在底部窗格中顯示節點屬性。
   + 可以從窗格中添加、刪除或更改屬性
+ 按兩下左側導覽中的檔案節點，在右上方窗格中開啟檔案的內容
+ 點選左上角的「全部儲存」按鈕以持續變更，或點選「全部儲存」旁的向下箭頭以還原任何未儲存的變更。

![CRXDE Lite — 除錯內容](./assets/crxde-lite/debugging-content.png)

在AEM中以Cloud Service開發環境的形式，透過CRXDE Lite在執行階段變更可變動內容，必須謹慎執行。
透過CRXDE Lite直接對AEM所做的任何變更都可能難以追蹤及控管。 視情況確定透過CRXDE Lite所做的變更會回到AEM專案的可變動內容套件(`ui.content`)並提交至Git，以確保問題已解決。 理想情況下，所有應用程式內容變更都源自程式碼基底，並透過部署流入AEM，而非透過CRXDE Lite直接對AEM進行變更。

### 調試訪問控制

CRXDE Lite提供了測試和評估特定用戶或組（也稱為主體）的特定節點上的訪問控制的方法。

若要存取CRXDE Lite中的「測試存取控制」主控台，請導覽至：

+ CRXDE Lite>工具>測試存取控制……

![CRXDE Lite — 測試存取控制](./assets/crxde-lite/permissions__test-access-control.png)

1. 使用「路徑」欄位，選擇要評估的JCR路徑
1. 使用「承擔者」(Principal)欄位，選擇要根據此路徑評估的用戶或組
1. 點選「測試」按鈕

結果顯示如下：

+ ____ Path重申已評估的路徑
+ ____ Principal重申已評估路徑的用戶或組
+ ____ 原則會列出所選主要屬於的所有主要。
   + 這有助於了解可透過繼承提供權限的傳遞式群組成員資格
+ __Path的權__ 限列出所選主體在評估路徑上擁有的所有JCR權限

### 不支援的調試活動

以下是可在CRXDE Lite中執行&#x200B;__not__&#x200B;的偵錯活動。

### 對OSGi配置進行調試

無法透過CRXDE Lite檢閱已部署的OSGi設定。 AEM專案的`ui.apps`程式碼套件中會維護OSGi設定，但部署至AEM做為Cloud Service環境時，OSGi設定資源不會持續保存至JCR，因此無法透過CRXDE Lite顯示。`/apps/example/config.xxx`

請改為使用[開發人員控制台>配置](./developer-console.md#configurations)來查看已部署的OSGi配置。
