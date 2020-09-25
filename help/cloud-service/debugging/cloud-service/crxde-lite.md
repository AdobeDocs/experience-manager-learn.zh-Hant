---
title: CRXDE Lite
description: 'CRXDE Lite是一套經典但功能強大的工具，可用來除錯AEM做為雲端服務開發人員環境。 CRXDE Lite提供一套功能，可協助除錯以檢查所有資源和屬性、控制JCR的可變部分並調查權限。 '
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: KT-5481
thumbnail: kt-5481.jpg
translation-type: tm+mt
source-git-commit: 10d0970c1b0debfec7cf94cc106bcae414fb55f6
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 0%

---


# 使用CRXDE Lite將AEM除錯為雲端服務

CRXDE Lite僅 __能在__ AEM上以雲端服務開發環境（以及本機AEM SDK）提供。

## 在AEM作者上存取CRXDE Lite

CRXDE Lite僅 __能在__ AEM中以雲端服務開發環境的形式存取， __在Stage或Production環境中__ 不提供。

若要存取AEM作者的CRXDE Lite:

1. 以雲端服務AEM作者服務的身分登入AEM。
1. 導覽至「工具>一般> CRXDE Lite」

這會使用用來登入AEM Author的認證和權限來開啟CRXDE Lite。

## 除錯內容

CRXDE Lite可讓您直接存取JCR。 透過CRXDE Lite顯示的內容受授予使用者的權限限制，這表示根據您的存取權限，您可能無法檢視或修改JCR中的所有內容。

請注意， `/apps`且是 `/libs` 不可 `/oak:index` 變的，這表示任何使用者在執行時期都無法變更這些項目。 JCR中的這些位置只能透過程式碼部署加以修改。

+ 使用左側導覽窗格導覽和操控JCR結構
+ 在左側導航窗格中選擇節點，將在底部窗格中顯示節點屬性。
   + 屬性可從窗格中新增、移除或變更
+ 連按兩下左側導覽中的檔案節點，在右上方窗格中開啟檔案內容
+ 點選左上角的「全部儲存」按鈕以持續變更，或點選「全部儲存以回復任何未儲存的變更」旁的向下箭頭。

![CRXDE Lite —— 除錯內容](./assets/crxde-lite/debugging-content.png)

在AEM中透過CRXDE Lite的雲端服務開發環境中，在執行時期變更可變內容時，必須小心。
透過CRXDE Lite直接對AEM所做的任何變更都可能難以追蹤和管理。 請視需要確保透過CRXDE Lite所做的變更返回AEM專案的可變內容套件(`ui.content`)並提交至Git，以確保問題已解決。 理想上，所有應用程式內容變更都源自程式碼庫，並透過部署流入AEM，而非透過CRXDE Lite直接對AEM進行變更。

### 除錯存取控制

CRXDE Lite提供一種方法，可測試和評估特定用戶或組的特定節點（又稱為承擔者）的訪問控制。

要訪問CRXDE Lite中的測試訪問控制控制台，請導航到：

+ CRXDE Lite >工具>測試存取控制……

![CRXDE Lite —— 測試存取控制](./assets/crxde-lite/permissions__test-access-control.png)

1. 使用「路徑」欄位，選擇要評估的JCR路徑
1. 使用「承擔者」(Principal)欄位，選擇用戶或組以根據
1. 點選「測試」按鈕

結果顯示如下：

+ __Path__ rextens the path that was exparated the path that exparated
+ __承擔者__ (Principal)重申已評估路徑的用戶或組
+ __承擔者__ (Principals)列出選定承擔者所屬的所有承擔者。
   + 這有助於瞭解透過繼承提供權限的傳遞性群組成員資格
+ __Path上的權限__ (Privileges at Path)列出所選承擔者在評估路徑上擁有的所有JCR權限

### 不支援的除錯活動

以下是無法在CRXDE Lite中執 __行__ 的除錯活動。

### 調試OSGi配置

部署的OSGi配置無法通過CRXDE Lite進行審核。 OSGi組態會維護在AEM Project的 `ui.apps` 程式碼套件中，位址為 `/apps/example/config.xxx`，但是當部署至AEM做為雲端服務環境時，OSGi組態資源不會持續存留至JCR，因此無法透過CRXDE Lite顯示。

請改用「開發人 [員主控台>設定](./developer-console.md#configurations) 」來檢視已部署的OSGi設定。
