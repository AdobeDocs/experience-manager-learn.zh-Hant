---
title: AEM Assets中的資產報表
description: 'AEM Assets提供企業級報告架構，可透過直覺式的使用體驗，針對大型資料庫進行擴充。 '
feature: reports
topics: authoring, operations, performance, metadata
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
kt: 648
translation-type: tm+mt
source-git-commit: 10784dce34443adfa1fc6dc324242b1c021d2a17
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 2%

---


# 資產報表{#using-reports-in-aem-assets}

AEM Assets提供企業級報告架構，可透過直覺式的使用體驗，針對大型資料庫進行擴充。

>[!VIDEO](https://video.tv.adobe.com/v/22140/?quality=12&learn=on)

## Microsoft Excel公式{#excel-formulas}

在視訊中使用下列公式，以在Microsoft Excel中產生「依大小劃分的資產」圖表。

### 資產大小標準化為位元組{#asset-size-normalization-to-bytes}

```
=IF(RIGHT(D2,2)="KB",
      LEFT(D2,(LEN(D2)-2))*1024,
  IF(RIGHT(D2,2)="MB",
      LEFT(D2,(LEN(D2)-2))*1024*1024,
  IF(RIGHT(D2,2)="GB",
      LEFT(D2,(LEN(D2)-2))*1024*1024*1024,
  IF(RIGHT(D2,2)="TB",
      LEFT(D2,(LEN(D2)-2))*1024*1024*1024*1024, 0))))
```

### 依大小{#asset-count-by-size}劃分的資產計數

#### 小於200 KB {#less-than-kb}

```
=COUNTIFS(E2:E1000,"< 200000")
```

#### 200 KB到500 KB {#kb-to-kb}

```
=COUNTIFS(E2:E1000,">= 200000", E2:E1000,"<= 500000")
```

#### 大於500 KB {#greater-than-kb}

```
=COUNTIFS(E2:E1000,"> 500000")
```

## 其他資源{#additional-resources}

下載[所有資產Excel檔案及Chart](./assets/asset-reports/all-assets.xlsx)
