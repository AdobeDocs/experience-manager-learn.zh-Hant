---
title: 在Linux上安裝AEM Forms。
description: 安裝32位元程式庫，讓AEM Forms在Linux安裝中運作。
feature: 適用性表單
audience: developer
doc-type: article
activity: setup
version: 6.4, 6.5
topic: 開發
role: Developer
level: Beginner
kt: 7593
translation-type: tm+mt
source-git-commit: da7837d45a9d5f614a4f6527b7bfe98aaf980d4f
workflow-type: tm+mt
source-wordcount: '945'
ht-degree: 0%

---


# 安裝32位元版本的共用程式庫

當AEM FORMS. OSGi或AEM Formsj2EE部署在Linux上時，您必須確保已安裝並提供一組共用庫的32位版本。  說明來自套件本身。

* expat（James Clark編寫的面向流的XML剖析器C庫，用於剖析XML）
* fontconfig（字型設定和自訂程式庫，旨在在系統中找出字型，並根據應用程式指定的需求加以選取）
* freetype(字型轉換引擎，專為針對多種平台和環境提供進階字型支援而開發。 它可開啟和管理字型檔案，並有效率地載入、提示及轉譯個別字元。 它不是字型伺服器或完整的文字轉換程式庫)
* glibc（用於GNU系統和GNU/Linux系統的核心庫，以及許多以Linux為內核的其他系統）
* libcurl（用戶端URL傳輸程式庫）
* libICE（客戶端間Exchange庫）
* libicu（提供強穩且功能完整的Unicode和地區支援的程式庫- International Components for Unicode）。 此程式庫需要64位元和32位元版本
* libSM（X11會話管理庫）
* libuuid（DCE相容的通用唯一標識符庫）-用於為可在本地系統之外訪問的對象生成唯一標識符。
* libX11（X11用戶端程式庫）
* libXau（X11授權通訊協定——適用於限制用戶端對顯示器的存取）
* libxcb（X協定C語言綁定- XCB）
* libXext（X11通訊協定的常用擴充程式庫）
* libXinerama(X11擴充功能，支援將案頭延伸至多部顯示器。 Cinerama是一種寬屏電影格式，使用多部放映程式。 libXinerama是與RandR擴展介面的庫)
* libXrandr（Xinerama擴展）現在已經基本過時——它已被RandR擴展所取代)
* libXrender（X Rendering Extension用戶端程式庫）
nss-softokn-freebl（網路安全服務專用的Freebl程式庫）
* zlib（通用、無專利、無損的資料壓縮庫）

從Red Hat Enterprise Linux 6開始，程式庫的32位元版本的副檔名為。686，而64位元版本的副檔名為。x86_64。 例如expat.i686。 在RHEL 6之前，32位元版本的副檔名為。i386。 在安裝32位元版本之前，請確定已安裝最新的64位元版本。 如果程式庫的64位元版本比正在安裝的32位元版本舊，您會收到如下錯誤：

0m錯誤：受保護的多庫版本：libsepol-2.5-10.el7.x86_64 != libsepol-2.5-6.el7.i686 [0mError:發現多庫版本問題。]

## 首次安裝

在Red Hat Enterprise Linux上，使用YellowDog Update Modifier(YUM)進行安裝，如下所示：

1. yum install expat.i686
2. yum install fontconfig.i686
3. yum install freetype.i686
4. yum install glibc.i686
5. yum install libcurl.i686
6. yum install libICE.i686
7. yum install libicu.i686
8. yum install libicus
9. yum install libSM.i686
10. yum install libuuid.i686
11. yum install libX11.i686
12. yum install libXau.i686
13. yum install libxcb.i686
14. yum install libXext.i686
15. yum install libXinerama.i686
16. yum install libXrandr.i686
17. yum install libXrender.i686
18. yum install nss-softokn-freebl.i686
19. yum install zlib.i686

## Symlinks

此外，您還需要分別建立libcurl.so、libcrypto.so和libssl.so符號連結，這些符號連結指向libcurl、libcrypto和libssl庫的最新32位版本。 您可在/usr/lib/中找到檔案
ln -s /usr/lib/libcurl.so.4.5.0 /usr/lib/libcurl.so
ln -s /usr/lib/libcrypto.so.1.1.1c /usr/lib/libcrypto.so
ln -s /usr/lib/libssl.so.1.1.1c /usr/lib/libssl.so

## 對現有系統的更新

在更新期間，x86_64和i686體系結構之間可能會發生衝突，例如：
錯誤：事務檢查錯誤：
來自glibc-2.28-72.el8.i686安裝的檔案/lib/ld-2.28.so與來自軟體包glibc32-2.28-42.1.el8.x86_64的檔案衝突

如果您遇到此問題，請先解除安裝違規的套件，例如：
yum remove glibc32-2.28-42.1.el8.x86_64

所有說明和完成後，您希望x86_64和i686版本完全相同，例如，從此輸出到命令：
yum info glibc

上次中繼資料過期檢查：0:41:33前美國東部時間2020年1月18日上午11:37:08
已安裝的軟體包
名稱：glibc
版本：二點二八
發行：72.el8
架構：i686
大小：13 M
來源：glibc-2.28-72.el8.src.rpm
儲存庫：@System
從回購：BaseOS
摘要：GNU libc庫
URL:http://www.gnu.org/software/glibc/
授權：LGPLv2+和LGPLv2+（含例外）,GPLv2+和GPLv2+（含例外）,BSD和Inner-Net以及ISC和公共域和GFDL
說明：glibc套件包含標準程式庫，由：系統上的多個程式。 為了節省磁碟空間和：記憶體，以及使升級更加容易，通用的系統代碼是：集中存放，並在程式間共用。 此特定套件：包含最重要的共用程式庫集：標準C:程式庫和標準數學程式庫。 沒有這兩個資料庫，a:Linux系統無法正常工作。

名稱：glibc
版本：二點二八
發行：72.el8
架構：x86_64
大小：15 M
來源：glibc-2.28-72.el8.src.rpm
儲存庫：@System
從回購：BaseOS
摘要：GNU libc庫
URL:http://www.gnu.org/software/glibc/
授權：LGPLv2+和LGPLv2+（含例外）,GPLv2+和GPLv2+（含例外）,BSD和Inner-Net以及ISC和公共域和GFDL
說明：glibc套件包含標準程式庫，由：系統上的多個程式。 為了節省磁碟空間和：記憶體，以及使升級更加容易，通用的系統代碼是：集中存放，並在程式間共用。 此特定套件：包含最重要的共用程式庫集：標準C:程式庫和標準數學程式庫。 沒有這兩個資料庫，a:Linux系統無法正常工作。

## 一些有用的yum命令

已安裝Yum List
yum search [part_of_package_name]
yum whatprovies [package_name]
yum install [package_name]
yum reinstall [package_name]
yum info [package_name]
yum deplist [package_name]
yum remove [package_name]
yum check-update [package_name]
yum update [package_name]
