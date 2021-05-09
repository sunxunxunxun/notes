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

**Linu常用命令**
#### 1. 查看文件夹下文件数
```
$ ls dir -l | wc -l
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
**docker相关**
```
// pull 并运行docker
$ sudo docker pull xx
$ sudo docker run -i xx
// 查看所有docker镜像信息
$ sudo docker images -a
// 启停容器
$ sudo docker start containerId/containerName
$ sudo docker stop containerId/containerName
// 删除docker镜像
$ sudo docker rmi imageId
// 删除docker容器
$ sudo docker rm containerId
$ sudo docker rm $(sudo docker ps -a -q)  // 删除所有未运行的容器
// 本机与docker互传文件
$ sudo docker cp local_path contrainerId:dest_path,                
$ sudo docker cp contrainerId:dest_path local_path
```

**pip相关**
#### 1. 换源
```
$ pip3 install web3 -i https://pypi.tsinghua.edu.cn/simple
```

**python虚拟环境相关**
创建venv的命令：`mkvirtualenv -p python3 maian`不能使用，报`No such file or dir: '/home/sunxun/python3'`.
最后用`mkvirtualenv --python=/usr/bin/python3`创建成功的.


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