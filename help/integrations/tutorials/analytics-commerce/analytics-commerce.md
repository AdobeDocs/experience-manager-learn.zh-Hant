---
title: 將Analytics與Commerce教學課程整合
description: 瞭解如何將Analytics與Commerce整合。
solution: Analytics, Commerce
feature: Integrations
topic: Integrations
role: Leader, Architect, Admin, Developer
level: Beginner
index: true
hidefromtoc: true
kt: null
thumbnail: null
last-substantial-update: 2023-04-11T00:00:00Z
badgeIntegration: label="整合" type="positive"
source-git-commit: ed53392381fa568de8230288e6b85c87540222cf
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 0%

---


# 將Analytics與商務整合

* 確認帳戶有權存取Adobe Analytics。

* 在Adobe Analytics中建立專案。

* 建立結構描述。
   * 您需要此項才能在後續步驟中選取選項。 若要建立方案，請檢視「資料管理」下方的左側欄，然後尋找方案。 現在在左上方，按一下「建立結構描述」。 選取XDM ExperienceEvent。
   * 在左側尋找欄位群組，按一下新增
      * 在搜尋中，您可以輸入以下內容進行篩選 `ExperienceEvent Commerce`
      * 尋找 `Adobe Analytics ExperienceEvent Commerce` 並勾選方塊
      * 請務必按一下 `Add field groups` 以儲存並繼續
* 建立資料集，下次設定「DataStream」時需要它。
   * 資料集位於左側欄「資料管理」下，尋找「資料集」。
   * 然後按一下右上方的「建立資料集」。 您可從結構描述建立資料集。
   * 搜尋並使用您先前建立的結構描述
* 建立資料串流。 您可以使用「左側欄中的資料收集」並尋找「資料串流」來存取它。
* 建立具有面板和區段的表格。 這在本教學課程中太複雜了，您需要經驗豐富的Analytics人員來協助。


最後，若要檢視您的報表，請導覽至experience.adobe.com ，尋找您的工作區專案、按一下您要檢視專案的連結，然後您應該會看到類似此影像的內容

![部分商務資料的Analytics熒幕擷圖](./assets/analytics-commerce/analytics-screenshot-commerce-items.png)