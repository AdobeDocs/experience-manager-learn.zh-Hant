---
title: CRXDE Lite
description: CRXDE Lite是傳統但功能強大的工具，用於偵錯AEM as a Cloud Service開發人員環境。 CRXDE Lite提供了一套功能，可協助進行除錯，使其無法檢查所有資源和屬性、控制JCR的可變部分及調查許可權。
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
kt: KT-5481
thumbnail: kt-5481.jpg
topic: Development
role: Developer
level: Beginner
exl-id: f3f2c89f-6ec1-49d3-91c7-10a42b897780
duration: 125
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 0%

---

# 使用CRXDE Lite除錯AEM as a Cloud Service

CRXDE Lite僅&#x200B;__可在AEM as a Cloud Service開發環境(以及本機AEM SDK)中使用__。

## 在AEM Author上存取CRXDE Lite

CRXDE Lite僅&#x200B;__可__&#x200B;在AEM as a Cloud Service開發環境中存取，且&#x200B;__無法__&#x200B;在預備或生產環境中使用。

若要存取AEM Author上的CRXDE Lite：

1. 登入AEM as a Cloud Service AEM Author服務。
1. 導覽至「工具>一般>CRXDE Lite」

這將使用用來登入AEM Author的認證和許可權來開啟CRXDE Lite。

## 偵錯內容

CRXDE Lite提供對JCR的直接存取。 透過CRXDE Lite看到的內容受限於授予您的使用者的許可權，這表示您可能無法檢視或修改JCR中的所有內容，具體取決於您的存取權。

請注意，`/apps`、`/libs`和`/oak:index`是不可變的，這表示任何使用者都無法在執行階段變更它們。 JCR中的這些位置只能透過程式碼部署來修改。

+ JCR結構可使用左側導覽窗格導覽和操作
+ 在左側導覽窗格中選取節點，會顯示底部窗格中的node屬性。
   + 可以從窗格新增、移除或變更屬性
+ 在左側導覽中連按兩下檔案節點，會在右上窗格中開啟檔案內容
+ 點選左上方的「儲存全部」按鈕以保留已變更的專案，或點選「儲存全部」旁的向下箭頭以還原任何未儲存的變更。

![CRXDE Lite — 偵錯內容](./assets/crxde-lite/debugging-content.png)

在AEM as a Cloud Service開發環境中透過CRXDE Lite在執行階段變更可變內容時，必須謹慎進行。
透過CRXDE Lite直接對AEM所做的任何變更可能很難追蹤和控管。 視情況而定，請確定透過CRXDE Lite進行的變更回到了AEM專案的可變內容套件(`ui.content`)並認可到Git，以確保問題已解決。 理想情況下，所有應用程式內容變更都源自程式碼基底，並透過部署流入AEM，而不是透過CRXDE Lite直接對AEM進行變更。

### 偵錯存取控制

CRXDE Lite提供一種在特定節點上測試和評估特定使用者或群組（亦稱為主體）之存取控制的方法。

若要存取CRXDE Lite中的「測試存取控制」主控台，請瀏覽至：

+ CRXDE Lite>工具>測試存取控制……

![CRXDE Lite — 測試存取控制](./assets/crxde-lite/permissions__test-access-control.png)

1. 使用路徑欄位，選取要評估的JCR路徑
1. 使用「主參與者」欄位，選取路徑評估對象或群組
1. 點選測試按鈕

結果顯示如下：

+ __Path__&#x200B;會重申評估過的路徑
+ __主體__&#x200B;會重申路徑評估對象的使用者或群組
+ __主參與者__&#x200B;列出選取的主參與者所屬的所有主參與者。
   + 這有助於瞭解透過繼承提供許可權的可傳遞群組成員資格
+ __位於路徑__&#x200B;的許可權列出所選主體在評估路徑上的所有JCR許可權

### 不支援的偵錯活動

下列是偵錯活動，這些活動無法&#x200B;__在CRXDE Lite中執行__。

### 偵錯OSGi設定

無法透過CRXDE Lite檢閱已部署的OSGi設定。 OSGi設定會保留在`/apps/example/config.xxx`的AEM專案的`ui.apps`程式碼套件中，不過在部署至AEM as a Cloud Service環境時，OSGi設定資源不會儲存至JCR，因此不會透過CRXDE Lite顯示。

請改用「[Developer Console >設定](./developer-console.md#configurations)」檢閱已部署的OSGi設定。
