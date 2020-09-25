---
title: 在AEM資產中使用階層式中繼資料
seo-title: 在AEM資產中使用階層式中繼資料
description: 進階中繼資料管理可讓使用者建立階層式欄位規則，以在AEM Assets的中繼資料之間建立內容相關關係。 以下視訊示範欄位需求、可見度和情境選擇的新動態規則。 視訊還詳細說明管理員將這些規則套用至自訂中繼資料架構所需的步驟。
seo-description: 進階中繼資料管理可讓使用者建立階層式欄位規則，以在AEM Assets的中繼資料之間建立內容相關關係。 以下視訊示範欄位需求、可見度和情境選擇的新動態規則。 視訊還詳細說明管理員將這些規則套用至自訂中繼資料架構所需的步驟。
uuid: 470c1b1a-f888-4c90-87d7-acfa9a5fa6b1
discoiquuid: ccd1acb1-bb7f-48c2-91e0-cccbeedad831
topics: metadata
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '318'
ht-degree: 0%

---


# 在AEM資產中使用階層式中繼資料{#using-cascading-metadata-in-aem-assets}

進階中繼資料管理可讓使用者建立階層式欄位規則，以在AEM Assets的中繼資料之間建立內容相關關係。 以下視訊示範欄位需求、可見度和情境選擇的新動態規則。 視訊還詳細說明管理員將這些規則套用至自訂中繼資料架構所需的步驟。

>[!VIDEO](https://video.tv.adobe.com/v/20702/?quality=9&learn=on)

可為指定中繼資料欄位啟用三個動態規則集：

1. **需求** :可以根據另一個下拉式欄位的值，動態標示欄位為必要欄位。

2. **可見度** :欄位永遠可以顯示，也只能根據其他下拉式欄位的值顯示。

3. **選擇** :（僅適用於下拉式欄位）根據其他下拉式欄位的目前選取值，篩選顯示給使用者的選項。

>[!NOTE]
>
>階層式規則只能根據下拉式欄位的值來建立。 您可將這三個規則集套用至相同的中繼資料欄位，但建議您最好將每個規則集依賴於相同的中繼資料下拉式清單。

下載自 [訂中繼資料套件](assets/cascade-metadata-values-001.zip)

## 其他資源{#additional-resources}

自訂中繼資料結構建立於： `/conf/global/settings/dam/adminui-extension/metadataschema/custom`. 以下AEM套件將會將自訂架構套用至資料夾： `/content/dam/we-retail/en/activities`:

