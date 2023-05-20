---
title: 從Maven項目中刪AEM除示例
description: 瞭解如何清除和刪除由項目原型生AEM成的項目中AEM的示例代碼。
version: Cloud Service
topic: Development
feature: AEM Project Archetype
role: Developer
level: Beginner
kt: 9092
thumbnail: 337263.jpeg
exl-id: 4e10c2b7-41b6-41a0-b8d4-9207a9d3f9c8
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '88'
ht-degree: 6%

---

# 從Maven項目中刪AEM除示例

瞭解如何清除和刪除由項目原型生成AEM的項目中生成AEM的示例代碼。

>[!VIDEO](https://video.tv.adobe.com/v/337263?quality=12&learn=on)


## 資源

+ [馬文AEM計畫原型](https://github.com/adobe/aem-project-archetype)

## 命令

可以執行以下命令以從Maven項目中刪除生成的AEM示例檔案：

```
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/filters \
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/listeners \
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/models \
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/schedulers \
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/servlets \
rm -rf core/src/test/java/com/adobe/aem/wknd/examples/core/* \
rm -rf ui.apps/src/main/content/jcr_root/apps/wknd-examples/components/helloworld \
rm -rf ui.frontend/src/main/webpack/components/_helloworld.js \
rm -rf ui.frontend/src/main/webpack/components/_helloworld.css
```

## 編輯

刪除 `<div class="helloworld" ...></div>` 從：

```
ui.frontend/src/main/webpack/static/index.html
```

刪除 `<helloworld>` 元件實例定義自：

```
ui.content/src/main/content/jcr_root/content/wknd-examples/us/en/.content.xml
```
