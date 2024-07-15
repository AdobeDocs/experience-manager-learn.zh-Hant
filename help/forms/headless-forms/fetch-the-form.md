---
title: 擷取最適化表單的JSON以內嵌
description: 使用API擷取最適化表單的json
feature: Adaptive Forms
version: 6.5
jira: KT-13285
topic: Development
role: User
level: Intermediate
exl-id: ee534724-54ea-48e1-8c92-de1c56a928d4
duration: 50
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 1%

---

# 擷取表單的JSON

登入您的AEM Forms作者執行個體，並使用&#x200B;**Blank with Core Components**&#x200B;範本建立新的最適化。 將您的表單Publish至發佈執行個體。

若要內嵌表單，我們會先對發佈伺服器發出get呼叫，擷取最適化表單的json。

下列程式碼片段會擷取名為&#x200B;**contactus**&#x200B;的最適化表單的json

```javascript
const getForm = async () => {
        
        const resp = await fetch('/adobe/forms/af/L2NvbnRlbnQvZm9ybXMvYWYvZmlyc3RoZWFkbGVzcw==');
        // Get the form id manually using the listform api
        let formJSON = await resp.json();
        console.log("The contact form json is "+formJSON);
        setForm(formJSON.afModelDefinition)
      }
```

Contact函式元件的完整程式碼如下

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
        
        const resp = await fetch('/adobe/forms/af/dor/L2NvbnRlbnQvZm9ybXMvYWYvcmlzaGk=');
        let formJSON = await resp.json();
        setForm(formJSON.afModelDefinition)
      
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

上述程式碼使用原生html元件，這些元件對應至最適化表單中使用的元件。 例如，我們將文字輸入最適化表單元件對應至TextField元件。 文章[中使用的原生元件可從這裡](./assets/native-components.zip)下載

## 後續步驟

[從下拉式清單中選取表單](./select-form-from-drop-down-list.md)
