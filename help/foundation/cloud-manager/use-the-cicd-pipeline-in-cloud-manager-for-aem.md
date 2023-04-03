---
title: 在Cloud Manager中使用CI/CD管道Adobe
description: AdobeCloud Manager提供簡單而有彈性的自助服務CI/CD管道，讓AEM專案團隊能快速、安全且一致地將程式碼部署至AMS中托管的所有AEM環境。 本影片系列探討如何在失敗和成功案例中設定和執行Cloud Manager的CI/CD管道。
sub-product: Experience Manager Cloud Manager, Experience Manager
topics: cicd, performance, best-practices, development, governance
doc-type: feature video
activity: understand
audience: all
topic: Architecture
role: Developer
level: Beginner
exl-id: d5d59ef5-9343-4ac2-9053-a010decdb9b6
last-substantial-update: 2022-08-15T00:00:00Z
thumbnail: cm-pipeline.jpg
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '312'
ht-degree: 2%

---

# 在Cloud Manager中使用CI/CD管道Adobe

AdobeCloud Manager提供簡單而有彈性的自助服務CI/CD管道，讓AEM專案團隊能快速、安全且一致地將程式碼部署至AMS中托管的所有AEM環境。 本影片系列探討如何在失敗和成功案例中設定和執行Cloud Manager的CI/CD管道。

## 簡介

簡介Cloud Manager和Cloud Manager計畫。

>[!NOTE]
>
>在這些影片中，建置、測試和部署時間皆已加快，以縮短影片的時間。 完成管道執行通常需要45分鐘或更久的時間（包括強制性的30分鐘效能測試），具體取決於專案大小、AEM例項數和UAT程式數。

>[!VIDEO](https://video.tv.adobe.com/v/23082?quality=12&learn=on)

## 設定CI/CD管道

本影片探討如何在Cloud Manager中為程式設定管道。

>[!VIDEO](https://video.tv.adobe.com/v/23083?quality=12&learn=on)

## 管道執行失敗

此影片會探索CI/CD管道的執行方式，使用的程式碼會使Cloud Manager的必要品質檢查失敗， **[!DNL yellow]** 存放庫分支。

>[!VIDEO](https://video.tv.adobe.com/v/23084?quality=12&learn=on)

## 成功執行管道

此影片會探索如何使用程式碼，以透過Cloud Manager的必要品質檢查，成功執行CI/CD管道(使用 **[!DNL master]** 存放庫分支。

此影片也觸及 [!UICONTROL 活動] Cloud Manager中的主控台，可允許重新進入使用中的執行，或檢閱已完成或失敗的執行。

>[!VIDEO](https://video.tv.adobe.com/v/23085?quality=12&learn=on)

## 支援材料

* [Cloud Manager使用手冊](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/introduction.html)
* [下載程式碼掃描 [!DNL SonarQube] 規則](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/code-quality-testing.html)
   * *XLSX可在連結區段的底部取得*
* [[!DNL SonarQube] Java™規則索引](https://rules.sonarsource.com/java/)
