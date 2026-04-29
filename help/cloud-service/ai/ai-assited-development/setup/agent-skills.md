---
title: 設定AEM Agent技能
description: 瞭解如何設定AEM Agent技能以進行AI輔助開發。
feature: Developer Tools
version: Experience Manager as a Cloud Service
role: Developer
level: Beginner
doc-type: Tutorial
duration: 0
last-substantial-update: 2026-04-24T00:00:00Z
jira: KT-20900
thumbnail: KT-20900.png
source-git-commit: e3ef450cfe9005ba940ff1897c216681654341b3
workflow-type: tm+mt
source-wordcount: '513'
ht-degree: 4%

---


# 設定AEM Agent技能

瞭解如何設定AEM Agent技能以進行AI輔助開發。

當您透過AI支援的IDE要求編碼代理程式處理AEM開發任務時，它可以使用Adobe提供的&#x200B;**AEM代理程式技能**&#x200B;程式指南，而不只依賴一般模型訓練或僅從存放庫推斷的任何內容。

Adobe透過[AEM技能](https://github.com/adobe/skills)存放庫提供Adobe代理程式技能。 另請參閱[AI輔助開發](../overview.md)，瞭解Adobe如何協助AI輔助開發。

在本教學課程中，您會在[WKND Sites專案](https://github.com/adobe/aem-guides-wknd)的本機複製本上安裝技能。 您可以對自己的AEM as a Cloud Service專案使用相同的步驟。

## 先決條件

若要依照本教學課程操作，您需要具備以下條件：

- [WKND Sites專案](https://github.com/adobe/aem-guides-wknd)或您自己的AEM as a Cloud Service專案的本機複製。
- AI支援的IDE，例如Cursor或具有GitHub Copilot的Visual Studio Code。

## 安裝AEM Agent技能

使用`npx`命令安裝AEM代理程式技能（需要[Node.js](https://nodejs.org/)，因此`npx`可用）。 如需其他安裝選項，例如Claude Code外掛程式或GitHub CLI擴充功能，請參閱Adobe技能存放庫中的[安裝](https://github.com/adobe/skills/tree/main#installation)區段。

1. 在本機複製[WKND Sites專案](https://github.com/adobe/aem-guides-wknd)：

   ```shell
   $ git clone https://github.com/adobe/aem-guides-wknd.git
   ```

1. 在AI支援的IDE中開啟複製的專案（例如Cursor），然後開啟整合式終端機。
   ![開啟終端機](../assets/agent-skills/wknd-in-cursor-ide-open-terminal.png)

1. 執行以下命令，為游標新增AEM代理程式技能：

   ```shell
   $ npx skills add https://github.com/adobe/skills/tree/main/plugins/aem/cloud-service --agent cursor
   ```

   如需其他代理程式型別，請參閱Adobe技能存放庫中的[安裝](https://github.com/adobe/skills/tree/main#installation)區段。

1. 出現提示時，選擇要安裝的AEM Agent技能。
   ![選取要安裝的AEM代理程式技能](../assets/agent-skills/select-aem-agent-skills-to-install.png)

   選取&#x200B;**confirm-agents-md**&#x200B;技能，以便安裝程式可以在存放庫根目錄中建立&#x200B;**AGENTS.md**&#x200B;和&#x200B;**CLAUDE.md**&#x200B;檔案。 該啟動程式技能會檢查您的專案，例如根`pom.xml`和模組，並產生量身打造的代理程式指導。

   如果&#x200B;**AGENTS.md**&#x200B;已經存在，則&#x200B;**不會**&#x200B;覆寫。

1. 選擇安裝範圍。 對於此逐步解說，**專案**範圍是典型的，因此技能檔案會存在於存放庫中。
   ![選取安裝範圍](../assets/agent-skills/select-installation-scope.png)

1. 確認`.agents/skills`下的安裝。 您應該會看到&#x200B;**SKILLS.md**以及相關的參考和資產資料夾。
   ![檢閱已安裝的技能](../assets/agent-skills/review-installed-skills.png)

1. 當Adobe新增或更新技能時，請使用CLI來新增、更新、移除或列出這些技能。 若要檢視所有命令：

   ```shell
   $ npx skills --help
   ```

   ![檢閱可用的技能命令](../assets/agent-skills/review-available-skills-commands.png)

## 使用案例

<!-- 
CARDS
{target = _self}

* ../use-cases/component-development.md    
    {title = Create AEM Component with AI-assisted development}
    {description = Learn how to use AI-assisted development to develop AEM components.}
    {image = ../assets/component-development/review-generated-code.png}
    {cta = Create AEM Component}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Create AEM Component with AI-assisted development">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="../use-cases/component-development.md" title="使用AI輔助開發建立AEM元件" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="../assets/component-development/review-generated-code.png" alt="使用AI輔助開發建立AEM元件"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="../use-cases/component-development.md" target="_self" rel="referrer" title="使用AI輔助開發建立AEM元件">使用AI輔助開發建立AEM元件</a>
                    </p>
                    <p class="is-size-6">瞭解如何使用AI輔助開發來開發AEM元件。</p>
                </div>
                <a href="../use-cases/component-development.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
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
