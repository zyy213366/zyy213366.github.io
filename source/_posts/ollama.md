---
title: 大语言模型的本地部署分享
date: 2024-12-01 19:39:13
tags: 代码分享
description : 通过ollama+docker+webui部署本地大语言模型
cover: /img/02.jpg
---
## 大语言模型的本地部署分享
前几天用的梯子过期没续费，直接用不了chatgpt，差点耽误我交作业，因此我想本地部署一个大语言模型，满足我的使用需求
### 目标
能够成功在本地部署大语言模型，实现不通过联网使用的目的。
### 流程
1. 下载 ```ollama```
   用魔法进入 ```ollama``` 官网 https://ollama.com/ 下载 ```ollama``` 启动器，并完成安装。
   ![](/img/page_1.png)
   进入cmd，输入```ollama```来判断是否安装成功。
   ![](/img/page_2.png)
2. 通过 ```ollama``` 下载大语言模型
   进入官网下载大语言模型，由于是个人电脑，推荐选小一点的。比如9b，就是9亿参数，大概五个g。
   ![](/img/page_3.png)
   下载完成后我们可以通过命令行调用的方式完成对话，先输入```ollama run gemma2```,然后对话即可。
   ![](/img/page_4.png)
3. 下载 ```docker```，下载 ```openwebui``` 并且部署
   既然已经可以运行，我们为什么还要下载```docker```和```openwebui```呢？因为输入命令行调用的方法太过繁琐，而```docker```和```openwebui```能给我们一个整洁的页面，前者可以理解为一个后端，而后者可以理解为前端页面。

   首先我们进入```docker```下载地址https://github.com/tech-shrimp/docker_installer 在浏览器内打开，我们需要在本地**启用或关闭windows功能**中选择**适用于linux的windows子系统**和**虚拟机平台**，打勾，重启电脑。
   ![](/img/page_5.png)
   再通过管理员权限打开cmd，复制以下代码并运行：
   ```
   wsl --set-default-version 2
   wsl --update --web-download
   ```
   ![](/img/page_6.png)
   安装完成后进入页面右侧release下载 docker_desktop_installer_windows_x86_64.exe 
   
   并且双击下载完成的 ```docker``` 启动器完成docker安装。
   ![](/img/page_7.png)
   之后下载 ```openwebui``` ，进入网址 https://github.com/open-webui/open-webui ，在docker开启的条件下在cmd中运行以下命令
    ```
    docker run -d -p 3000:8080 --add-host=host.docker.internal:host-gateway -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:main
    ```
   安装完成后在```docker```中就能找到端口号进行访问。
   ![](/img/page_8.png)
