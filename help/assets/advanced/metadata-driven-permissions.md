---
title: AEM Assets中的中繼資料導向許可權
description: 中繼資料導向許可權是一項功能，可根據資產中繼資料屬性而非檔案夾結構來限制存取。
version: Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Intermediate
jira: KT-13757
doc-type: Tutorial
last-substantial-update: 2024-05-03T00:00:00Z
exl-id: 57478aa1-c9ab-467c-9de0-54807ae21fb1
source-git-commit: 98b26eb15c2fe7d1cf73fe028b2db24087c813a5
workflow-type: tm+mt
source-wordcount: '738'
ht-degree: 0%

---

# 中繼資料導向的許可權{#metadata-driven-permissions}

中繼資料導向許可權是一項功能，可讓AEM Assets Author的存取控制決定以資產中繼資料屬性為基礎，而非檔案夾結構。 透過此功能，您可以定義存取控制原則來評估屬性，例如資產狀態、型別或任何您定義的自訂中繼資料屬性。

讓我們來看一個範例。 創意人員將其工作上傳至AEM Assets至行銷活動相關資料夾，可能是未獲核准使用的進行中資產。 我們希望確保行銷人員只看見此行銷活動的已核准資產。 我們可以運用中繼資料屬性，指出資產已核准，且可供行銷人員使用。

## 運作方式

啟用中繼資料導向許可權涉及定義哪些資產中繼資料屬性將驅動存取限制，例如「狀態」或「品牌」。 然後，這些屬性可用於建立存取控制專案，以指定哪些使用者群組有權存取具有特定屬性值的資產。

## 先決條件

若要設定中繼資料導向的許可權，必須存取更新至最新版本的AEMas a Cloud Service環境。

## OSGi設定 {#configure-permissionable-properties}

若要實作中繼資料導向許可權，開發人員必須將OSGi設定部署至AEMas a Cloud Service，如此可啟用特定資產中繼資料屬性，以支援中繼資料導向許可權。

1. 決定哪些資產中繼資料屬性將用於存取控制。 屬性名稱是資產上的JCR屬性名稱 `jcr:content/metadata` 資源。 在我們的案例中，這會是名為的屬性 `status`.
1. 建立OSGi設定 `com.adobe.cq.dam.assetmetadatarestrictionprovider.impl.DefaultRestrictionProviderConfiguration.cfg.json` 在您的AEM Maven專案中。
1. 將下列JSON貼到建立的檔案中：

   ```json
   {
     "restrictionPropertyNames":[
       "status",
       "brand"
     ],
     "enabled":true
   }
   ```

1. 以必要的值取代屬性名稱。

## 重設基本資產許可權

新增限制型存取控制專案之前，應先新增頂層專案，以首先拒絕受限於資產（例如「貢獻者」或類似專案）許可權評估的所有群組的讀取存取：

1. 導覽至 __工具→安全性→許可權__ 畫面
1. 選取 __貢獻者__ 群組（或所有使用者群組所屬的其他自訂群組）
1. 按一下 __新增ACE__ 在畫面的右上角
1. 選取 `/content/dam` 的 __路徑__
1. 輸入 `jcr:read` 的 __許可權__
1. 選取 `Deny` 的 __許可權型別__
1. 在「限制」底下，選取 `rep:ntNames` 並輸入 `dam:Asset` 作為 __限制值__
1. 按一下 __儲存__

![拒絕存取](./assets/metadata-driven-permissions/deny-access.png)

## 透過中繼資料授予資產存取權

現在可以新增存取控制專案，以根據 [已設定的資產中繼資料屬性值](#configure-permissionable-properties).

1. 導覽至 __工具→安全性→許可權__ 畫面
1. 選取應具備資產存取權的使用者群組
1. 按一下 __新增ACE__ 在畫面的右上角
1. 選取 `/content/dam` （或子資料夾） __路徑__
1. 輸入 `jcr:read` 的 __許可權__
1. 選取 `Allow` 的 __許可權型別__
1. 在 __限制__，選取其中一項 [在OSGi設定中設定的資產中繼資料屬性名稱](#configure-permissionable-properties)
1. 在中輸入必要的中繼資料屬性值 __限制值__ 欄位
1. 按一下 __+__ 圖示以新增限制至存取控制專案
1. 按一下 __儲存__

![允許存取](./assets/metadata-driven-permissions/allow-access.png)

## 有效中繼資料導向許可權

範例資料夾包含一些資產。

![管理員檢視](./assets/metadata-driven-permissions/admin-view.png)

設定許可權並相應地設定資產中繼資料屬性後，使用者（在此案例中為行銷人員使用者）將只會看到核准的資產。

![行銷人員檢視](./assets/metadata-driven-permissions/marketeer-view.png)

## 優點與考量事項

中繼資料導向許可權的優點包括：

- 根據特定屬性對資產存取進行微調控制。
- 將存取控制原則與檔案夾結構分離，讓資產組織更具彈性。
- 能夠根據多個中繼資料屬性定義複雜的存取控制規則。

>[!NOTE]
>
> 請務必注意：
> 
> - 中繼資料屬性是依據限制評估，使用 __字串相等__ (`=`) (尚未支援其他資料型別或運運算元，適用於大於(`>`)或日期屬性)
> - 若要允許限制屬性有多個值，可以從「選取型別」下拉式清單中選取相同屬性，並輸入新的限制值(例如 `status=approved`， `status=wip`)並按一下「+」以新增專案限制
> ![允許多個值](./assets/metadata-driven-permissions/allow-multiple-values.png)
> - __AND限制__ 可透過具有不同屬性名稱(例如， `status=approved`， `brand=Adobe`)將評估為AND條件，也就是說，選取的使用者群組將被授與資產的讀取存取權， `status=approved AND brand=Adobe`
> ![允許多重限制](./assets/metadata-driven-permissions/allow-multiple-restrictions.png)
> - __OR限制__ 透過新增具有中繼資料屬性限制的新存取控制專案來支援，將為專案建立OR條件，例如，具有限制的單一專案 `status=approved` 和單一專案，包含 `brand=Adobe` 將會評估為 `status=approved OR brand=Adobe`
> ![允許多重限制](./assets/metadata-driven-permissions/allow-multiple-aces.png)
