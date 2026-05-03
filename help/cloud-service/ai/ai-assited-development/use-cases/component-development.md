---
title: 使用AEM Agent技能的元件開發
description: 瞭解如何使用AEM代理程式技能來開發AEM元件，作為AI輔助開發的一部分。
version: Experience Manager as a Cloud Service
feature: Developer Tools
role: Developer, Architect
level: Beginner
doc-type: Article
duration: 0
last-substantial-update: 2026-04-24T00:00:00Z
jira: KT-20901
thumbnail: KT-20901.png
exl-id: bd9b74e8-81ab-4d42-bd0a-5443248b5770
source-git-commit: f93359e731b6c3fa549e9499ef693042eba3aad7
workflow-type: tm+mt
source-wordcount: '632'
ht-degree: 2%

---

# 使用AEM Agent技能的元件開發

瞭解如何使用AEM代理程式技能來開發AEM元件，作為[AI輔助開發](../overview.md)的一部分。

在此逐步解說中，您在AI支援的IDE （例如Cursor）中使用自然語言，在[WKND Sites專案](https://github.com/adobe/aem-guides-wknd)中開發&#x200B;**促銷橫幅**&#x200B;元件。 編碼代理程式會套用`create-component` AEM代理程式技能以產生實作。

>[!VIDEO](https://video.tv.adobe.com/v/3484952/?learn=on&enablevpops)

## 先決條件

若要依照本教學課程操作，您需要具備以下條件：

- AI支援的IDE，例如Cursor或具有GitHub Copilot的Visual Studio Code。
- [WKND Sites專案](https://github.com/adobe/aem-guides-wknd)的本機復本，已建置並部署至&#x200B;_本機AEM SDK_&#x200B;執行個體。
- 在該專案中安裝&#x200B;_AEM代理程式技能_。 如果您尚未完成，請完成[設定AEM代理程式技能](../setup/agent-skills.md)。

## 元件需求

假設WKND團隊想要在首頁上顯示促銷橫幅，設計參考如下：

![促銷橫幅設計參考](../assets/component-development/promo-banner-design-reference.png)

作者必須在元件對話方塊中設定&#x200B;_促銷標籤_、_CTA標籤_&#x200B;及&#x200B;_CTA連結_&#x200B;欄位。

設計參考是透過線框、模型或靜態標籤擷取取得的熒幕擷圖。

## 開發元件

1. 在IDE中開啟WKND專案。 確認AEM代理程式技能存在（例如，`.agents/skills`底下），然後開始新的代理程式聊天。
   ![確認已安裝AEM代理程式技能](../assets/component-development/verify-aem-agent-skills-installed.png)

1. 輸入類似以下的提示。 如果IDE支援聊天中的影像，請附加元件設計熒幕擷取畫面（透過線框、模型或靜態標籤擷取取得）：

   ```text
   Create a WKND Promo Banner Component. Please see attached screenshot for design reference.
   
   Dialog specification are:
   
   1. Promo Label - Textfield, required
   2. CTA Text - Textfield, required
   3. CTA Link - Pathfield, required
   ```

1. 編碼代理程式使用`create-component` AEM代理程式技能來產生元件。 檢閱建議的HTL、Sling模型、對話方塊XML和相關檔案。
   ![檢閱產生的程式碼](../assets/component-development/review-generated-code.png)

>[!TIP]
>
>您也可以透過[Figma MCP伺服器](https://www.figma.com/mcp-catalog/)提供Figma設計來產生元件，而不提供設計參考作為熒幕擷圖。 `create-component`技能支援[Figma設計整合](https://github.com/adobe/skills/blob/main/plugins/aem/cloud-service/skills/create-component/references/figma-design-rules.md)


1. 將元件部署至本機AEM執行個體/SDK。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. 在製作時，請將「促銷橫幅」放置在首頁上，並驗證行為。 如果實作仍偏離設計參考，請調整實作。
   ![編寫促銷橫幅元件](../assets/component-development/author-promo-banner-component.png)

1. 透過發佈頁面或檢視為已發佈來檢閱新建立的元件。
   ![檢閱新建立的元件](../assets/component-development/review-newly-created-component.png)

恭喜！ 您已成功使用AEM代理程式技能建立新的AEM元件，作為AI輔助開發的一部分。

## 超越簡單元件

此逐步解說使用簡單元件。 相同的`create-component`技能也支援更豐富的案例，包括：

- 多欄位和巢狀對話方塊欄位
- AEM核心元件擴充功能（包括Sling資源合併模式）
- 在IDE中啟用Figma MCP伺服器（例如`plugin-figma-figma`）時，配置與樣式的Figma檔案或框架URL

對於欄位型別、對話方塊模式、Figma規則和範例，請讀取已安裝技能資料夾中的`SKILL.md`，例如`.agents/skills/create-component/SKILL.md`。

如需概觀、依IDE提供的安裝路徑以及疑難排解，請參閱Adobe技能存放庫中的[AEM元件開發代理程式](https://github.com/adobe/skills/blob/main/plugins/aem/cloud-service/skills/create-component/README.md)。

## AGENTS.md

總結之前，請先瞭解建立元件時如何產生AGENTS.md。

對於AEM as a Cloud Service專案，`ensure-agents-md`啟動程式技能（在[設定AEM代理程式技能](../setup/agent-skills.md)期間選取）會在存放庫根目錄中建立`AGENTS.md`，但此時有&#x200B;**遺失**。 它會使用從您的專案版面中學習到的內容。

它&#x200B;**不會**&#x200B;覆寫現有的`AGENTS.md`檔案。

![AGENTS.md建立](../assets/component-development/agents-md-creation.png)

## 其他資源

- [使用AI工具進行本機開發](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/local-development-with-ai-tools)

- [AI編碼代理程式的Adobe技能](https://github.com/adobe/skills)

- [AGENTS.md](https://agents.md/)

- [代理程式技能](https://agentskills.io/home)
