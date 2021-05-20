---
title: 在Linux上安裝AEM Forms
description: 了解如何安裝32位元程式庫，以便AEM Forms在Linux安裝上運作。
feature: 適用性表單
audience: developer
doc-type: article
activity: setup
version: 6.4, 6.5
topic: 開發
role: Developer
level: Beginner
kt: 7593
source-git-commit: 9583006352ca6a20a763c9d5ec7ba15c3791e897
workflow-type: tm+mt
source-wordcount: '948'
ht-degree: 0%

---


# 安裝32位版共用程式庫

當AEM FORMS.在Linux上部署OSGi或AEM Forms j2EE時，您必須確保安裝並提供一組共用庫的32位版本。  說明來自套件本身。

* expat（由James Clark編寫的面向流的XML解析器C庫，用於解析XML）
* fontconfig（字型配置和定制庫，設計為在系統內查找字型，並根據應用程式指定的要求選擇字型）
* freetype(字型轉譯引擎，專為各種平台和環境提供進階字型支援而開發。 它可以開啟和管理字型檔案，並有效地載入、提示和渲染單個字形。 它不是字型伺服器或完整的文本呈現庫)
* glibc（用於GNU系統和GNU/Linux系統的核心庫，以及許多以Linux為內核的其他系統）
* libcurl（用戶端URL傳輸程式庫）
* libICE（客戶端間Exchange庫）
* libicu（提供強大且功能完整的Unicode和區域設定支援的庫 — Unicode的國際元件）。 此程式庫需要64位元和32位元版本
* libSM（X11會話管理庫）
* libuuid（與DCE相容的通用唯一標識符庫 — 用於為可在本地系統之外訪問的對象生成唯一標識符）
* libX11（X11用戶端程式庫）
* libXau（X11授權通訊協定 — 可用於限制用戶端對顯示器的存取）
* libxcb（X協定C語言綁定 — XCB）
* libXext（X11通訊協定的常用擴充功能的程式庫）
* libXinerama(X11擴充功能，支援將案頭擴展到多個顯示器。 Cinerama是一個使用多台放映機的寬屏電影格式，它的名字是雙關語。 libXinerama是與RandR擴展介面的庫)
* libXrandr（Xinerama擴展現在已基本過時 — 它已被RandR擴展替換）
* libXrender（X呈現擴展客戶端庫）
nss-softokn-freebl（網路安全服務的Freebl庫）
* zlib（通用、無專利、無損資料壓縮庫）

從Red Hat Enterprise Linux 6開始，32位版本的庫的副檔名為。686，而64位版本的庫的副檔名為.x86_64。 例如expat.i686。 在RHEL 6之前，32位元版本的副檔名為.i386。 安裝32位版本之前，請確定已安裝最新的64位版本。 如果64位元版本的程式庫早於所安裝的32位元版本，您會收到下列錯誤：

0m錯誤：受保護的多庫版本：libsepol-2.5-10.el7.x86_64 != libsepol-2.5-6.el7.i686 [0mError:發現多庫版本問題。]

## 首次安裝

在Red Hat Enterprise Linux上，使用YellowDog Update修飾符(YUM)進行安裝，如下所示：

1. yum install expat.i686
2. yum install fontconfig.i686
3. yum install freetype.i686
4. yum install glibc.i686
5. yum install libcurl.i686
6. yum install libICE.i686
7. yum install libicu.i686
8. 百勝安裝
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

此外，您還需要建立libcurl.so、libcrypto.so和libssl.so符號連結，分別指向libcurl、libcrypto和libssl庫的最新32位版本。 您可以在/usr/lib/中找到檔案
ln -s /usr/lib/libcurl.so.4.5.0 /usr/lib/libcurl.so
ln -s /usr/lib/libcrypto.so.1.1.1c /usr/lib/libcrypto.so
ln -s /usr/lib/libssl.so.1.1.1c /usr/lib/libssl.so

## 更新現有系統

在更新期間，x86_64和i686體系結構之間可能存在衝突，例如：
錯誤：事務檢查錯誤：
從glibc-2.28-72.el8.i686安裝的檔案/lib/ld-2.28.so與包glibc32-2.28-42.1.el8.x86_64的檔案衝突

如果您遇到此問題，請先卸載違規包，如下所示：
yum remove glibc32-2.28-42.1.el8.x86_64

所有說明和完成，您希望x86_64和i686版本完全相同，例如從此輸出到命令：
yom info glibc

上次元資料過期檢查：0:41:33年1月18日星期六東11:37:08午
已安裝的軟體包
名稱：glibc
版本：2.28
版本：72.el8
架構：i686
大小：13米
來源：glibc-2.28-72.el8.src.rpm
存放庫：@System
從存放庫：BaseOS
摘要：GNU libc庫
URL :http://www.gnu.org/software/glibc/
授權：LGPLv2+和LGPLv2+，帶有異常，GPLv2+和GPLv2+帶有異常，BSD和內網以及ISC和公共域以及GFDL
說明：glibc套件包含所使用的標準程式庫：系統上的多個程式。 為了節省磁碟空間和：記憶體，為了讓升級更輕鬆，常見的系統程式碼為：在一個地方保持，並在各方案之間共用。 此特定套件：包含最重要的共用程式庫集：標準C :程式庫和標準數學程式庫。 若沒有這兩個資料庫，即為：Linux系統無法運行。

名稱：glibc
版本：2.28
版本：72.el8
架構：x86_64
大小：15米
來源：glibc-2.28-72.el8.src.rpm
存放庫：@System
從存放庫：BaseOS
摘要：GNU libc庫
URL :http://www.gnu.org/software/glibc/
授權：LGPLv2+和LGPLv2+，帶有異常，GPLv2+和GPLv2+帶有異常，BSD和內網以及ISC和公共域以及GFDL
說明：glibc套件包含所使用的標準程式庫：系統上的多個程式。 為了節省磁碟空間和：記憶體，為了讓升級更輕鬆，常見的系統程式碼為：在一個地方保持，並在各方案之間共用。 此特定套件：包含最重要的共用程式庫集：標準C :程式庫和標準數學程式庫。 若沒有這兩個資料庫，即為：Linux系統無法運行。

## 一些有用的yum命令

已安裝
yum search [part_of_package_name]
yum whatprovids [package_name]
yum install [package_name]
yum reinstall [package name]
yum info [package_name]
yum deplist [package_name]
yum remove [package_name]
yum check-update [package_name]
yum update [package_name]
