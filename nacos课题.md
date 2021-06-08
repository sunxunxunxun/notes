
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

## 环境搭建
- 创建微服务    
1. 勾选spring web和openferign两个微服务的必要组件.
2. 创建root服务的pom.xml
### 一. 安装mysql, redis
#### 1. 安装docker
https://www.runoob.com/docker/macos-docker-install.html
```shell
$ brew install --cask --appdir=/Applications docker
```
#### 2. docker安装mysql
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
#### 3. docker安装redis并启动
[官方文档](https://hub.docker.com/_/redis)
```shell
$ open /Applications/Docker.app
$ docker pull redis
# $ docker run --name some-redis -d redis
$ touch redis.conf  # 否则会被docker当成dir
$ docker run -p 6379:6379 --name redis \
-v /Users/sunxun/Documents/mydata/redis/data:/data \
-v /Users/sunxun/Documents/mydata/redis/conf/redis.conf:/etc/redis/redis.conf \
-d redis redis-server /etc/redis/redis.conf
```
- redis简单测试
```shell
$ docker exec -it redis redis-cli
127.0.0.1:6379> set a b
OK
127.0.0.1:6379> get a
"b"
127.0.0.1:6379> exit
```
redis把数据存在内存中，重启便没有a的值。——————>我的mac还是有
```shell
$ docker restart redis
$ docker exec -it redis redis-cli
127.0.0.1:6379> get a
"b"
127.0.0.1:6379> 
```
否则更改默认持久化. 在redis.conf中添加```appendonly yes```. 然后重启.
- 安装一个redis可视化客户端————待做     

### 二. 配置maven
#### 1. 设置阿里云镜像
修改maven/conf/settings.xml，添加mirror,profile标签。
使用阿里云镜像
```xml
<mirror>
    <id>nexus-aliyun</id>
    <mirrorOf>central</mirrorOf>
    <name>Nexus aliyun</name>
    <url>http://maven.aliyun.com/nexus/content/groups/public</url>
</mirror>
```
编译java1.8版本的项目
```xml
这个部分没做
```

### 三.idea配置
- 在setting里设置使用的setting.xml文件
https://www.jianshu.com/p/214baa511f2e
[exlipse编辑Linux服务器上的文件](https://blog.csdn.net/hehuihh/article/details/80667014)