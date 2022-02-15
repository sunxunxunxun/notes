**每次启动服务器**
#### 1. dns设置
```
$ sudo gedit /etc/resolv.conf
// 在文件种添加
nameserver 114.114.114.114
// nameserver 8.8.8.8
```
#### 2. 网络配置
因为DHCP设置失效，所以每次重新启动都需要检查静态ip是否分配成功。
```
// 第一步 查看ip分配情况
$ ip a  
$ sudo gedit /etc/network/interfaces
// 第二步 修改成静态IP配置
![static-ip-config](D:\biye\md-notes\md-pics\静态ip配置.png)
/* 
auto ens32
iface ens32 inet static
address 192.168.2.111
gateway 192.168.1.1
*/
// 第三步 重启网络验证
$ sudo /etc/init.d/networking restart
$ ping baidu.com
$ ping 随便一个ip addr
```
查看防火墙状态
```shell
$ sudo ufw status
```

**Linu常用命令**
#### 1. 查看文件夹下文件
```shell
## 查看文件数
$ ls dir -l | wc -l
## 查看前10个文件
$ ls dir | head -n 10
```
#### 2. 终端重定向
```
// w, 覆盖
$ command > file_path  
// a, 追加
$ command >> file_path
// 同时在终端显示和重定向到文件
$ command | tee file_path
```
#### 3. 拷贝文件(夹)
```
cp ~/documents/xx.txt .
cp -r ~/documents .
```
#### 4. 重启网卡
https://blog.csdn.net/marc07/article/details/62885872/
```shell
$ /etc/init.d/networking restart
```
#### 5. 进程与端口      
- 查看端口使用情况
```shell
$ netstat -apn
$ netstat -apn | grep 8848
```
- 查看所有进程
```shell
$ ps -a
$ ps -a | grep 88
$ kill <pid>
```

**docker相关**
```
// pull 并运行docker
$ sudo docker pull <image>
$ sudo docker run -i <image>
// 查看所有docker镜像信息
$ sudo docker images -a
// 启停容器
$ sudo docker start <containerId or containerName>
$ sudo docker stop <containerId or containerName>
// 删除docker镜像
$ sudo docker rmi imageId
// 删除docker容器
$ sudo docker rm containerId
$ sudo docker rm $(sudo docker ps -a -q)  // 删除所有未运行的容器
// 本机与docker互传文件
$ sudo docker cp local_path contrainerId:dest_path                
$ sudo docker cp contrainerId:dest_path local_path
```
传文件到docker遇到`unable to execute /usr/bin/docker: Argument list too long`——传输的文件过多，传文件压缩包解决

- 一个docker容器开多个终端
```shell
$ docker attach <dockerid>  ##两个终端显示同样的内容
$ docker exec -it <dockerid> /bin/bash  # 运行两个终端
```


**pip相关**
#### 1. 换源
```shell
$ pip3 install web3 -i https://pypi.tsinghua.edu.cn/simple
```
可解决ProxyError的问题

**Linux安装geth客户端（多版本场景）**
- 如果只安装一个版本，直接apt安装。
```shell
$ add-apt-repository -y ppa:ethereum/ethereum
$ apt-get update
$ apt-get install ethereum
```
会自动安装最新的geth和evm到`/usr/bin/geth`, `/usr/bin/evm`,以后会自动使用这两个可执行文件。
- 如果不同的项目对evm的版本有要求，需要分别安装不同的版本到`/usr/bin/`, 比如`/usr/bin/geth_1.8.18`  
1. 下载go-ethereum指定版本的[源码](https://github.com/ethereum/go-ethereum/releases), 并解压、编译。
```shell
$ tar -C /usr/local -xzf go1.15.linux-amd64.tar.gz
$ make all  ##需要生成evm的命令，这里不要make geth
```
2. 添加环境变量
```shell
$ sudo gedit ~/.bashrc
## 添加如下两行
export GOROOT=/usr/local/go
export PATH=$ PATH:$ GOROOT/bin
## 生效
$ source ~/.bashrc
```
3. 将geth, evm命令添加到全局
```shell
$ sudo cp <local geth path> /usr/bin/geth_x.x.x
$ sudo cp <local evm path> /usr/bin/evm_x.x.x
##具体要使用哪个版本的时候
$ sudo cp /usr/bin/geth_x.x.x /usr/bin/geth
```

**python虚拟环境相关**
创建venv的命令：`mkvirtualenv -p python3 maian`不能使用，报`No such file or dir: '/home/sunxun/python3'`.
最后用`mkvirtualenv --python=/usr/bin/python3`创建成功的.

**git相关**
- 创建git repository    
```shell
$ git init
$ git remote add origin <链接>
$ git pull origin master
$ git add *
$ git commin -m "<notes>"
$ git push origin master
```
- 提交代码但不显示contribution的解决    
https://www.cnblogs.com/calamus/p/8176084.html  
设置邮箱地址与github一致
```shell
$ git config user.name(user.email)  #查看设置
$ git config user.email <github网页setting里的邮箱地址>
```
- git proxy失效的解决
本地物理机重启后，ip地址发生变化，导致虚拟机的git proxy失效。
```shell
 ## 查询代理
$ git config --global http.proxy[https.proxy]
$ git config --local http.proxy[https.proxy]
## 设置代理
$ git config --global http.proxy[https.proxy] 192.168.1.31:1081     # ip地址是本机地址，端口需要查看，一般1080/1081
$ git config --local http.proxy[https.proxy] 192.168.1.31:1081
## 取消代理
$ git config --global --unset http.proxy[https.proxy]
$ git config --local --unset http.proxy[https.proxy]
```
local(局部代理):在git clone仓库内执行.

[解决git账号密码验证问题:Please use a personal access token instead](https://www.cnblogs.com/plpeng/p/15175315.html)


**本地服务器配置信息**

- python, python2 -> python2.7
- pip -> python2-pip
- python3 -> python3.6
- pip3 -> python3-pip


**链接自查**

[docker 安装](https://www.cnblogs.com/walker-lin/p/11214127.html)   
[docker常用操作](https://www.cnblogs.com/dwlovelife/p/11520221.html)       
[解决docker run permission denied的问题](https://blog.csdn.net/liangllhahaha/article/details/92077065)  
[github配置](https://www.linuxidc.com/Linux/2018-05/152611.htm)     
[python和pip的安装](http://www.py3study.com/Article/details/id/9434.html)   
[ssh配置](https://blog.csdn.net/future_ai/article/details/81701744)
[scp传输文件](https://www.cnblogs.com/jiangyao/archive/2011/01/26/1945570.html)     
[vscode远程连接服务器开发]()
[python virtualenv安装和使用](https://www.linuxidc.com/Linux/2019-08/160096.htm)
[latex语法](https://blog.csdn.net/qingdujun/article/details/80805613)