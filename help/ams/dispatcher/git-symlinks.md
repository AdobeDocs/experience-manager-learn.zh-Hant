---
title: 正確地將Symlink新增至GIT
description: 有關在使用Dispatcher設定時如何以及在何處新增符號連結的說明。
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 6e751586-e92e-482d-83ce-6fcae4c1102c
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '1252'
ht-degree: 0%

---

# 將符號連結新增至GIT

[目錄](./overview.md)

[&lt; — 上一步：Dispatcher健康狀態檢查](./health-check.md)

在AMS中，您會取得預先填入的GIT存放庫，其中包含您的Dispatcher原始程式碼成熟度，可讓您開始開發和自訂。

在您建立第一個 `.vhost` 檔案，或頂層 `farm.any` 檔案，您將需要從 `available_*` 目錄到 `enabled_*` 目錄。 使用正確的連結型別將是透過Cloud Manager管道成功部署的關鍵。 此頁面可協助您瞭解如何執行此動作。

## Dispatcher原型

AEM開發人員通常從以下位置開始他們的專案 [AEM原型](https://github.com/adobe/aem-project-archetype)

以下是原始程式碼區域的範例，您可在此看到使用的符號連結：

```
$ tree dispatcher
dispatcher
└── src
   ├── conf.d
.....SNIP.....
    │   └── available_vhosts
    │   │   ├── 000_unhealthy_author.vhost
    │   │   ├── 000_unhealthy_publish.vhost
    │   │   ├── aem_author.vhost
    │   │   ├── aem_flush.vhost
    │   │   ├── aem_health.vhost
    │   │   ├── aem_lc.vhost
    │   │   └── aem_publish.vhost
    └── dispatcher_vhost.conf
    │   └── enabled_vhosts
    │   │   ├── aem_author.vhost -> ../available_vhosts/aem_author.vhost
    │   │   ├── aem_flush.vhost -> ../available_vhosts/aem_flush.vhost
    │   │   ├── aem_health.vhost -> ../available_vhosts/aem_health.vhost
    │   │   └── aem_publish.vhost -> ../available_vhosts/aem_publish.vhost
.....SNIP.....
    └── conf.dispatcher.d
    │   ├── available_farms
    │   │   ├── 000_ams_author_farm.any
    │   │   ├── 001_ams_lc_farm.any
    │   │   └── 002_ams_publish_farm.any
.....SNIP.....
    │   └── enabled_farms
    │   │   ├── 000_ams_author_farm.any -> ../available_farms/000_ams_author_farm.any
    │   │   └── 002_ams_publish_farm.any -> ../available_farms/002_ams_publish_farm.any
.....SNIP.....
17 directories, 60 files
```

例如， `/etc/httpd/conf.d/available_vhosts/` 目錄包含分段潛在 `.vhost` 我們可在執行組態中使用的檔案。

已啟用 `.vhost` 檔案將顯示為相對路徑 `symlinks` 內部 `/etc/httpd/conf.d/enabled_vhosts/` 目錄。

## 建立符號連結

我們使用檔案的符號連結，因此Apache Webserver會將目的地檔案視為相同的檔案。  我們不想在兩個目錄中複製檔案。  而只是從某個目錄（符號連結）到另一個目錄的捷徑。

瞭解您部署的組態會針對Linux主機。  建立與目標系統不相容的symlink將會導致失敗和不需要的結果。

如果您的工作站不是Linux電腦，您可能會想知道要使用哪些命令來正確建立這些連結，以便這些連結能夠提交到GIT中。

> `TIP:` 使用相對連結很重要，因為如果您安裝了Apache Webserver的本機復本，且安裝基礎不同，連結仍可運作。  如果您使用絕對路徑，那麼您的工作站或其他系統都必須符合相同的確切目錄結構。

### OSX / Linux

符號連結是這些作業系統的原生連結，以下是如何建立這些連結的一些範例。  開啟您最愛的終端機應用程式，並使用下列範例命令建立連結：

```
$ cd <LOCATION OF CLONED GIT REPO>\src\conf.d\enabled_vhosts
$ ln -s ../available_vhosts/<Destination File Name> <Target File Name>
```

以下是供參考的填入命令範例：

```
$ git clone https://github.com/adobe/aem-project-archetype.git
$ cd aem-project-archetype/src/main/archetype/dispatcher.ams/src/conf.d/enabled_vhosts/
$ ln -s ../available_vhosts/aem_flush.vhost aem_flush.vhost
```

如果您使用列出檔案，以下是現在連結的範例 `ls` 命令：

```
ls -l
total 0
lrwxrwxrwx. 1 root root 35 Oct 13 21:38 aem_flush.vhost -> ../available_vhosts/aem_flush.vhost
```

### Windows

> `Note:` 原來MS Windows （較佳的是NTFS）支援符號連結，因為……Windows Vista！

![顯示mklink命令說明輸出的Windows命令提示字元圖片](./assets/git-symlinks/windows-terminal-mklink.png)

> `Warning:` 建立symlink的mklink命令需要管理員許可權才能正確執行。 即使是管理員帳戶，除非您已啟用開發人員模式，否則您將需要以管理員身分執行命令提示字元
> <br/>不適當的許可權：
> ![Windows命令提示字元圖片，顯示命令因許可權而失敗](./assets/git-symlinks/windows-mklink-underpriv.png)
> <br/>適當的許可權：
> ![以系統管理員身分執行Windows命令提示字元的圖片](./assets/git-symlinks/windows-mklink-properpriv.png)

以下是建立連結的命令：

```
C:\<PATH TO SRC>\enabled_vhosts> mklink <Target File Name> ..\available_vhosts\<Destination File Name>
```


以下是供參考的填入命令範例：

```
C:\> git clone https://github.com/adobe/aem-project-archetype.git
C:\> cd aem-project-archetype\src\main\archetype\dispatcher.ams\src\conf.d\enabled_vhosts\
C:\aem-project-archetype\src\main\archetype\dispatcher.ams\src\conf.d\enabled_vhosts> mklink aem_flush.vhost ..\available_vhost\aem_flush.vhost
symbolic link created for aem_flush.vhost <<===>> ..\available_vhosts\aem_flush.vhost
```

#### 開發人員模式( Windows 10 )

放入時 [開發人員模式](https://docs.microsoft.com/en-us/windows/apps/get-started/enable-your-device-for-development)，Windows 10可讓您更輕鬆地測試您正在開發的應用程式、使用Ubuntu Bash殼層環境、變更各種以開發人員為中心的設定，以及執行其他類似作業。

Microsoft似乎會持續新增功能至開發人員模式，或是在這些功能獲得更廣泛採用且被視為穩定後，預設會啟用其中的部分功能（例如，使用建立者更新，Ubuntu Bash Shell環境不再需要開發人員模式）。

符號連結呢？ 啟用開發人員模式後，就不需要以提升的許可權執行命令提示字元來建立符號連結。 因此，在啟用開發人員模式後，任何使用者都可以建立符號連結。

> 啟用「開發人員模式」後，使用者應登出/登入，變更才會生效。

現在您無需以管理員身分執行即可看到命令運作

![以一般使用者身分執行Windows命令提示字元並啟用開發人員模式的圖片](./assets/git-symlinks/windows-mklink-devmode.png)

#### 替代/程式化方法

有特定原則可允許特定使用者建立符號連結→ [建立符號連結(Windows 10) - Windows安全性 | Microsoft檔案](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/create-symbolic-links)

PRO：
- 客戶可運用此功能，以程式設計方式允許在其組織（即Active Directory）內為所有開發人員建立符號連結，而不需在每個裝置上手動啟用開發人員模式。
- 此外，此原則應該在不提供開發人員模式的舊版MS Windows中提供。

CON：
- 此原則似乎對屬於Administrators群組的使用者沒有影響。 管理員仍需要以提升的許可權執行命令提示字元。 奇怪。

> 使用者必須登出/登入，本機/群組原則變更才會生效。

執行 `gpedit.msc`，視需要新增/變更使用者。 管理員預設存在

![群組原則編輯器視窗顯示調整許可權](./assets/git-symlinks/windows-localpolicies-symlinks.png)

#### 在GIT中啟用符號連結

Git會根據core.symlinks選項處理符號連結

來源： [Git - git-config檔案](https://git-scm.com/docs/git-config#Documentation/git-config.txt-coresymlinks)

*如果core.symlinks為false，符號連結會以包含連結文字的小型純檔案形式出庫。 `git-update-index[1]` 和 `git-add[1]` 不會將錄製型別變更為一般檔案。 對於不支援符號連結的檔案系統（例如FAT）非常有用。
預設值為true，但以下情況除外 `git-clone[1]` 或 `git-init[1] will probe and set core.symlinks false if appropriate when the repository is created.` 在大多數情況下，Git會假設Windows對符號連結沒有好處，並會將此項設定為false。*

以下說明Git在Windows上的行為：符號連結· git-for-windows/git Wiki · GitHub

> `Info`：上述連結的檔案中所列的假設似乎與AEM Developer在Windows上的可能設定沒有關係，最明顯的是NTFS，以及我們只有檔案符號連結與目錄符號連結的差異

好消息如下，因為 [Windows 2.10.2版適用的Git](https://github.com/git-for-windows/git/releases/tag/v2.10.2.windows.1) 安裝程式有 [明確選項以啟用符號連結支援。](https://github.com/git-for-windows/git/issues/921)

> `Warning`：可在執行階段複製存放庫時提供core.symlink選項，或可儲存為全域設定。

![顯示GIT安裝程式，顯示對symlink的支援](./assets/git-symlinks/windows-git-install-symlink.png)

Windows適用的Git將儲存全域偏好設定於 `"C:\Program Files\Git\etc\gitconfig"` . 其他Git案頭使用者端應用程式可能不會考慮這些設定。
訣竅如下，並非所有開發人員都會使用Git原生使用者端（即Git Cmd、Git Bash），而部分Git案頭應用程式（如GitHub Desktop、Atlassian Sourcetree）可能有不同的設定/預設來使用System或內嵌Git

以下是內容範例： `gitconfig` 檔案

```
[diff "astextplain"]
    textconv = astextplain
[filter "lfs"]
    clean = git-lfs clean -- %f
    smudge = git-lfs smudge -- %f
    process = git-lfs filter-process
    required = true
[http]
    sslBackend = openssl
    sslCAInfo = C:/Program Files/Git/mingw64/ssl/certs/ca-bundle.crt
[core]
    autocrlf = true
    fscache = true
    symlinks = true
[pull]
    rebase = false
[credential]
    helper = manager-core
[credential "https://dev.azure.com"]
    useHttpPath = true
[init]
    defaultBranch = master
```

#### Git命令列提示

在某些情況下，您可能需要建立新的符號連結（例如新增主機或新陣列）。

我們在上述檔案中看到，Windows提供「mklink」命令來建立符號連結。

如果您在Git Bash環境中工作，可以改用標準Bash指令 `ln -s` 但前置詞必須以特殊指示字元，例如下列範例：

```
MSYS=winsymlinks:nativestrict ln -s test_vhost_symlink ../dispatcher/src/conf.d/available_vhosts/default.vhost
```

#### 摘要

若要讓Git在Microsoft Windows作業系統上正確處理符號連結(至少是在目前AEM Dispatcher設定基準線的範圍內)，您需要：

| 項目 | 最低版本/設定 | 建議的版本/設定 |
|------|---------------------------------|-------------------------------------|
| 作業系統 | Windows Vista或更新版本 | Windows 10 Creator Update或更新版本 |
| 檔案系統 | NTFS | NTFS |
| 能夠為Windows使用者處理symlink | `"Create symbolic links"` 群組/本機原則 `under "Group Computer Configuration\Windows Settings\Security Settings\Local Policies\User Rights Assignment"` | Windows 10開發人員模式已啟用 |
| GIT | 原生使用者端1.5.3版 | Native Client 2.10.2或更新版本 |
| Git設定 | `--core.symlinks=true` 選項（從命令列執行Git複製時） | Git全域設定<br/>`[core]`<br/>    symlinks = true <br/> 原生Git使用者端設定路徑： `C:\Program Files\Git\etc\gitconfig` <br/>Git案頭使用者端的標準位置： `%HOMEPATH%\.gitconfig` |

> `Note:` 如果您已經有本機存放庫，則需要從來源重新複製。 您可以複製至新的位置，並手動將您未認可/未暫存的本機變更合併至新複製的存放庫。
