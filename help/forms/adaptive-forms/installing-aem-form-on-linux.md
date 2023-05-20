---
title: 在Linux上安裝AEM Forms
description: 瞭解如何為AEM Forms安裝32位庫以在Linux安裝上工作。
feature: Adaptive Forms
type: Tutorial
version: 6.4, 6.5
topic: Development
role: Developer
level: Beginner
kt: 7593
exl-id: b9809561-e9bd-4c67-bc18-5cab3e4aa138
last-substantial-update: 2019-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '945'
ht-degree: 0%

---

# 正在安裝32位版本的共用庫

當AEM FORMS。在Linux上部署OSGi或AEM Formsj2EE時，必須確保已安裝並可用一組共用庫的32位版本。  說明來自包本身。

* expat（面向流的XML解析器C庫，用於解析XML，由James Clark編寫）
* fontconfig（字型配置和自定義庫，設計為在系統內查找字型，並根據應用程式指定的要求選擇它們）
* freetype(字型呈現引擎，開發為為各種平台和環境提供高級字型支援。 它可以開啟和管理字型檔案，並可以高效地載入、提示和呈現單個字形。 它不是字型伺服器或完整的文本呈現庫)
* glibc（用於GNU系統和GNU/Linux系統的核心庫，以及許多使用Linux作為內核的其他系統）
* libcurl（客戶端URL傳輸庫）
* libICE（客戶端間Exchange庫）
* libicu（提供強大且功能齊全的Unicode和區域設定支援的庫 — International Components for Unicode）。 此庫的64位和32位版本都是必需的
* libSM（X11會話管理庫）
* libuuid（DCE相容的通用唯一標識符庫），用於為可在本地系統之外訪問的對象生成唯一標識符。
* libX11（X11客戶端庫）
* libXau（X11授權協定 — 用於限制客戶端訪問顯示器）
* libxcb（X協定C語言綁定 — XCB）
* libXext（X11協定的常用擴展庫）
* libXinerama(X11擴展，支援跨多個顯示器擴展案頭。 這個名稱是Cinerama的雙關語，Cinerama是一種使用多個放映機的寬屏電影格式。 libXinerama是與RandR擴展介面的庫)
* libXrandr（Xinerama擴展現在已基本過時 — 它已被RandR擴展所取代）
* libXrender（X呈現擴展客戶端庫）nss-softokn-freebl（用於網路安全服務的Freebl庫）
* zlib（通用、無專利、無損資料壓縮庫）

從Red Hat Enterprise Linux 6開始，庫的32位版本的檔案副檔名為。686，而64位版本的副檔名為.x86_64。 例如， expat.i686。 在RHEL 6之前，32位版本的副檔名為.i386。 在安裝32位版本之前，請確保已安裝最新的64位版本。 如果庫的64位版本早於正在安裝的32位版本，則會出現以下錯誤：

0m錯誤：受保護的多庫版本：libsepol-2.5-10.el7.x86_64 != libsepol-2.5-6.el7.i686 [0m錯誤：發現多庫版本問題。]

## 首次安裝

在Red Hat Enterprise Linux上，使用YellowDog更新修飾符(YUM)進行安裝，如下所示：

1. yum安裝expat.i686
2. yum安裝fontconfig.i686
3. yum安裝freetype.i686
4. yum安裝glibc.i686
5. yum安裝libcurl.i686
6. yum安裝libICE.i686
7. yum安裝libicu.i686
8. 百勝安裝
9. yum安裝libSM.i686
10. yum安裝libuuid.i686
11. yum安裝libX11.i686
12. yum安裝libXau.i686
13. yum安裝libxcb.i686
14. yum安裝libXext.i686
15. yum安裝libXinerama.i686
16. yum安裝libXrandr.i686
17. yum安裝libXrender.i686
18. yum install nss-softokn-freebl.i686
19. yum安裝zlib.i686

## 符號連結

此外，您還需要建立libcurl.so、libcrypto.so和libssl.so符號連結，它們分別指向libcurl、libcrypto和libssl庫的最新32位版本。 可以在/usr/lib/ ln -s /usr/lib/libcurl.so.4.5.0 /usr/lib/libcurl.so ln -s /usr/lib/libcrypto.so.1.1.1c /usr/lib/libcrypto.so ln -s /usr/lib/libssl.so.1.1.1c /usr/lib/libssl.so中找到檔案

## 對現有系統的更新

在更新期間，x86_64和i686體系結構之間可能存在衝突，例如：錯誤：事務檢查錯誤：檔案/lib/ld-2.28.so（安裝glibc-2.28-72.el8.i686）與軟體包glibc32-2.28-42.1.el8.x86_64中的檔案衝突

如果遇到此問題，請先卸載違規軟體包，如下所示：yum remove glibc32-2.28-42.1.el8.x86_64

所有說完，您都希望x86_64和i686版本完全相同，例如，從以下輸出到命令：百勝資訊

上次元資料過期檢查：0:41:33年1月18日:37:東部時間上午08點
已安裝的包名稱：glibc版本：2.28版本：72.el8體系結構：i686大小：13 M來源：glibc-2.28-72.el8.src.rpm儲存庫：@System從回購：BaseOS摘要：GNU libc庫URL :http://www.gnu.org/software/glibc/許可證：LGPLv2+和LGPLv2+（有例外）以及GPLv2+和GPLv2+（有例外）以及BSD和Inner-Net和ISC以及公共域和GFDL說明：glibc包包含標準庫，它們由：系統上有多個程式。 為了節省磁碟空間和：記憶體，以及使升級更容易，通用系統代碼是：在一個地方存放，程式之間共用。 此特定包：包含最重要的共用庫集：標準C:庫和標準數學庫。 沒有這兩個庫，a :Linux系統無法正常工作。

名稱：glibc版本：2.28版本：72.el8體系結構：x86_64大小：15 M來源：glibc-2.28-72.el8.src.rpm儲存庫：@System從回購：BaseOS摘要：GNU libc庫URL :http://www.gnu.org/software/glibc/許可證：LGPLv2+和LGPLv2+（有例外）以及GPLv2+和GPLv2+（有例外）以及BSD和Inner-Net和ISC以及公共域和GFDL說明：glibc包包含標準庫，它們由：系統上有多個程式。 為了節省磁碟空間和：記憶體，以及使升級更容易，通用系統代碼是：在一個地方存放，程式之間共用。 此特定包：包含最重要的共用庫集：標準C:庫和標準數學庫。 沒有這兩個庫，a :Linux系統無法正常工作。

## 一些有用的yum命令

Yum清單安裝的Yum搜索 [part_of_package_name]
百勝 [軟體包名稱]
yum安裝 [軟體包名稱]
yum重新安裝 [軟體包名稱]
百勝資訊 [軟體包名稱]
百勝 [軟體包名稱]
刪除 [軟體包名稱]
yum檢查更新 [軟體包名稱]
yum更新 [軟體包名稱]
