---
title: 使用存放庫工具設定IntelliJ
description: 準備您的IntelliJ以與AEM雲端就緒例項同步
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: Development
kt: 8844
source-git-commit: d38da94bd4164163a16899b565c90b159194580a
workflow-type: tm+mt
source-wordcount: '504'
ht-degree: 2%

---

# 安裝Cygwin


Cygwin是與POSIX相容的寫程式和運行時環境，在Microsoft Windows上本機運行。
安裝 [齊格溫](https://www.cygwin.com/). 我已在C:\cygwin64 folder中安裝
>[!NOTE]
> 請務必安裝zip、unzip、curl、rsync套件並安裝cygwin

在c:\cloudmanager下建立名為adoberepo的資料夾。

[安裝存放庫工具].(https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo).Installing)存放庫工具只是複製存放庫檔案並放入您的c:\cloudmanger\adoberepo folder中。

將下列內容新增至Path環境變數C:\cygwin64\bin;C:\CloudManager\adoberepo;

## 設定外部工具

* 啟動IntelliJ
* 按Ctrl+Alt+S鍵以啟動設定視窗。
* 選擇「工具」 — >「外部工具」，然後按一下+號，然後輸入以下內容，如螢幕抓圖所示。
   ![rep](assets/repo.png)
* 請務必在「群組」下拉式欄位中輸入「repo」，並將您建立的所有命令歸屬於 **repo** 群組


**Put命令**
**方案**:C:\cygwin64\bin\bash
**引數**:-l C:\CloudManager\adoberepo\repo put -f \$FilePath\$
**工作目錄**:\$ProjectFileDir\$
![put-command](assets/put-command.png)

**獲取命令**
**方案**:C:\cygwin64\bin\bash
**引數**:-l C:\CloudManager\adoberepo\repo get -f \$FilePath\$
**工作目錄**:\$ProjectFileDir\$
![get-command](assets/get-command.png)

**狀態命令**
**方案**:C:\cygwin64\bin\bash
**引數**:-l C:\CloudManager\adoberepo\repo st -f \$FilePath\$
**工作目錄**:\$ProjectFileDir\$
![status-command](assets/status-command.png)

**Diff命令**
**方案**:C:\cygwin64\bin\bash
**引數**:-l C:\CloudManager\adoberepo\repo diff -f $FilePath$
**工作目錄**:\$ProjectFileDir\$
![diff命令](assets/diff-command.png)

從中擷取.repo檔案 [repo.zip](assets/repo.zip) 並將其置於AEM專案根資料夾中。 (C:\CloudManager\aem-banking-application)。 開啟.repo檔案，確認伺服器和憑證設定符合您的環境。
開啟.gitignore檔案，並將下列項目新增至檔案底部，然後儲存變更\# repo .repo

選取aem-banking-application專案內的任何專案，例如ui.content和右鍵，您應該會看到repo選項，而在repo選項下方，您會看到我們先前新增的4個命令。

## 設定AEM製作例項

可依照下列步驟，在您的本機系統上快速設定雲端就緒例項。
* [下載最新AEM SDK](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)

* [下載最新的AEM Forms addon](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)

* 建立下列資料夾結構c:\aemformscs\aem-sdk\author

* 從AEM SDK zip檔案中解壓縮aem-sdk-quickstart-xxxxxxx.jar檔案，並將其置於c:\aemformscs\aem-sdk\author folder.Rename中，將jar檔案置於aem-author-p4502.jar

* 開啟命令提示符並導航到c:\aemformscs\aem-sdk\author enter the following command java -jar aem-author-p4502.jar -gui。 這會開始安裝AEM。
* 使用管理員/管理員憑證登入
* 停止AEM例項
* 建立下列資料夾結構。C:\aemformscs\aem-sdk\author\crx-quickstart\install
* 將aem-forms-addon-xxxxx.far複製至安裝資料夾
* 開啟命令提示符並導航到c:\aemformscs\aem-sdk\author enter the following command java -jar aem-author-p4502.jar -gui。 這會在您的AEM執行個體中部署forms附加套件。



















