---
title: AEM Headless中的受保護內容
description: 瞭解如何保護AEM Headless中的內容。
version: Experience Manager as a Cloud Service
topic: Headless
feature: GraphQL API
role: Developer, Architect
level: Intermediate
jira: KT-15233
last-substantial-update: 2024-05-01T00:00:00Z
exl-id: c4b093d4-39b8-4f0b-b759-ecfbb6e9e54f
duration: 254
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1151'
ht-degree: 0%

---

# 在AEM Headless中保護內容

從AEM Publish提供AEM Headless內容時，確保資料的完整性和安全性是提供敏感內容時的重要關鍵。 本操作說明將逐步解說如何保護AEM Headless GraphQL API端點提供的內容。

本教學課程中的指引，說明對內容有嚴格要求，必須專門提供給特定使用者或使用者群組使用。 必須區分個人化行銷內容與私人內容，例如PII或個人財務資料，以避免混淆和意外結果。 本教學課程說明如何保護私人內容。

討論行銷內容時，我們指的是針對個別使用者或群組量身打造的內容，並非供一般使用者使用。 不過，您必須瞭解，雖然此內容是針對特定使用者，但在預期內容之外公開（例如透過操控HTTP請求）不會帶來安全性、法律或聲譽風險。

需要強調的是，本文中提及的所有內容皆假設為私人內容，且只能由指定的使用者或群組檢視。 行銷內容通常不需要保護，而是應用程式可以管理其傳送給特定使用者的內容，並快取以提升效能。

本操作說明不涵蓋：

- 直接保護端點，而是專注於保護其傳送的內容。
- AEM發佈或取得登入權杖的驗證。 驗證方法和認證傳遞取決於個別使用案例和實施。

## 使用者群組

首先，我們必須定義[使用者群組](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/cloud-service/accessing/aem-users-groups-and-permissions)，其中包含應該有權存取受保護內容的使用者。

![AEM Headless受保護的內容使用者群組](./assets/protected-content/user-groups.png){align="center"}

使用者群組會指派對AEM Headless內容的存取權，包括內容片段或其他參考資產。

1. 以&#x200B;**使用者管理員**&#x200B;的身分登入AEM Author。
1. 瀏覽至&#x200B;**工具** > **安全性** > **群組**。
1. 選取右上角的&#x200B;**建立**。
1. 在&#x200B;**詳細資料**&#x200B;索引標籤中，指定&#x200B;**群組識別碼**&#x200B;和&#x200B;**群組名稱**。
   - 群組ID和群組名稱可以是任何內容，但在此範例中使用名稱&#x200B;**AEM Headless API使用者**。
1. 選取「**儲存並關閉**」。
1. 選取新建立的群組，然後從動作列選擇&#x200B;**啟動**。

如果需要各種存取層級，請建立多個可與不同內容相關聯的使用者群組。

### 將使用者新增至使用者群組

若要授予AEM Headless GraphQL API請求對受保護內容的存取權，您可以將Headless請求與屬於特定使用者群組的使用者建立關聯。 以下是兩種常見方法：

1. **AEM as a Cloud Service [技術帳戶](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials)：**
   - 在AEM as a Cloud Service Developer Console中建立技術帳戶。
   - 使用技術帳戶登入AEM Author一次。
   - 透過&#x200B;**工具>安全性>群組> AEM Headless API使用者>成員**，將技術帳戶新增到使用者群組。
   - 在AEM發佈上&#x200B;**同時啟動技術帳戶使用者和使用者群組**。
   - 此方法要求Headless使用者端不要將服務認證公開給使用者，因為這些認證是特定使用者的認證，不應該共用。

   ![AEM技術帳戶群組管理](./assets/protected-content/group-membership.png){align="center"}

2. **已命名的使用者：**
   - 驗證已命名的使用者，並直接將其新增到AEM發佈上的使用者群組。
   - 此方法需要Headless使用者端使用AEM Publish驗證使用者憑證、取得AEM登入或存取Token，並將此Token用於向AEM提出的後續請求。 本作法不說明如何達成此目標的詳細資訊，且須視實施而定。

## 保護內容片段

保護內容片段是保護AEM Headless內容的必要措施，可透過將內容與封閉式使用者群組(CUG)建立關聯來達成。 使用者向AEM Headless GraphQL API提出請求時，系統會根據使用者的CUG篩選傳回的內容。

![AEM Headless CUG](./assets/protected-content/cugs.png){align="center"}

請依照下列步驟，透過[封閉式使用者群組(CUG)](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/assets/advanced/closed-user-groups)達成此目的。

1. 以&#x200B;**DAM使用者**&#x200B;身分登入AEM Author。
2. 導覽至&#x200B;**Assets >檔案**，然後選取包含要保護的內容片段的&#x200B;**資料夾**。 除非被不同的CUG覆寫，否則CUG會以階層方式套用並影響子資料夾。
   - 確保此使用者群組中包含屬於其他管道（使用資料夾內容）的使用者。 或者，也可以將與這些管道相關聯的使用者群組包含在CUG清單中。 否則，這些頻道將無法存取內容。
3. 選取資料夾，然後從工具列選擇&#x200B;**屬性**。
4. 選取「**許可權**」標籤。
5. 輸入&#x200B;**群組名稱**&#x200B;並選取&#x200B;**新增**&#x200B;按鈕以新增新的CUG。
6. **儲存**&#x200B;以套用CUG。
7. **選取**&#x200B;資產資料夾並選取&#x200B;**發佈**，將套用CUG的資料夾傳送至AEM Publish，並評估為許可權。

對包含需要保護的內容片段的所有資料夾執行這些相同步驟，將正確的CUG套用至每個資料夾。

現在，向AEM Headless GraphQL API端點發出HTTP請求時，結果中只會包含請求使用者的指定CUG可存取的內容片段。 如果使用者缺少任何內容片段的存取權，結果將為空白，儘管仍會傳回200 HTTP狀態代碼。

### 保護引用的內容

內容片段經常會參考其他AEM內容，例如影像。 若要保護此參考內容的安全，請將CUG套用至儲存參考資產的資產資料夾。 請注意，參考的資產通常會使用與AEM Headless GraphQL API不同的方法來請求。 因此，在要求傳遞存取權杖給這些參考資產的方式可能會有所不同。

根據內容架構，可能有必要將CUG套用至多個資料夾，以確保所有參考內容都受到保護。

## 防止快取受保護內容

AEM as a Cloud Service [依預設快取HTTP回應](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/cloud-service/caching/publish)以提升效能。 但是，這可能會導致提供受保護內容時發生問題。 若要防止快取這類內容，請在AEM Publish執行個體的Apache設定中，[移除特定端點的快取標頭](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/cloud-service/caching/publish#how-to-customize-cache-rules-1)。

將以下規則新增到您的Dispatcher專案的Apache設定檔案中，以移除特定端點的快取標頭：

```xml
# dispatcher/src/conf.d/available_vhosts/example.vhost

<VirtualHost *:80>
    ...
    # Replace `example` with the name of your GraphQL endpoint's configuration name.
    <LocationMatch "^/graphql/execute.json/example/.*$">
        # Remove cache headers for protected endpoints so they are not cached
        Header unset Cache-Control
        Header unset Surrogate-Control
        Header set Age 0
    </LocationMatch>
    ...
</VirtualHost>
```

請注意，這將導致效能損失，因為Dispatcher或CDN不會快取內容。 這是效能和安全性之間的權衡。

## 保護AEM Headless GraphQL API端點

本指南並未說明如何保護[AEM Headless GraphQL API端點](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/headless/graphql-api/graphql-endpoint)本身，而是著重於保護其所提供的內容。 所有使用者（包括匿名使用者）都可存取包含受保護內容的端點。 系統只會傳回使用者的「已關閉的使用者群組」可存取的內容。 如果沒有可存取的內容，AEM Headless API回應仍會有200 HTTP回應狀態代碼，但結果會是空的。 一般而言，保護內容安全便已足夠，因為端點本身不會揭露敏感資料。 如果您需要保護端點，請透過[Sling存放庫初始化(repoinit)指令碼](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios)在AEM Publish上將ACL套用至這些端點。
