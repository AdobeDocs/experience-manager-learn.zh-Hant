---
title: 建立地址元件
description: 在AEM FormsCloud Service中建立新的位址核心元件
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15752
source-git-commit: a8fc8fa19ae19e27b07fa81fc931eca51cb982a1
workflow-type: tm+mt
source-wordcount: '272'
ht-degree: 1%

---


# 建立地址元件

登入本機雲端就緒的AEM Forms執行個體的CRXDE。

複製 ``/apps/bankingapplication/components/adaptiveForm/button`` 節點並將其重新命名為addressblock。 選取addressblock節點並設定其屬性，如下所示。

>[!NOTE]
>
> ``bankingapplication`` 是在建立Maven專案時提供的appId。 您的環境中此appId可能不同。 您可以製作任何元件的副本，我剛才正好製作按鈕元件的副本


![address-bloc](assets/address-properties.png)

## cq-template節點屬性

選取 ``cq-template`` 下的節點 ``addressblock`` 節點並設定其屬性，如下所示。 請注意，fieldType已設定為panel
![cq-template](assets/cq-template.png)

## 在cq-template下新增節點

新增下列型別的節點 ``nt:unstructured`` 在 ``cq-template``

* streetaddress
* 城市
* ZIP
* 州別

這些節點代表位址區塊元件的欄位。 街道地址、城市和郵遞區號欄位將是文字輸入欄位，而州別欄位將是下拉式欄位。

## 設定streetaddress節點的屬性

>[!NOTE]
>
> 此 **_銀行應用程式_** 在路徑中是指maven專案的appId。 您的環境可能會有所不同

選取 ``streetaddress`` 節點並設定其屬性，如下所示。
![street-address](assets/streetaddress.png)

## 設定城市節點的屬性

選取 ``city`` 節點並設定其屬性，如下所示。
![城市](assets/city.png)

## 設定zip節點的屬性

選取 ``zip`` 節點並設定其屬性，如下所示。
![zip](assets/zip.png)

## 設定狀態節點的屬性

選取 ``state`` 節點並設定其屬性，如下所示。 請注意fieldType的狀態型別 — 已設定為下拉式清單
![state](assets/state.png)

最終的addressblock元件看起來像這樣

![final-address](assets/crx-address-block.png)

## 後續步驟

[部署專案](./deploy-your-project.md)




