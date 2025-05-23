---
title: 在AEM中建立Adobe Target Cloud Service帳戶
description: 使用Cloud Service和Adobe IMS驗證整合Adobe Experience Manager as a Cloud Service與Adobe Target。
jira: KT-6044
thumbnail: 41244.jpg
topic: Integrations
feature: Integrations
role: Admin
level: Intermediate
badgeIntegration: label="整合" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service， AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: dd6c17ae-8e08-4db3-95f9-081cc7dbd86e
duration: 316
source-git-commit: 22bd6237bb1665bf87c6302d2988135b505e40c0
workflow-type: tm+mt
source-wordcount: '191'
ht-degree: 0%

---

# 建立Adobe Target Cloud Service帳戶 {#adobe-target-cloud-service}

以下影片會逐步說明如何將AEM as a Cloud Service與Adobe Target連線。

此整合可讓AEM Author服務直接與Adobe Target通訊，並將體驗片段從AEM推送至Target做為選件。  這項整合&#x200B;*不會*&#x200B;將Adobe Target JavaScript (AT.js)新增至AEM Sites網頁，因為使用Target擴充功能[&#128279;](../experience-platform/data-collection/tags/connect-aem-tag-property-using-ims.md)整合AEM和標籤。

>[!WARNING]
>
>影片顯示已遭取代的JWT驗證方法，用於將AEM連線至Adobe Target。 不過，建議使用OAuth伺服器對伺服器驗證方法。 如需詳細資訊，請參閱[AEM的JWT-To-OAuth認證移轉](https://experienceleague.adobe.com/en/docs/experience-manager-learn/foundation/authentication/jwt-to-oauth-migration.html)。 我們正在更新視訊以反映這項變更。


>[!VIDEO](https://video.tv.adobe.com/v/41244?quality=12&learn=on)

>[!CAUTION]
>
>影片中顯示的Adobe Target雲端服務設定有已知問題。 在此問題解決之前，請依照影片中的相同步驟操作，但使用[舊版Adobe Target雲端服務設定](https://experienceleague.adobe.com/docs/experience-manager-learn/aem-target-tutorial/aem-target-implementation/using-aem-cloud-services.html)。
