
- java环境配置      
https://blog.csdn.net/weixx3/article/details/80296779
待查：一台机器安装多个版本的java
- maven安装
https://blog.csdn.net/weixx3/article/details/80331538


- 解决浏览器无法访问nacos
https://blog.csdn.net/Gedulding/article/details/108928762       没有用->用mac就没问题

todo
- mac上安装idea
- server上发布配置，本地java程序获取

**20210607**
- 创建微服务    
1. 勾选spring web和openferign两个微服务的必要组件.
2. 创建root服务的pom.xml
- 安装mysql, redis
1. 安装docker
https://www.runoob.com/docker/macos-docker-install.html
```shell
$ brew install --cask --appdir=/Applications docker
```
2. docker安装mysql
- 在[docker hub]()中找到搜索mysql
```shell
$ docker pull mysql:5.7
```
- 创建mysql实例
```shell
sudo docker run -p 3306:3306 --name mysql \
-v /Users/sunxun/Documents/mydata/mysql/log:/var/log/mysql \
-v /Users/sunxun/Documents/mydata/mysql/data:/var/lib/mysql \
-v /Users/sunxun/Documents/mydata/mysql/conf:/etc/mysql \
-e MYSQL_ROOT_PASSWORD=root \
-d mysql:5.7
```
- 下载数据库连接工具        
下载了mac上的sqlpro, 但不开源需破解————>待做
- docker安装redis

[exlipse编辑Linux服务器上的文件](https://blog.csdn.net/hehuihh/article/details/80667014)