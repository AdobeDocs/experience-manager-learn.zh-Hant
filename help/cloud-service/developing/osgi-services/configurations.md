---
title: OSGi設定屬性
description: 瞭解OSGi設定屬性的基本概念，以及如何在OSGi服務中使用這些屬性。
role: Developer
level: Beginner
topic: Development
feature: OSGI
jira: KT-8268
thumbnail: 335729.jpeg
exl-id: 096b0a95-7039-4570-b567-ba316bfc8709
duration: 784
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '52'
ht-degree: 1%

---

# OSGi設定屬性

瞭解使用OSGi設定索引鍵/值配對來定義及公開OSGi設定資料給OSGi服務的低階方法。

>[!VIDEO](https://video.tv.adobe.com/v/335729?quality=12&learn=on)

## 資源

+ [@Component JavaDocs](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/org/osgi/service/component/annotations/Component.html)
+ [@Activate JavaDocs](https://javadoc.io/static/com.adobe.aem/aem-sdk-api/2021.7.5658.20210723T140305Z-210600/org/osgi/service/component/annotations/Activate.html)

## 程式碼

### ActivitiesImpl.java

`/core/src/main/java/com/adobe/aem/wknd/examples/core/adventures/impl/ActivitiesImpl.java`

```java
package com.adobe.aem.wknd.examples.core.adventures.impl;

import java.util.Map;
import java.util.Random;

import com.adobe.aem.wknd.examples.core.adventures.Activities;

import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Deactivate;
import org.osgi.util.converter.Converters;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Component(
    service = { Activities.class },
    property = {
        "activities=Hiking",
        "activities=Jogging",
        "activities=Walking",
        "random.seed:Integer=10"
    }
)
public class ActivitiesImpl implements Activities {
    private static final Logger log = LoggerFactory.getLogger(ActivitiesImpl.class);

    private String[] activities;

    private Random random;

    /**
     * @return the name of a random WKND adventure activity
     */
    public String getRandomActivity() {
        int randomIndex = random.nextInt(activities.length);
        return activities[randomIndex];
    }    

    @Activate
    protected void activate(Map<String, Object> properties) {

        this.activities = Converters.standardConverter()
                .convert(properties.get("activities"))
                .defaultValue(new String[] { 
                    "Default Activity 1",
                    "Default Activity 2"
                })
                .to(String[].class);

        final Integer randomSeed = Converters.standardConverter()
                .convert(properties.get("random.seed"))
                .defaultValue(25)
                .to(Integer.class);   
        
        this.random = new Random(randomSeed);

        log.info("Activated ActivitiesImpl with activities [ {} ]", String.join(", ", this.activities));
    }

    @Deactivate
    protected void deactivate() {
        log.info("ActivitiesImpl has been deactivated!");
    }
}
```

### com.adobe.aem.wknd.examples.core.adventures.impl.ActivitiesImpl.cfg.json

`/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config/com.adobe.aem.wknd.examples.core.adventures.impl.ActivitiesImpl.cfg.json`

```javascript
{
    "activities": [ "Skateboarding", "Surfing", "Skiing" ],
    "random.seed:Integer": 32
}
```
