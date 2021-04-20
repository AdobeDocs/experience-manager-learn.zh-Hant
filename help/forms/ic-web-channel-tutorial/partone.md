---
title: 安裝和配置Tomcat
seo-title: 安裝和配置Tomcat
description: 這是建立第一個互動式通信文檔的多步驟教程的第1部分。在本部分，我們將安裝TOMCAT並在TOMCAT中部署sampleRest.war檔案。 此WAR檔案公開的REST端點將是我們的資料源和表單資料模型的基礎。
seo-description: 這是建立第一個互動式通信文檔的多步驟教程的第1部分。在本部分，我們將安裝TOMCAT並在TOMCAT中部署sampleRest.war檔案。 此WAR檔案公開的REST端點將是我們的資料源和表單資料模型的基礎。
uuid: c6d4c74c-ea16-4c63-92c9-182d087fd88c
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 4f400c22-6c96-4018-851c-70d988ce7c6c
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '313'
ht-degree: 1%

---


# 安裝和配置Tomcat {#install-and-configure-tomcat}

在本部分，我們將安裝TOMCAT並在TOMCAT中部署sampleRest.war檔案。 此WAR檔案公開的REST端點將是我們的資料源和表單資料模型的基礎。

要設定tomcat，請按照以下說明操作：

1. 下載並安裝JDK1.8。
2. 將JAVA_HOME設定為指向JDK1.8。
3. 下載[tomcat](https://tomcat.apache.org/)。 此war檔案已通過Tomcat 8.5.x和9.0.x版的測試。
4. 下載您偏好設定的tomcat版本。 您可以在核心區段下載64位元Windows zip。
5. 將內容解壓縮至您的c:\tomcat。
6. 根據tomcat的版本，您應在c驅動器中看到類似的內容&#x200B;**c:\tomcat\apache-tomcat-8.5.27**
7. 建立名為&quot;CATALINA_HOME&quot;的環境變數，並將其值設定為tomcat安裝資料夾示例c:\tomcat\apache- tomcat-8.5.27
8. 將SampleRest.war檔案複製到Tomcat安裝的webapps檔案夾
9. 啟動新的命令提示窗口。
10. 導覽至&lt;tomcat install folder>\bin並執行startup.bat
11. 啟動tomcat後，通過[按一下這裡](http://localhost:8080/SampleRest/webapi/getStatement/9586)測試WAR檔案公開的端點
12. 您應該取得此呼叫的範例資料。

恭喜！!!.. 您已設定tomcat並部署了SampleRest.war檔案。
