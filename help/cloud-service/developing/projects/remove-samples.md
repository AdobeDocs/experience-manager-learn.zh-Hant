---
title: 從AEM Maven專案移除範例
description: 了解如何從AEM專案原型產生的AEM專案中清除和移除范常式式碼。
version: Cloud Service
topic: Development
feature: AEM Project Archetype
role: Developer
level: Beginner
kt: 9092
thumbnail: 337263.jpeg
source-git-commit: e8b3bcaeee40b4bfd4f967f929ad664e8d168cb0
workflow-type: tm+mt
source-wordcount: '88'
ht-degree: 6%

---


# 從AEM Maven專案中移除範例

了解如何從AEM專案原型產生的AEM專案中清除並移除產生的范常式式碼。

>[!VIDEO](https://video.tv.adobe.com/v/337263/?quality=12&learn=on)


## 資源

+ [AEM Maven專案原型](https://github.com/adobe/aem-project-archetype)

## 命令

可執行下列命令，以從AEM Maven專案中移除產生的範例檔案：

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

移除 `<div class="helloworld" ...></div>` 從：

```
ui.frontend/src/main/webpack/static/index.html
```

移除 `<helloworld>` 元件實例定義來自：

```
ui.content/src/main/content/jcr_root/content/wknd-examples/us/en/.content.xml
```