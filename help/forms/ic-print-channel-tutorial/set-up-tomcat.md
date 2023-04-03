---
title: 安裝並配置Tomcat視頻
seo-title: Install and Configure Tomcat
description: 這是建立第一個互動式通訊檔案的多步驟教學課程的第1部分。
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
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '235'
ht-degree: 0%

---

# 安裝並配置Tomcat {#install-and-configure-tomcat}

在本部分，我們安裝了TOMCAT，並在TOMCAT中部署了sampleRest.war檔案。 此WAR檔案公開的REST端點是資料源和表單資料模型的基礎。

>[!VIDEO](https://video.tv.adobe.com/v/37815?quality=12&learn=on)

要設定tomcat，請按照以下說明操作：

1. 下載並安裝JDK1.8。
2. 將JAVA_HOME設定為指向JDK1.8。
3. 下載 [tomcat](https://tomcat.apache.org/). 此war檔案已使用Tomcat 8.5.x版和9.0.x版進行測試。
4. 下載您的首選項的tomcat版本。 您可以在核心區段下載64位元的windows zip。
5. 將內容解壓縮至c:\tomcat。
6. 你應該在c驅動器中看到類似的東西 **c:\tomcat\apache-tomcat-8.5.27** 取決於您的tomcat版本
7. 建立名為&quot;CATALINA_HOME&quot;的環境變數，並將其值設定為tomcat安裝資料夾示例c:\tomcat\apache- tomcat-8.5.27
8. 將SampleRest.war檔案複製到Tomcat安裝的Webapps資料夾中
9. 啟動新的命令提示窗口。
10. 導覽至 &lt;tomcat install=&quot;&quot; folder=&quot;&quot;>\bin並運行startup.bat
11. 啟動tomcat後，通過測試WAR檔案公開的端點 [按一下這裡](http://localhost:8080/SampleRest/webapi/getStatement/9586)
12. 您應該會收到此呼叫的結果範例資料。

恭喜!!!。 您已設定tomcat並部署了SampleRest.war檔案。

以下影片說明在Tomcat中部署範例應用程式
>[!VIDEO](https://video.tv.adobe.com/v/37815?quality=12&learn=on)
