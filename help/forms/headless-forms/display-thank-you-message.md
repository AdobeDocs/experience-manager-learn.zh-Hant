---
title: 在表單提交時顯示感謝訊息
description: 使用onSubmitSuccess處理常式，在react應用程式中顯示已設定的感謝訊息
feature: Adaptive Forms
version: 6.5
kt: 13490
topic: Development
role: User
level: Intermediate
source-git-commit: 10ff0d87991d7766d5ca9563062a2f7be6035e43
workflow-type: tm+mt
source-wordcount: '172'
ht-degree: 0%

---

# 顯示已設定的感謝訊息

在提交表單時顯示感謝訊息，是對使用者完成和提交表單表示認可和感謝的深思熟慮的方式。 這可確認其提交內容已收到並受到讚賞。 感謝訊息是使用最適化表單的指南容器的提交索引標籤來設定

![感謝訊息](assets/thank-you-message.png)

可在AdaptiveForm超級元件的onSuccess事件處理常式中存取已設定的感謝訊息。
關聯onSuccess事件的程式碼和onSuccess事件處理常式的程式碼如下所列

```javascript
<AdaptiveForm mappings={extendMappings} onSubmitSuccess={onSuccess} formJson={selectedForm}/>
```

```javascript
const onSuccess=(action) =>{
        let body = action.payload?.body;
        debugger;
        setThankYouMessage(body.thankYouMessage.replace(/<(.|\n)*?>/g, ''));
        console.log("Thank you message "+body.thankYouMessage.replace(/<(.|\n)*?>/g, ''));

      }
```

Contact函式元件的完整程式碼如下

```javascript
import Form from './components/Form';
import PlainText from './components/plainText';
import TextField from './components/TextField';
import Button from './components/Button';
import { useState,useEffect } from "react";
import { AdaptiveForm } from "@aemforms/af-react-renderer";
export default function Contact(){
  
    const [selectedForm, setForm] = useState("");
    const [thankYouMessage, setThankYouMessage] = useState("");
    const [formSubmitted, setFormSubmitted] = useState(false);
  
    const extendMappings = {
        'plain-text' : PlainText,
        'text-input' : TextField,
        'button' : Button,
        'form': Form
      };
     const onSuccess=(action) =>{
        let body = action.payload?.body;
        debugger;
        setFormSubmitted(true);
        setThankYouMessage(body.thankYouMessage.replace(/<(.|\n)*?>/g, ''));
        // Remove any html tags in the thank you message
        console.log("Thank you message "+body.thankYouMessage.replace(/<(.|\n)*?>/g, ''));

      }
      
      const getForm = async () => {
        
        const resp = await fetch('/adobe/forms/af/L2NvbnRlbnQvZm9ybXMvYWYvY29udGFjdHVz');
        // Get the form id manually using the listform api
        let formJSON = await resp.json();
        setForm(formJSON.afModelDefinition)
      }
      useEffect( ()=>{
        getForm()
        

    },[]);
    
    return(
        
        <div>
           {!formSubmitted ?
            (
                <div>
                    <h1>Get in touch with us!!!!</h1>
                    <AdaptiveForm mappings={extendMappings} onSubmitSuccess={onSuccess} formJson={selectedForm}/>
                </div>
            ) :
            (
                <div>
                    <div>{thankYouMessage}</div>
                </div>
            )}
        </div>
      
          
        
    )
}
```

上述程式碼使用原生html元件，這些元件對應至最適化表單中使用的元件。 例如，我們將文字輸入最適化表單元件對應至TextField元件。 文章中使用的原生元件 [可從此處下載](./assets/native-components.zip)

