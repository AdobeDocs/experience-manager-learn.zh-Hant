---
title: 正確將符號連結新增至GIT
description: 使用Dispatcher設定時，如何新增符號連結及新增位置的相關指示。
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: df3afc60f765c18915eca3bb2d3556379383fafc
workflow-type: tm+mt
source-wordcount: '1252'
ht-degree: 0%

---


# 將符號連結新增至GIT

[目錄](./overview.md)

[&lt; — 上一個：Dispatcher健康狀況檢查](./health-check.md)

在AMS中，您會得到預先填入的GIT存放庫，其中包含Dispatcher的原始碼已成熟，且可供您開始開發和自訂。

在您首次建立 `.vhost` 檔案或頂級 `farm.any` 檔案，您需要從 `available_*` 目錄 `enabled_*` 目錄。 若要透過Cloud Manager管道成功部署，使用適當的連結類型將是關鍵。 本頁面將協助您了解如何執行此作業。

## Dispatcher原型

AEM開發人員通常會從 [AEM原型](https://github.com/adobe/aem-project-archetype)

以下是原始碼區域的範例，您可在其中看到使用的符號連結：

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

例如， `/etc/httpd/conf.d/available_vhosts/` 目錄包含暫存潛在 `.vhost` 可在執行中設定的檔案。

已啟用 `.vhost` 檔案會顯示為相對路徑 `symlinks` 內 `/etc/httpd/conf.d/enabled_vhosts/` 目錄。

## 建立符號連結

我們使用指向檔案的符號連結，因此Apache Webserver會將目標檔案視為相同的檔案。  我們不想在兩個目錄中複製檔案。  而是從一個目錄（符號連結）到另一個目錄的快捷方式。

識別您部署的配置將針對Linux主機。  建立與目標系統不相容的符號連結將導致失敗和不想要的結果。

如果您的工作站不是Linux電腦，您可能會想知道要使用哪些命令正確建立這些連結，以便將連結提交至GIT。

> `TIP:` 使用相對連結非常重要，因為如果您安裝了Apache Webserver的本地副本，並且安裝了不同的安裝庫，這些連結仍然有效。  如果您使用絕對路徑，則工作站或其他系統必須符合相同的確切目錄結構。

### OSX / Linux

Symlink是這些作業系統的原生連結，以下是如何建立這些連結的一些範例。  開啟您最喜愛的終端機應用程式，並使用下列範例命令來建立連結：

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

以下是現在連結的範例，如果您使用 `ls` 命令：

```
ls -l
total 0
lrwxrwxrwx. 1 root root 35 Oct 13 21:38 aem_flush.vhost -> ../available_vhosts/aem_flush.vhost
```

### Windows

> `Note:` 事實證明MS Windows（更好，NTFS）支援符號連結，因為……Windows Vista!

![顯示mklink命令幫助輸出的Windows命令提示符的圖片](./assets/git-symlinks/windows-terminal-mklink.png)

> `Warning:` mklink命令建立symlink需要管理員權限才能正確運行。 即使您是管理員帳戶，除非您已啟用開發人員模式，否則您仍需執行命令提示「以管理員身分」
> <br/>權限不當：
> ![Windows命令提示符的圖片顯示命令因權限而失敗](./assets/git-symlinks/windows-mklink-underpriv.png)
> <br/>正確權限：
> ![Windows命令提示符的圖片以管理員身份運行](./assets/git-symlinks/windows-mklink-properpriv.png)

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

#### 開發人員模式(Windows 10)

放進 [開發人員模式](https://docs.microsoft.com/en-us/windows/apps/get-started/enable-your-device-for-development), Windows 10可讓您更輕鬆地測試正在開發的應用程式、使用Ubuntu Bash殼層環境、更改各種以開發人員為中心的設定，以及執行其他此類操作。

Microsoft似乎會持續在開發人員模式中新增功能，或在這些功能達到更廣泛的採用且被視為穩定狀態時，依預設啟用其中一些功能（例如，透過建立者更新，Ubuntu Bash Shell環境不再需要開發人員模式）。

交響樂呢？ 啟用開發人員模式後，無需以提升的權限運行命令提示符即可建立符號連結。 因此，一旦啟用「開發人員模式」，任何使用者都可以建立符號連結。

> 啟用開發人員模式後，用戶應註銷/登錄以使更改生效。

現在，您可以看到，若不以管理員身分執行，命令即可運作

![Windows命令提示符的圖片以普通用戶的身份運行，並啟用了開發人員模式](./assets/git-symlinks/windows-mklink-devmode.png)

#### 替代/方案方法

有特定的策略可允許某些用戶建立符號連結→ [建立符號連結(Windows 10)- Windows安全性 | Microsoft檔案](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/create-symbolic-links)

專業技術：
- 客戶可以利用此工具，以程式設計方式為組織內的所有開發人員（即Active Directory）建立符號連結，而無須在每部裝置上手動啟用開發人員模式。
- 此外，此策略應在不提供開發人員模式的舊版MS Windows中提供。

內容：
- 此策略似乎對屬於管理員組的用戶沒有影響。 管理員仍需以提升的權限運行命令提示符。 真奇怪。

> 要使對本地/組策略的更改生效，將需要用戶註銷/登錄。

執行 `gpedit.msc`，視需要新增/變更使用者。 預設情況下，管理員位於

![「組策略編輯器」窗口顯示調整的權限](./assets/git-symlinks/windows-localpolicies-symlinks.png)

#### 在GIT中啟用符號連結

Git會根據core.symlinks選項處理symlinks

來源： [Git - git-config檔案](https://git-scm.com/docs/git-config#Documentation/git-config.txt-coresymlinks)

*如果core.symlinks為false，則符號連結將作為包含連結文本的普通小檔案簽出。 `git-update-index[1]` 和 `git-add[1]` 不會將記錄的類型變更為一般檔案。 對於不支援符號連結的檔案系統（如FAT）非常有用。
預設值為true, `git-clone[1]` 或 `git-init[1] will probe and set core.symlinks false if appropriate when the repository is created.` 在大多數情況下，Git會假設Windows對符號連結沒有好處，且會將此值設為false。*

以下是Git在Windows上的行為的詳細說明：符號連結· git-for-windows/git Wiki · GitHub

> `Info`:上面連結的文檔中列出的假設與Windows上可能設定的AEM開發人員（最明顯的是NTFS）以及我們只有檔案符號連結和目錄符號連結的事實似乎是正確的

好消息是 [Windows適用的Git 2.10.2版](https://github.com/git-for-windows/git/releases/tag/v2.10.2.windows.1) 安裝程式具有 [啟用符號連結支援的顯式選項。](https://github.com/git-for-windows/git/issues/921)

> `Warning`:複製存放庫時，可在執行階段提供core.symlink選項，或以其他方式儲存為全域設定。

![顯示GIT安裝程式，顯示對symlinks的支援](./assets/git-symlinks/windows-git-install-symlink.png)

適用於Windows的Git會將全域偏好設定儲存於 `"C:\Program Files\Git\etc\gitconfig"` . 其他Git案頭用戶端應用程式不得考慮這些設定。
以下是要了解的，並非所有開發人員都會使用Git原生用戶端（例如Git Cmd、Git Bash），而某些Git案頭應用程式（例如GitHub Desktop、Atlassian Sourcetree）可能具有不同的設定/預設值以使用系統或內嵌Git

以下是內容的範例 `gitconfig` 檔案

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

在某些情況下，您可能必須建立新的符號連結（例如，新增主機或新伺服器陣列）。

我們在上面的檔案中看到，Windows提供「mklink」命令來建立符號連結。

若您在Git Bash環境中工作，則可改用標準Bash命令 `ln -s` 但必須以特殊指令加上前置詞，如下所示：

```
MSYS=winsymlinks:nativestrict ln -s test_vhost_symlink ../dispatcher/src/conf.d/available_vhosts/default.vhost
```

#### 摘要

若要在Microsoft Windows OS上讓Git正確處理符號連結(至少在目前AEM Dispatcher設定基線的範圍內)，您需要：

| 項目 | 最低版本/配置 | 建議的版本/配置 |
|------|---------------------------------|-------------------------------------|
| 作業系統 | Windows Vista或更高版本 | Windows 10 Creator更新或更高版本 |
| 檔案系統 | NTFS | NTFS |
| 能夠為Windows用戶處理符號連結 | `"Create symbolic links"` 組/本地策略 `under "Group Computer Configuration\Windows Settings\Security Settings\Local Policies\User Rights Assignment"` | 啟用Windows 10開發人員模式 |
| GIT | 本機用戶端1.5.3版 | 本機用戶端2.10.2版或更新版本 |
| Git設定 | `--core.symlinks=true` 選項 | Git全域設定<br/>`[core]`<br/>    symlinks = true <br/> 原生Git用戶端設定路徑： `C:\Program Files\Git\etc\gitconfig` <br/>Git案頭用戶端的標準位置： `%HOMEPATH%\.gitconfig` |

> `Note:` 如果您已有本機存放庫，則需要從來源複製。 您可以將原地複製到新位置，並將未提交/未分段的本機變更手動合併至新複製的存放庫。