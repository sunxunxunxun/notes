
## 准备工作
- java环境配置      
https://blog.csdn.net/weixx3/article/details/80296779
待查：一台机器安装多个版本的java
- maven安装     
https://blog.csdn.net/weixx3/article/details/80331538


- 解决浏览器无法访问nacos       
https://blog.csdn.net/Gedulding/article/details/108928762       没有用->用mac就没问题

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
- 在[docker hub](https://hub.docker.com/?ref=login)中找到mysql
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
#### 3. docker安装redis
[官方文档](https://hub.docker.com/_/redis)
```shell
$ open /Applications/Docker.app
$ docker pull redis
## $ docker run --name some-redis -d redis
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
redis把数据存在内存中，重启便没有a的值。——————>我的mac还是有（现在已经默认持久化了
```shell
$ docker restart redis
$ docker exec -it redis redis-cli
127.0.0.1:6379> get a
"b"
127.0.0.1:6379> 
```
否则更改默认持久化. 在redis.conf中添加```appendonly yes```. 然后重启.
- ~~安装一个redis可视化客户端————待做~~    
https://www.jianshu.com/p/214baa511f2e

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
### 三.idea配置     
- 在setting里设置使用的setting.xml文件
https://www.jianshu.com/p/214baa511f2e
[exlipse编辑Linux服务器上的文件](https://blog.csdn.net/hehuihh/article/details/80667014)

### 四.创建数据库
- docker运行mysql
- 使用SQLPro Studio连接数据库(localhost就行)
- 从sql文件创建数据库       
[sql文件](https://github.com/cosmoswong/gulimall/tree/master/doc/sql)

### 五.建立后台管理系统
- 获取脚手架        
[人人开源](https://gitee.com/renrenio)中clone renren-fast(后台管理系统)和renren-fast-vue(前端)，删除.git目录后复制到mall项目目录下。
- 运行项目      
修改文件renren-fast/src/main/resources/application-dev.yml中的配置.
```xml
url: jdbc:mysql://localhost:3306/mall_admin?useUnicode=true&characterEncoding=UTF-8&serverTimezone=Asia/Shanghai
username: root
password: root
```
运行renren-fast/src/main/java/io/renren/RenrenApplication.java，可看到控制台输出信息.
最后打开浏览器，输入```localhost:8080/renren-fast```可看到访问信息.

### 六.安装前端开发环境
- 已安装好nodejs和npm
npm设置淘宝镜像
```shell
$ npm config set registry http://registry.npm.taobao.org/
```
- 安装依赖
npm install报```Not Found - GET https://registry.npm.taobao.org/@types/strip-json-comments/-/strip-json-comments-3.0.0.tgz - [not_found] document not found```
将nodejs[更换为10.16.3版本](https://www.cnblogs.com/conserdao/p/6876381.html)解决
### 七.逆向工程搭建
1. 前后端联调启动
后台运行renrenApplication, 前端输入```npm run dev```, 打开浏览器后登录系统.
2. 逆向生成代码
通过renren-generator逆向生成项目基本代码，以product为例.
- 添加renren-generator文件夹
clone renren-generator并添加到项目mall文件夹下.
mall项目pom.xml文件中加入module.
- 更改application.yml文件配置
```yml
url: jdbc:mysql://localhost:3306/gulimall_pms?useUnicode=true&characterEncoding=UTF-8&useSSL=false&serverTimezone=Asia/Shanghai
username: root
password: root
```
这里数据库名改为gulimall_pms,你想要生成的微服务对应的数据库名（商品服务
- 更改generator.properties配置
```
mainPath=com.sunxun
package=com.sunxun.mall
moduleName=product
author=sunxun
email=sunxun1116@gmail.com
tablePrefix=pms_  #表前缀
```
- 生成代码
- 运行renren-generator中的RenrenApplication程序，打开浏览器查看结果。
会有pms数据库的表格信息，勾选所有表名——生成代码——得到一个代码压缩包.

3. 添加公共依赖
- 创建mall-common 的maven module
加依赖...
```<version>0.0.1-SNAPSHOT</version>```这句话有什么错呢，没有在mall-product的pom.xml里加它，就会报```mall-common unknown```的错.
- 在mall-common中添加公共类
**mybatis-plus**
**lombok**————这个不知道为什么不能自动寻找自动补全
@Data: lombok的功能，会在编译的时候自动生成get, set方法

- 整合MyBatis-Plus      
    1). 导入依赖
    ```xml
    <dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus-boot-starter</artifactId>
        <version>3.4.3</version>
    </dependency>
    ```
    2). 配置        
        a. 配置数据源       
            - 导入数据库的驱动
            - 在application.yml配置数据源相关信息
        b. 配置MyBatis-Plus
            - 使用@MapperScan（Mapper是用来干嘛的啊
            - 告诉MyBatis-Plus, sql映射文件位置
    ```yml
    spring:
        datasource:
            username: root
            password: root
            url: jdbc:mysql://localhost:3306/gulimall_pms
            driver-class-name: com.mysql.jdbc.Driver

    mybatis-plus:
        mapper-locations: classpath:/mapper/**/*.xml
        global-config:
            db-config:
            id-type: auto   #主键自增
    ```

### 八.用nacos管理微服务
1. 在common中加入依赖
[版本对照](https://github.com/alibaba/spring-cloud-alibaba/wiki/%E7%89%88%E6%9C%AC%E8%AF%B4%E6%98%8E)
```xml
<dependencies>
    <dependency>
        <groupId>com.alibaba.cloud</groupId>
        <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
    </dependency>
</dependencies>
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-alibaba-dependencies</artifactId>
            <version>2021.1</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```
2. 注册服务<br>
- 开启服务发现功能<br>
在application中加入注解```@EnableDiscoveryClient```.
- 在app中加入nacos server地址<br>
以mall-order为例
```yml
spring:
  datasource:
    username: root
    password: root
    url: jdbc:mysql://localhost:3306/gulimall_oms
    driver-class-name: com.mysql.cj.jdbc.Driver
  cloud:
    nacos:
      discovery:
        server-addr: 127.0.0.1:8848
  application:
    name: mall-order

mybatis-plus:
  mapper-locations: classpath:/mapper/**/*.xml
  global-config:
    db-config:
      id-type: auto
server:
  port: 7000
```
- 启动nacos server<br>
以standalone模式启动
```shell
$ sh startup.sh -m standalone
```
浏览器访问```127.0.0.1:8848/nacos```, 账号密码均为nacos.就可在ServiceManagement-Service List里看到新注册的服务.

3. 微服务间的调用<br>
测试user服务调用order服务，获取自身的order信息。
利用feign做远程调用
- 在user中引入open-feign<br>
在其中引入，创建module的时候勾选了这两个，已存在。
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
```
- 编写接口<br>
编写接口，并指明这个接口需要调用的远程服务及其请求。
[接口示例]()
- 开启远程调用功能
[解决No Feign Client for loadBalancing defined的问题](https://blog.csdn.net/weixin_43556636/article/details/110653989)

遇到了一个问题```feign.RetryableException: Read timed out executing GET http://mall-order/order/order/user/list] with root cause```, 还没解决。

4. Nacos配置中心实例<br>
- 引入依赖<br>
```xml
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
</dependency>
```
- 添加nacos配置<br>
创建bootstrap.properties，并配置
```
spring.application.name=mall-order
spring.cloud.nacos.config.server-addr=127.0.0.1:8848
```
- 在nacos中添加数据配置
data-ID: appName.properties
在其中添加配置
- 动态获取配置
@RefreshScope:动态获取并刷新配置
@Value("${配置项名}):获取指定数据项

注意：如果nacos配置中心和当前应用的配置文件中配置了相同的项，优先使用配置中心的配置。

**一些细节**
1. 命名空间：配置隔离<br>
public: 保留空间，默认
- 用来做开发、测试、发布等版本等环境隔离<br>
若获取另一命名空间的同名数据，需设置：
```
spring.cloud.nacos.config.namespace=dev
```
- 为每个微服务创建命名空间<br>
2. 配置集：所有配置的集合<br>
3. 配置集ID：配置文件名<br>
4. 配置分组<br>
默认所有配置集都属于DEFAULT_GROUP
```
spring.cloud.nacos.config.group=0616
```
**每个微服务创建自己的命名空间，使用配置group区分环境(dev, test, release)**

**待解决**
- mysql数据库表中文乱码问题
- ~~运行app后访问数据```This application has no explicit mapping for /error, so you are seeing this as a fallback.```的问题~~<br>
**快捷键**
idea
opt+cmd+B: go to implementation

**链接自查**
[mybatis-plus](https://mybatis.plus/)
[maven repository](https://mvnrepository.com/)
[spring cloud](https://spring.io/projects/spring-cloud)
[github:spring-cloud-alibaba]https://github.com/alibaba/spring-cloud-alibaba
[doc:spring-cloud-alibaba](https://spring.io/projects/spring-cloud-alibaba)