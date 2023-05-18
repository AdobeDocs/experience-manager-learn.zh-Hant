---
title: 擷取要內嵌的最適化表單的JSON
description: 使用API擷取最適化表單的json
feature: Adaptive Forms
version: 6.5
kt: 13285
topic: Development
role: User
level: Intermediate
source-git-commit: c6e83a627743c40355559d9cdbca2b70db7f23ed
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 0%

---


# 擷取表單的JSON

登入您的AEM Forms製作例項，並使用 **核心元件空白** 範本。 將表單發佈至發佈執行個體。

若要內嵌表單，我們會先對發佈伺服器發出get呼叫，以擷取最適化表單的json。

下列程式碼片段會擷取最適化表單的json，稱為 **聯絡圖**

```javascript
const getForm = async () => {
        const resp = await fetch('/content/forms/af/contactus/jcr:content/guideContainer.model.json');
        let formJSON = await resp.json();
        console.log(formJSON);
        setForm(formJSON);
      }
```

Contact功能元件的完整代碼如下

```javascript
import Form from './components/Form';
import PlainText from './components/plainText';
import TextField from './components/TextField';
import Button from './components/Button';
import Panel from './components/Panel';
import { useState,useEffect } from "react";
import { AdaptiveForm } from "@aemforms/af-react-renderer";

export default function Contact(){
   const [selectedForm, setForm] = useState("");
   const extendMappings = {
        'plain-text' : PlainText,
        'text-input' : TextField,
        'button' : Button,
        'form': Form
      };
    const getForm = async () => {
        
        const resp = await fetch('/content/forms/af/contactus/jcr:content/guideContainer.model.json');
        let formJSON = await resp.json();
        console.log(formJSON);
        setForm(formJSON);
      }
      useEffect( ()=>{
        getForm();
        

    },[]);
    return(
        
        <div>
            <h1>Get in touch with us!!!!</h1>
            <AdaptiveForm mappings={extendMappings} formJson={selectedForm} />
      
          
        </div>
    )
}
```

上述程式碼使用原生html元件，這些元件會對應至適用性表單中使用的元件。 例如，我們會將文字輸入適用性表單元件對應至TextField元件。 文章中使用的原生元件 [可從此處下載](./assets/native-components.zip)