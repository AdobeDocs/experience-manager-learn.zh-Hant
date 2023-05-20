---
title: 正確將符號連結添加到GIT
description: 有關在處理Dispatcher配置時如何添加符號連結以及在何處添加符號連結的說明。
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
exl-id: 6e751586-e92e-482d-83ce-6fcae4c1102c
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '1252'
ht-degree: 0%

---

# 將符號連結添加到GIT

[目錄](./overview.md)

[&lt; — 上一個：Dispatcher運行狀況檢查](./health-check.md)

在AMS中，您將獲得一個預填充的GIT儲存庫，其中包含調度程式的原始碼，該儲存庫已成熟，可供您開始開發和自定義。

建立第一個 `.vhost` 檔案或頂級 `farm.any` 檔案，您需要從 `available_*` 目錄 `enabled_*` 的子菜單。 使用正確的連結類型將是通過雲管理器管道成功部署的關鍵。 此頁將幫助您瞭解如何執行此操作。

## 調度器原型

開發AEM人員通常從 [AEM原型](https://github.com/adobe/aem-project-archetype)

下面是原始碼區域的示例，您可以在其中查看使用的符號連結：

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

例如， `/etc/httpd/conf.d/available_vhosts/` 目錄包含暫存的潛在 `.vhost` 我們在運行配置中使用的檔案。

已啟用 `.vhost` 檔案將顯示為相對路徑 `symlinks` 內 `/etc/httpd/conf.d/enabled_vhosts/` 的子菜單。

## 建立符號連結

我們使用指向檔案的符號連結，以便Apache Webserver將目標檔案視為同一檔案。  我們不想在兩個目錄中複製該檔案。  而是從一個目錄（符號連結）到另一個目錄的快捷方式。

認識到您部署的配置將針對Linux主機。  建立與目標系統不相容的符號連結將導致失敗和不需要的結果。

如果您的工作站不是Linux電腦，您可能會想知道使用哪些命令來正確建立這些連結，以便它們可以將它們提交到GIT。

> `TIP:` 使用相對連結很重要，因為如果安裝了Apache Webserver的本地副本，並且安裝了不同的安裝基礎，連結仍然有效。  如果使用絕對路徑，則工作站或其他系統必須與相同的目錄結構匹配。

### OSX/Linux

符號連結是這些作業系統的固有屬性，下面是建立這些連結的一些示例。  開啟您最喜愛的終端應用程式，然後使用以下示例命令建立連結：

```
$ cd <LOCATION OF CLONED GIT REPO>\src\conf.d\enabled_vhosts
$ ln -s ../available_vhosts/<Destination File Name> <Target File Name>
```

下面是一個填充的命令示例供參考：

```
$ git clone https://github.com/adobe/aem-project-archetype.git
$ cd aem-project-archetype/src/main/archetype/dispatcher.ams/src/conf.d/enabled_vhosts/
$ ln -s ../available_vhosts/aem_flush.vhost aem_flush.vhost
```

下面是連結的示例，如果您使用 `ls` 命令：

```
ls -l
total 0
lrwxrwxrwx. 1 root root 35 Oct 13 21:38 aem_flush.vhost -> ../available_vhosts/aem_flush.vhost
```

### Windows

> `Note:` 結果MS Windows（更好，NTFS）支援符號連結，因為……Windows Vista!

![顯示mklink命令幫助輸出的Windows命令提示符圖片](./assets/git-symlinks/windows-terminal-mklink.png)

> `Warning:` mklink命令建立symlinks需要管理員權限才能正確運行。 即使作為管理員帳戶，您也需要運行命令提示符「作為管理員」，除非您啟用了開發人員模式
> <br/>權限不正確：
> ![顯示由於權限而導致命令失敗的Windows命令提示符的圖片](./assets/git-symlinks/windows-mklink-underpriv.png)
> <br/>正確的權限：
> ![Windows命令提示符的圖片以管理員身份運行](./assets/git-symlinks/windows-mklink-properpriv.png)

以下是建立連結的命令：

```
C:\<PATH TO SRC>\enabled_vhosts> mklink <Target File Name> ..\available_vhosts\<Destination File Name>
```


下面是一個填充的命令示例供參考：

```
C:\> git clone https://github.com/adobe/aem-project-archetype.git
C:\> cd aem-project-archetype\src\main\archetype\dispatcher.ams\src\conf.d\enabled_vhosts\
C:\aem-project-archetype\src\main\archetype\dispatcher.ams\src\conf.d\enabled_vhosts> mklink aem_flush.vhost ..\available_vhost\aem_flush.vhost
symbolic link created for aem_flush.vhost <<===>> ..\available_vhosts\aem_flush.vhost
```

#### 開發人員模式(Windows 10)

放入 [開發人員模式](https://docs.microsoft.com/en-us/windows/apps/get-started/enable-your-device-for-development),Windows 10允許您更輕鬆地test正在開發的應用程式、使用Ubuntu Bash外殼環境、更改各種以開發人員為中心的設定，以及執行其他類似操作。

Microsoft似乎在繼續向開發者模式添加功能，或者在這些功能達到更廣泛的採用並被視為穩定後，預設情況下啟用其中一些功能（例如，使用建立者更新時，Ubuntu Bash Shell環境不再需要開發者模式）。

交響曲呢？ 啟用開發者模式後，無需運行具有提升權限的命令提示符來建立符號連結。 因此，一旦啟用了「開發人員模式」，任何用戶都可以建立符號連結。

> 啟用開發人員模式後，用戶應註銷/登錄以使更改生效。

現在，您可以看到，不以管理員身份運行，該命令將起作用

![Windows命令提示符的圖片以啟用開發模式的普通用戶運行](./assets/git-symlinks/windows-mklink-devmode.png)

#### 備選/方案方法

有一個特定策略，允許某些用戶建立符號連結→ [建立符號連結(Windows 10)- Windows安全 |Microsoft文檔](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/create-symbolic-links)

專業人員：
- 客戶可以利用此功能以寫程式方式允許建立符號連結到其組織（即Active Directory）內的所有開發人員，而無需在每台設備上手動啟用開發人員模式。
- 此外，此策略應在不提供開發者模式的MS Windows的早期版本中提供。

CON:
- 此策略似乎對屬於「管理員」組的用戶沒有影響。 管理員仍需要使用提升的權限運行命令提示符。 真奇怪。

> 對本地/組策略的更改要生效，將需要用戶註銷/登錄。

運行 `gpedit.msc`，根據需要添加/更改用戶。 預設情況下，管理員在

![顯示調整權限的組策略編輯器窗口](./assets/git-symlinks/windows-localpolicies-symlinks.png)

#### 在GIT中啟用符號連結

Git根據core.symlinks選項處理符號連結

來源： [Git - git-config文檔](https://git-scm.com/docs/git-config#Documentation/git-config.txt-coresymlinks)

*如果core.symlinks為false，則符號連結會作為包含連結文本的小型純檔案檢出。 `git-update-index[1]` 和 `git-add[1]` 將不會將錄制的類型更改為常規檔案。 對於不支援符號連結的FAT等檔案系統非常有用。
預設值為true, `git-clone[1]` 或 `git-init[1] will probe and set core.symlinks false if appropriate when the repository is created.` 在大多數情況下，Git會假設Windows對符號連結沒有好處，並將其設定為false。*

Git在Windows上的行為在以下方面得到了很好的解釋：符號連結· Git-for-windows/git Wiki · GitHub

> `Info`:上面連結的文檔中列出的假設似乎與Windows上可能的AEMDeveloper安裝（最特別是NTFS）以及我們只有檔案符號連結與目錄符號連結這一事實一致

好消息是 [Windows 2.10.2版的Git](https://github.com/git-for-windows/git/releases/tag/v2.10.2.windows.1) 安裝程式 [explicit選項，用於啟用符號連結支援。](https://github.com/git-for-windows/git/issues/921)

> `Warning`:在克隆儲存庫時，可以在運行時提供core.symlink選項，或以其他方式將其儲存為全局配置。

![顯示顯示對符號連結支援的GIT安裝程式](./assets/git-symlinks/windows-git-install-symlink.png)

Git for Windows將在中儲存全局首選項 `"C:\Program Files\Git\etc\gitconfig"` 。 其他Git案頭客戶端應用可能不考慮這些設定。
以下是問題，並非所有開發人員都會使用Git本機客戶端（即Git Cmd、Git Bash），而某些Git案頭應用（例如GitHub案頭、Atlassian源樹）可能具有不同的設定/預設值來使用系統或嵌入式Git

這裡是一個 `gitconfig` 檔案

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

#### Git命令行提示

可能存在必須建立新符號連結（例如添加新主機或新伺服器場）的情況。

我們在上面的文檔中看到，Windows提供了「mklink」命令來建立符號連結。

如果您在Git Bash環境中工作，則可以改用標準Bash命令 `ln -s` 但是它必須用一個特殊的說明來前置詞，例如：

```
MSYS=winsymlinks:nativestrict ln -s test_vhost_symlink ../dispatcher/src/conf.d/available_vhosts/default.vhost
```

#### 摘要

要使Git在MicrosoftWindows OS上正確處理符號連結(至少AEM在當前調度程式配置基線的範圍)，您需要：

| 項目 | 最低版本/配置 | 推薦的版本/配置 |
|------|---------------------------------|-------------------------------------|
| 作業系統 | Windows Vista或更高版本 | Windows 10建立者更新或更新 |
| 檔案系統 | NTFS | NTFS |
| 能夠處理Windows用戶的符號連結 | `"Create symbolic links"` 組/本地策略 `under "Group Computer Configuration\Windows Settings\Security Settings\Local Policies\User Rights Assignment"` | Windows 10開發人員模式已啟用 |
| GIT | 本機客戶端1.5.3版 | 本機客戶端2.10.2版或更高版本 |
| Git配置 | `--core.symlinks=true` 選項從命令行執行git克隆時 | Git全局配置<br/>`[core]`<br/>    符號連結=true <br/> 本機Git客戶端配置路徑： `C:\Program Files\Git\etc\gitconfig` <br/>Git案頭客戶端的標準位置： `%HOMEPATH%\.gitconfig` |

> `Note:` 如果已經有本地儲存庫，則需要從源重新克隆。 可以克隆到新位置，並將未提交/未轉移的本地更改手動合併到新克隆的儲存庫中。
