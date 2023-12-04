---
title: 安裝和設定Tomcat
description: 這是建立第一個互動式通訊檔案的多步驟教學課程的第1部分。在本部分中，我們將安裝TOMCAT並在TOMCAT中部署sampleRest.war檔案。
feature: Interactive Communication
doc-type: Tutorial
version: 6.4,6.5
discoiquuid: 4f400c22-6c96-4018-851c-70d988ce7c6c
topic: Development
role: Developer
level: Beginner
exl-id: f0f19838-1ade-4eda-b736-a9703a3916c2
duration: 66
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '240'
ht-degree: 0%

---

# 安裝和設定Tomcat {#install-and-configure-tomcat}

在本部分中，我們會安裝TOMCAT並在TOMCAT中部署sampleRest.war檔案。 此WAR檔案公開的REST端點是我們資料來源和表單資料模型的基礎。

若要設定tomcat，請遵循下列指示：

1. 下載並安裝JDK1.8。
2. 將JAVA_HOME設定為指向JDK1.8。
3. 下載 [tomcat](https://tomcat.apache.org/). 此War檔案已經過Tomcat 8.5.x和9.0.x版的測試。
4. 下載您偏好設定的tomcat版本。 您可以在核心區段底下下載64位元windows zip。
5. 將內容解壓縮至您的c：\tomcat。
6. 您應該會在C磁碟機中看到類似這樣的內容 **c：\tomcat\apache-tomcat-8.5.27** 視您的tomcat版本而定
7. 建立名為「CATALINA_HOME」的環境變數，並將其值設定為tomcat安裝資料夾範例c：\tomcat\apache- tomcat-8.5.27
8. 將SampleRest.war檔案複製到Tomcat安裝的webapps資料夾中
9. 啟動新的命令提示字元視窗。
10. 瀏覽至 &lt;tomcat install=&quot;&quot; folder=&quot;&quot;>\bin並執行startup.bat
11. 在您的tomcat啟動後，請測試WAR檔案公開的端點，方法為 [按一下這裡](http://localhost:8080/SampleRest/webapi/getStatement/9586)
12. 您應該會從此呼叫取得範例資料。

恭喜!!!。 您已設定tomcat並部署SampleRest.war檔案。

## 後續步驟

[設定RESTful資料來源](./parttwo.md)
