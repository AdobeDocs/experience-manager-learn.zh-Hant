---
title: 安裝和配置Tomcat
seo-title: 安裝和配置Tomcat
description: 這是建立第一個互動式通信文檔的多步驟教程的第1部分。在本部分，我們將安裝TOMCAT並在TOMCAT中部署sampleRest.war檔案。 此WAR檔案公開的REST端點將是我們的資料源和表單資料模型的基礎。
seo-description: 這是建立第一個互動式通信文檔的多步驟教程的第1部分。在本部分，我們將安裝TOMCAT並在TOMCAT中部署sampleRest.war檔案。 此WAR檔案公開的REST端點將是我們的資料源和表單資料模型的基礎。
uuid: 835e2342-82b6-4f0c-9a6b-467bbbd8527a
feature: interactive-communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
thumbnail: 37815.jpg
discoiquuid: 5f68be3d-aa35-4a3f-aaea-b8ee213c87ae
translation-type: tm+mt
source-git-commit: c60a46027cc8d71fddd41aa31dbb569e4df94823
workflow-type: tm+mt
source-wordcount: '319'
ht-degree: 0%

---


# 安裝和配置Tomcat {#install-and-configure-tomcat}

在本部分，我們將安裝TOMCAT並在TOMCAT中部署sampleRest.war檔案。 此WAR檔案公開的REST端點將是我們的資料源和表單資料模型的基礎。

>[!VIDEO](https://video.tv.adobe.com/v/37815/?quality=9&learn=on)

要設定tomcat，請按照以下說明操作：

1. 下載並安裝JDK1.8。
2. 將JAVA_HOME設定為指向JDK1.8。
3. 下載 [tomcat](https://tomcat.apache.org/)。 此war檔案已通過Tomcat 8.5.x和9.0.x版的測試。
4. 下載您偏好設定的tomcat版本。 您可以在核心區段下載64位元Windows zip。
5. 將內容解壓縮至您的c:\tomcat。
6. 您應在c驅動器c:\tomcat\apache-tomcat-8.5. **27中看到類似的內容** ，具體取決於tomcat的版本
7. 建立名為&quot;CATALINA_HOME&quot;的環境變數，並將其值設定為tomcat安裝資料夾示例c:\tomcat\apache- tomcat-8.5.27
8. 將SampleRest.war檔案複製到Tomcat安裝的webapps檔案夾
9. 啟動新的命令提示窗口。
10. 導覽至&lt;tomcat install folder>\bin並執行startup.bat
11. 啟動tomcat後，按一下這裡測試WAR檔案公開的端 [點。](http://localhost:8080/SampleRest/webapi/getStatement/9586)
12. 您應該取得此呼叫的範例資料。

恭喜！!!.. 您已設定tomcat並部署了SampleRest.war檔案。

以下視訊說明在Tomcat中部署範例應用程式的方式
>[!VIDEO](https://video.tv.adobe.com/v/37815)