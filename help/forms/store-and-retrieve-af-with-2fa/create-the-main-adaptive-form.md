---
title: 建立主自適應窗體
description: 建立用於捕獲申請人資訊的自適應表單和用於檢索已保存的自適應表單的自適應表單
feature: Adaptive Forms
type: Tutorial
activity: implement
version: 6.4,6.5
kt: 6552
thumbnail: 6552.jpg
topic: Development
role: User
level: Beginner
exl-id: 73de0ac4-ada6-4b8e-90a8-33b976032135
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 0%

---

# 建立主自適應窗體

窗體 **儲存AFWithAttachments** 是主要的自適應形式。 此自適應窗體是使用案例的入口點。 在此表單中，將捕獲包括移動號碼在內的用戶詳細資訊。 此表單還能夠添加一些附件。 當按一下「保存並退出」按鈕時，執行伺服器端代碼以將表單資料儲存在資料庫中，並生成唯一的應用程式ID並呈現給用戶以進行安全保存。 此應用程式ID用於檢索與應用程式關聯的移動號碼。

![主要申請表](assets/6552.JPG)

此窗體與 **bootboxjs540,storeAFWithAttachments** 在課程早期建立的客戶端庫和AEM在提交表單時觸發的工作流。


* 示例表格基於 [自定義自適應表單模板](assets/custom-template-with-page-component.zip) 要正確呈現樣AEM式，需要導入到。

* 已完成 [StoreAfWithAttachments窗體](assets/store-af-with-attachments-form.zip) 可以下載並導入到實AEM例中。

* 的 [與此AEM表單關聯的工作流](assets/workflow-model-store-af-with-attachments.zip) 需要導入到您AEM的實例中才能使用表單。


## 後續步驟

[建立檢索已保存表單的表單](./retrieve-saved-form.md)