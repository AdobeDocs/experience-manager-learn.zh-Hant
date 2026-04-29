---
title: AI輔助開發
description: 瞭解AI輔助開發，其使用AI支援的IDE或編碼代理程式，以及AGENTS.md、Agent Skills和MCP伺服器，以幫助為AEM as a Cloud Service上的專案產生高品質、可用於生產的程式碼。
version: Experience Manager as a Cloud Service
feature: Developer Tools
role: Developer, Architect
level: Beginner
doc-type: Article
duration: 0
last-substantial-update: 2026-04-24T00:00:00Z
jira: KT-20899
thumbnail: KT-20899.pngKT-20899
source-git-commit: e3ef450cfe9005ba940ff1897c216681654341b3
workflow-type: tm+mt
source-wordcount: '906'
ht-degree: 0%

---


# AI輔助開發

AI輔助開發使用AI支援的IDE或編碼代理程式，以及`AGENTS.md`、代理程式技能和MCP伺服器，來協助為AEM as a Cloud Service專案產生高品質、生產就緒的程式碼。

Visual Studio Code[&#128279;](https://code.visualstudio.com/docs/copilot/overview)、[Claude Code](https://code.claude.com/docs/en/overview)中的工具，例如[Cursor](https://www.cursor.com/)、GitHub Copilot，以及類似的AI支援的IDE和編碼代理程式，有幾個主要方法可協助：

- **更快的反複專案**：從描述所需功能或變更的自然語言提示產生或重構程式碼。
- **學習輔助**：在提示時說明不熟悉的程式碼路徑、設定、概念或最佳實務。

不過，這些優點在很大程度上取決於編碼代理程式&#x200B;_可用的_&#x200B;內容。 一般訓練資料和單一存放庫快照集通常&#x200B;_不足_，無法可靠地產生可用於生產環境的AEM程式碼。

## 為什麼只有AI是不夠的

沒有正確的上下文，AI模型（透過AI支援的IDE或編碼代理程式）可以：

- **幻覺API或生命週期**：建議不符合AEM as a Cloud Service最佳實務或最新功能的程式碼或設定。
- **遺漏程式步驟**：省略程式碼存放庫或訓練資料中不可見的必要步驟。
- **偏離專案標準**：忽略元件、OSGi服務、工作流程或Dispatcher設定的已建立模式。

此間隙是讓&#x200B;_結構化內容_ (Agent Skills and AGENTS.md)和&#x200B;_執行階段可見度_ （MCP伺服器）成為讓AI輔助開發&#x200B;_生產力_&#x200B;和&#x200B;_可靠_&#x200B;的必要條件。

## Adobe如何協助AI輔助開發

Adobe針對AEM as a Cloud Service專案提供：

- Agent Skills and AGENTS.md via [AI編碼代理程式的Adobe Skills](https://github.com/adobe/skills)
- 透過[軟體發佈](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=mcp*&1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&1_group.propertyvalues.operation=equals&1_group.propertyvalues.0_values=software-type%3Atooling&orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&orderby.sort=desc&layout=list&p.offset=0&p.limit=3)入口網站，為AEM SDK和本機Dispatcher提供本機MCP伺服器
- Adobe代管的AEM MCP伺服器，用於您的IDE或聊天應用程式中的內容和Cloud Manager工作流程 — 請參閱AEM中的[MCP伺服器](../mcp/overview.md)

以下各節會概述每個專案。 使用本頁結尾的&#x200B;**安裝程式**&#x200B;和&#x200B;**使用案例**&#x200B;區段，進行AI輔助開發的安裝和逐步解說。

## 代理程式技能為何

代理程式技能是&#x200B;_程式知識或專業知識_，可協助編碼代理程式&#x200B;_可靠地執行實際工作_。 如需詳細資訊，請參閱[代理程式技能](https://agentskills.io)。

若為AEM as a Cloud Service專案，[AI編碼代理程式的Adobe技能](https://github.com/adobe/skills)存放庫中會提供代理程式技能。

## 什麼是AGENTS.md

AGENTS.md提供&#x200B;_內容與指示_，以協助編碼代理程式&#x200B;_處理您的專案_。 如需詳細資訊，請參閱[AGENTS.md](https://agents.md/)。

若為AEM as a Cloud Service專案，當遺失&#x200B;**時，`ensure-agents-md`啟動程式技能會在存放庫根目錄**&#x200B;建立&#x200B;**AGENTS.md**。 此技能會檢查您的專案（例如，根`pom.xml`和模組），並產生量身打造的指引，而不是使用靜態檔案。 如果&#x200B;**AGENTS.md**&#x200B;已經存在，則&#x200B;**不會**&#x200B;覆寫。

檔案存在後，您可以編輯它以新增更多內容與指示，供您的團隊或組織的最佳實務使用。 技能也可以建立參照&#x200B;**AGENTS.md**&#x200B;的&#x200B;**CLAUDE.md**，以便Claude型工具取得相同的指引。

## MCP伺服器是什麼？

MCP伺服器透過[模型內容通訊協定](https://modelcontextprotocol.io/)將工具和資料公開給編碼代理程式，該通訊協定支援偵錯、檢查、執行和驗證變更等動作。 MCP伺服器可在您的工作站（**本機**）上執行，或做為裝載服務（**遠端**）。

針對AEM SDK和Dispatcher進行&#x200B;**本機開發**，請從[軟體發佈](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=mcp*&1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&1_group.propertyvalues.operation=equals&1_group.propertyvalues.0_values=software-type%3Atooling&orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&orderby.sort=desc&layout=list&p.offset=0&p.limit=3)入口網站安裝這&#x200B;**部本機MCP伺服器**：

- **AEM Quickstart本機MCP伺服器**：公開本機AEM SDK執行個體的即時執行階段資料，以支援疑難排解和開發。 如需詳細資訊，請參閱[AEM Quickstart MCP Server](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/local-development-with-ai-tools#aem-quickstart-mcp-server)。
- **Dispatcher本機MCP伺服器**：啟用本機Dispatcher執行個體的執行階段驗證和檢查。 如需詳細資訊，請參閱[Dispatcher MCP伺服器](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/local-development-with-ai-tools#dispatcher-mcp-server)。

若為Adobe託管的AEM MCP伺服器（例如內容、唯讀內容和Cloud Manager），請參閱AEM中的[MCP伺服器](../mcp/overview.md)。

## 設定

<!-- 
CARDS
{target = _self}

* ./setup/agent-skills.md
    {title = Set up AEM Agent Skills}
    {description = Learn how to set up AEM Agent Skills for AI-assisted development.}
    {image = ./assets/agent-skills/select-aem-agent-skills-to-install.png}
    {cta = Install AEM Agent Skills}

-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Set up AEM Agent Skills">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./setup/agent-skills.md" title="設定AEM Agent技能" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/agent-skills/select-aem-agent-skills-to-install.png" alt="設定AEM Agent技能"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./setup/agent-skills.md" target="_self" rel="referrer" title="設定AEM Agent技能">設定AEM代理程式技能</a>
                    </p>
                    <p class="is-size-6">瞭解如何設定AEM Agent技能以進行AI輔助開發。</p>
                </div>
                <a href="./setup/agent-skills.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">安裝AEM代理程式技能</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## 使用案例

<!-- 
CARDS
{target = _self}

* ./use-cases/component-development.md    
    {title = Create AEM Component with AI-assisted development}
    {description = Learn how to use AI-assisted development to develop AEM components.}
    {image = ./assets/component-development/review-generated-code.png}
    {cta = Create AEM Component}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Create AEM Component with AI-assisted development">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/component-development.md" title="使用AI輔助開發建立AEM元件" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/component-development/review-generated-code.png" alt="使用AI輔助開發建立AEM元件"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/component-development.md" target="_self" rel="referrer" title="使用AI輔助開發建立AEM元件">使用AI輔助開發建立AEM元件</a>
                    </p>
                    <p class="is-size-6">瞭解如何使用AI輔助開發來開發AEM元件。</p>
                </div>
                <a href="./use-cases/component-development.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">建立AEM元件</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## 其他資源

- [使用AI工具進行本機開發](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/local-development-with-ai-tools)

- [AI編碼代理程式的Adobe技能](https://github.com/adobe/skills)

- [AGENTS.md](https://agents.md/)

- [代理程式技能](https://agentskills.io/home)