---
title: 從HTL引數化Sling模型
description: 瞭解如何在AEM中從HTL傳遞引數至Sling模型。
version: Cloud Service
topic: Development
feature: Sling Model
role: Developer
jira: KT-15923
level: Intermediate, Experienced
source-git-commit: f45d04b489771b9d3613cb4ce5e7b94d531ac783
workflow-type: tm+mt
source-wordcount: '264'
ht-degree: 0%

---


# 從HTL引數化Sling模型

Adobe Experience Manager (AEM)提供強大的架構，可建立動態且具備調整能力的Web應用程式。 其強大的功能之一是可引數化Sling模型，增強其彈性和可重複使用性。 本教學課程將引導您建立引數化Sling模型，並在HTL (HTML範本語言)中使用它來呈現動態內容。

## HTL指令碼

在此範例中，HTL指令碼傳送兩個引數給`ParameterizedModel` Sling模型。 模型會在其`getValue()`方法中操作這些引數，並傳回顯示結果。

此範例傳遞兩個String引數，但您可以將任何型別的值或物件傳遞至Sling模型，只要[Sling模型欄位型別（以`@RequestAttribute`](#sling-model-implementation)註釋）符合從HTL傳遞的物件型別或值。

```html
<sly data-sly-use.myModel="${'com.example.core.models.ParameterizedModel' @ myParameterOne='Hello', myParameterTwo='World'}"
     data-sly-test.isReady="${myModel.isReady()}">

    <p>
        ${myModel.value}
    </p>

</sly>

<sly data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
     data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!isReady}">
</sly>
```

- **引數化：** `data-sly-use`陳述式會建立具有`myParameterOne`和`myParameterTwo`的`ParameterizedModel`執行個體。
- **條件式演算：** `data-sly-test`在顯示內容之前會先檢查模型是否已準備就緒。
- **預留位置呼叫：** `placeholderTemplate`會處理模型未就緒的情況。

## Sling模型實施

以下說明如何實作Sling模型：

```java
package com.example.core.models.impl;

import org.apache.commons.lang3.StringUtils;
import org.apache.sling.api.SlingHttpServletRequest;
import org.osgi.service.component.annotations.Model;
import org.osgi.service.component.annotations.RequestAttribute;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Model(
    adaptables = { SlingHttpServletRequest.class },
    adapters = { ParameterizedModel.class }
)
public class ParameterizedModelImpl implements ParameterizedModel {
    private static final Logger log = LoggerFactory.getLogger(ParameterizedModelImpl.class);

    // Use the @RequestAttribute annotation to inject the parameter set in the HTL.
    // Note that the Sling Model field can be any type, but must match the type of object or value passed from HTL.
    @RequestAttribute
    private String myParameterOne;

    // If the HTL parameter name is different from the Sling Model field name, use the name attribute to specify the HTL parameter name
    @RequestAttribute(name = "myParameterTwo")
    private String mySecondParameter;

    // Do some work with the parameter values
    @Override
    public String getValue() {
        return myParameterOne + " " + mySecondParameter + ", from the parameterized Sling Model!";
    }

    @Override
    public boolean isReady() {
        return StringUtils.isNotBlank(myParameterOne) && StringUtils.isNotBlank(mySecondParameter);
    }
}
```

- **模型註解：** `@Model`註解將此類別指定為Sling模型，可從`SlingHttpServletRequest`改寫並實作`ParameterizedModel`介面。
- **要求屬性：** `@RequestAttribute`註解會將HTL引數插入模型中。
- **方法：** `getValue()`串連引數，且`isReady()`檢查引數是否非空白。

`ParameterizedModel`介面的定義如下：

```java
package com.example.core.models;

import org.osgi.annotation.versioning.ConsumerType;

@ConsumerType
public interface ParameterizedModel {
    /**
     * Get an example string value from the model. This value is the concatenation of the two parameters.
     * 
     * @return the value of the model
     */
    String getValue();

    /**
     * Check if the model is ready to be used.
     *
     * @return {@code true} if the model is ready, {@code false} otherwise
     */
    boolean isReady();
}
```

## 範例輸出

HTL指令碼使用引數`"Hello"`和`"World"`產生下列輸出：

```html
<p>
    Hello World, from the parameterized Sling Model!
</p>
```

這示範了如何根據透過HTL提供的輸入引數來影響AEM中的引數化Sling模型。

