---
title: 安裝和配置Tomcat視頻
seo-title: Install and Configure Tomcat
description: 這是用於建立第一個互動式通信文檔的多步驟教程的第1部分。在本部分中，我們將安裝TOMCAT並在TOMCAT中部署sampleRest.war檔案。 此WAR檔案所暴露的REST端點將是我們的資料源和表單資料模型的基礎。
seo-description: This is part 1 of multistep tutorial for creating your first interactive communications document.In this part, we will install TOMCAT and deploy the sampleRest.war file in TOMCAT. The REST endpoint exposed by this WAR file will be the basis for our Data Source and Form Data Model.
uuid: 835e2342-82b6-4f0c-9a6b-467bbbd8527a
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
thumbnail: 37815.jpg
discoiquuid: 5f68be3d-aa35-4a3f-aaea-b8ee213c87ae
topic: Development
role: Developer
level: Beginner
exl-id: faa9ca2d-6cfa-4abf-be5e-3e549202853a
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# 安裝和配置Tomcat {#install-and-configure-tomcat}

在本部分，我們將安裝TOMCAT並在TOMCAT中部署sampleRest.war檔案。 此WAR檔案所暴露的REST端點將是我們的資料源和表單資料模型的基礎。

>[!VIDEO](https://video.tv.adobe.com/v/37815/?quality=9&learn=on)

要設定tomcat，請按照以下說明操作：

1. 下載並安裝JDK1.8。
2. 將JAVA_HOME設定為指向JDK1.8。
3. 下載 [tomcat](https://tomcat.apache.org/)。 已使用Tomcat 8.5.x和9.0.x版對該戰爭檔案進行了測試。
4. 下載首選項的tomcat版本。 您可以下載核心部分下的64位windows zip。
5. 將內容解壓縮到c:\tomcat。
6. 你應該在你的c驅動器中看到這樣的東西 **c:\tomcat\apache-tomcat-8.5.27** 取決於tomcat的版本
7. 建立名為&quot;CATALINA_HOME&quot;的環境變數，並將其值設定為tomcat安裝資料夾示例c:\tomcat\apache- tomcat-8.5.27
8. 將SampleRest.war檔案複製到Tomcat安裝的webapps資料夾
9. 啟動新命令提示窗口。
10. 導航到 &lt;tomcat install=&quot;&quot; folder=&quot;&quot;>\bin並運行startup.bat
11. 啟動tomcat後，testWAR檔案所暴露的端點， [按一下這裡](http://localhost:8080/SampleRest/webapi/getStatement/9586)
12. 您應獲得此調用的示例資料。

恭喜你！!!. 您已安裝tomcat並部署了SampleRest.war檔案。

以下視頻說明了在Tomcat中部署示例應用程式
>[!VIDEO](https://video.tv.adobe.com/v/37815)
