---
title: 使用回購工具設定IntelliJ
description: 準備IntelliJ以與雲就緒實AEM例同步
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8844
exl-id: 9a7ed792-ca0d-458f-b8dd-9129aba37df6
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '504'
ht-degree: 2%

---

# 安裝Cygwin


Cygwin是一個與POSIX相容的寫程式和運行時環境，在MicrosoftWindows上以本機方式運行。
安裝 [齊格溫](https://www.cygwin.com/)。 我已安裝在C:\cygwin64 folder
>[!NOTE]
> 確保在cygwin安裝時安裝zip、unzip、curl和rsync包

在c:\cloudmanager目錄下建立一個名為adoberepo的資料夾。

[安裝回購工具]。(https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo).Installing)回購工具只是複製回購檔案並將其放在c:\cloudmanger\adoberepo folder中。

將以下內容添加到路徑環境變數C:\cygwin64\bin;C:\CloudManager\adoberepo;

## 設定外部工具

* 啟動IntelliJ
* 按Ctrl+Alt+S鍵啟動設定窗口。
* 選擇「工具」 — >「外部工具」，然後按一下+號並按螢幕抓圖中所示輸入以下內容。
   ![代表](assets/repo.png)
* 通過在「組」下拉欄位中鍵入「repo」，並且您建立的所有命令均屬於 **回購** 組


**放置命令**
**計畫**:C:\cygwin64\bin\bash
**參數**:-l C:\CloudManager\adoberepo\repo put -f \$FilePath\$
**工作目錄**:\$ProjectFileDir\$
![命令](assets/put-command.png)

**獲取命令**
**計畫**:C:\cygwin64\bin\bash
**參數**:-l C:\CloudManager\adoberepo\repo get -f \$FilePath\$
**工作目錄**:\$ProjectFileDir\$
![get命令](assets/get-command.png)

**狀態命令**
**計畫**:C:\cygwin64\bin\bash
**參數**:-l C:\CloudManager\adoberepo\repo st -f \$FilePath\$
**工作目錄**:\$ProjectFileDir\$
![狀態命令](assets/status-command.png)

**比較命令**
**計畫**:C:\cygwin64\bin\bash
**參數**:-l C:\CloudManager\adoberepo\repo diff -f $FilePath$
**工作目錄**:\$ProjectFileDir\$
![diff命令](assets/diff-command.png)

從中提取.repo檔案 [repo.zip](assets/repo.zip) 並將其放在項目根AEM資料夾中。 (C:\CloudManager\aem-banking-application)。 開啟.repo檔案，確保伺服器和憑據設定與您的環境匹配。
開啟.gitignore檔案，在檔案底部添加以下內容並保存更改\# repo .repo

在您的銀行應用程式項目中選擇任何項目，如ui.content，然後按一下右鍵，您應看到回購選項，在回購選項下，您將看到我們前面添加的4個命令。

## 設定AEM作者實例

可以執行以下步驟，在本地系統上快速設定雲就緒實例。
* [下載最新AEM的SDK](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)

* [下載最新的AEM Forms載入](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)

* 建立以下資料夾結構c:\aemformscs\aem-sdk\author

* 從AEMSDK zip檔案中解壓aem-sdk-quickstart-xxxxxx.jar檔案，並將其放在c:\aemformscs\aem-sdk\author folder.Rename中，將jar檔案放到aem-author-p4502.jar

* 開啟命令提示符並導航到c:\aemformscs\aem-sdk\author enter the following command java -jar aem-author-p4502.jar -gui。 這將開始安裝AEM。
* 使用管理員/管理員憑據登錄
* 停止實AEM例
* 建立以下資料夾結構。C:\aemformscs\aem-sdk\author\crx-quickstart\install
* 將aem-forms-addon-xxxxx.far複製到install資料夾中
* 開啟命令提示符並導航到c:\aemformscs\aem-sdk\author enter the following command java -jar aem-author-p4502.jar -gui。 這將部署實例中的表單添加AEM包。
