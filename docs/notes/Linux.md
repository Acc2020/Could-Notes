# Linux 系统

 [TOC] 

<!-- GFM-TOC -->
* [GFM-TOC]


<!-- GFM-TOC -->


## 一般常用的命令
**主机名修改** <br >
1、临时修改
```shell
$ hostname acc2020
```
2、永久修改     
CentOS 系统为例，需要更改配置文件生效，修改 /etc/sysconfig/network 里的 HOSTNAME=主机名(可自定义)，重启生效。  
Ubuntu 系统，则需要修改文件 /etc/hostname， 将其对应的主机名修改为新的主机名。  
 
注意，修改完成后需要将 /etc/hosts 中 127.0.0.1 对应的老主机名更换为新的主机名

## 用户及文件权限管理




**常用命令**

|  需求 |  命令  |  命令参数 |
| --- | --- | --- |
|   增加用户  | sudo adduser xxx |  |
| 删除用户|  sudo  deluser xxx  --remove-home | 
| 增加 sudo 权限| sudo usermod -G sudo lilei |



## 文件目录
一种不同是体现在目录与存储介质（磁盘，内存，DVD 等）的关系上，以往的 Windows 一直是以存储介质为主的，主要以盘符（C 盘，D 盘...）及分区来实现文件管理，然后之下才是目录，目录就显得不是那么重要，除系统文件之外的用户文件放在任何地方任何目录也是没有多大关系。所以通常 Windows 在使用一段时间后，磁盘上面的文件目录会显得杂乱无章（少数善于整理的用户除外吧）。然而 UNIX/Linux 恰好相反，UNIX 是以目录为主的，Linux 也继承了这一优良特性。 Linux 是以树形目录结构的形式来构建整个系统的，可以理解为树形目录是一个用户可操作系统的骨架。虽然本质上无论是目录结构还是操作系统内核都是存储在磁盘上的，但从逻辑上来说 Linux 的磁盘是“挂在”（挂载在）目录上的，每一个目录不仅能使用本地磁盘分区的文件系统，也可以使用网络上的文件系统。举例来说，可以利用网络文件系统（Network File System，NFS）服务器
载入某特定目录等。
### 创建
**创建空白文件**
```shell
$ cd /home/root
touch test
```

**创建空白文件夹**
使用 mkdir （make directories）命令可以创建空目录，也可以同时指定创建目录的权限。
```bash
$ mkdir mydir
```

参数及其意义
| 参数 | 意义 |
| --- | --- | 
| -p | 创建多级目录时如果不含有该夫目录同时创建多级目录 | 

### cp
复制文件。如果源文件有两个以上，目的文件需要是目录

```shell
cp [-adfilprsu] source destination
-a ：相当于 -dr --preserve=all
-d ：若来源文件为链接文件，则复制链接文件属性而非文件本身
-i ：若目标文件已经存在时，在覆盖前会先询问
-p ：连同文件的属性一起复制过去
-r ：递归复制
-u ：destination 比 source 旧才更新 destination，或 destination 不存在的情况下才复制
--preserve=all ：除了 -p 的权限相关参数外，还加入 SELinux 的属性, links, xattr 等也复制了

```
### rm
删除命令 rm （remove files or directories）删除文件
```shell
rm [-fiIrdv] source destination
-f : 删除一些只读权限的文件忽略提示强制删除
-i ：
-r/R ： 删除目录
-d ：删除空文件，递归删除
-v ：解释做了什么 

```

### mv
mv （move or rename files）命令移动文件（剪切）。
```shell
mv []
 source destination to dest


```

### cat / nl

 ```
 nl
 -b : 指定添加行号的方式，主要有两种：
    -b a:表示无论是否为空行，同样列出行号("cat -n"就是这种方式)
    -b t:只列出非空行的编号并列出（默认为这种方式）
-n : 设置行号的样式，主要有三种：
    -n ln:在行号字段最左端显示
    -n rn:在行号字段最右边显示，且不加 0
    -n rz:在行号字段最右边显示，且加 0
-w : 行号字段占用的位数(默认为 6 位)
 ```

### df
**磁盘分区显示命令**
```shell
df -lh # 显示磁盘分区，包括挂宅的创建一个目录
df -TH # 增加能够显示分区种类，别的列和 lh 一样
```
### du
**查看文件大写信息** 
```shell
du -sh 目录名 # 显示该目录的所有文件大小
```
### free
**系统空闲状态**

### 搜索命令 
whereis，which，find 和 locate 。

- **whereis 简单快速**   
这个搜索很快，因为它并没有从硬盘中依次查找，而是直接从数据库中查询。whereis 只能搜索二进制文件(-b)，man 帮助文件(-m)和源代码文件(-s)。
```shell
$ whereis who
$ whereis find
```
### mkfs
**linux 现在空闲的空间 **

### mount / umount
**文件系统挂载（重启后会失效）**
```
mount -t xfs /dev/sdc1 /file #把/dev/sdc1文件系统以 xfs 类型进行挂载到 /file 文件位置 
mount -t ext4 /dev/sdc1 /file #把/dev/sdc1文件系统以 ext4 格式挂载到 /file 位置
```
umount
```shell
umount /file 卸下 /file #挂载点的文件系统
umount /dev/sdc1 #卸下/dev/sdc1 文件系统
````
**设置开机自动挂载的方式**
```shell
#编辑 
[root@acc2020 ~]# vi /etc/rc.d/rc.local
#增加
mount -t xfs /dev/sdc1 /filename

```

-  **locate 快而全**

通过“ /var/lib/mlocate/mlocate.db ”数据库查找，不过这个数据库也不是实时更新的，系统会使用定时任务每天自动执行 updatedb 命令更新一次，所以有时候你刚添加的文件，它可能会找不到，需要手动执行一次 updatedb 命令（在我们的环境中必须先执行一次该命令）。它可以用来查找指定目录下的不同文件类型，如查找 /etc 下所有以 sh 开头的文件：


    注意，它不只是在 /bin 目录下查找，还会自动递归子目录进行查找。

查找 /usr/share/ 下所有 jpg 文件：
```shell
$ locate /usr/share/\*.jpg

    注意要添加 * 号前面的反斜杠转义，否则会无法找到。
```

如果想只统计数目可以加上 -c 参数，-i 参数可以忽略大小写进行查找，whereis 的 -b、-m、-s 同样可以使用。

 **which 小而精**

which 本身是 Shell 内建的一个命令，我们通常使用 which 来确定是否安装了某个指定的软件，因为它只从 PATH 环境变量指定的路径中去搜索命令：
```shell

$ which man
```
   **find 精而细**

find 应该是这几个命令中最强大的了，它不但可以通过文件类型、文件名进行查找而且可以根据文件的属性（如文件的时间戳，文件的权限等）进行搜索。find 命令强大到，要把它讲明白至少需要单独好几节课程才行，我们这里只介绍一些常用的内容。

这条命令表示去 /etc/ 目录下面 ，搜索名字叫做 interfaces 的文件或者目录。这是 find 命令最常见的格式，千万记住 find 的第一个参数是要搜索的地方：
```shell
$ sudo find /etc/ -name interfaces

    注意 find 命令的路径是作为第一个参数的， 基本命令格式为 find [path] [option] [action] 。
```
与时间相关的命令参数：
|参数 |	说明|
|---|---|
|-atime |	最后访问时间
|-ctime |	最后修改文件内容的时间
|-mtime |    最后修改文件属性的时间

```shell
 mtime
    -mtime n：n 为数字，表示为在 n 天之前的“一天之内”修改过的文件
    -mtime +n：列出在 n 天之前（不包含 n 天本身）被修改过的文件
    -mtime -n：列出在 n 天之内（包含 n 天本身）被修改过的文件
    -newer file：file 为一个已存在的文件，列出比 file 还要新的文件名
```

### 服务/进程
**service**
```shell
service
service 服务 start  #启动服务
service 服务 restart # 服务重启
service 服务 stop # 停止服务 等同于 kill -9 PID

```
**chkconfig**  
服务管理命令类似于Win中的进程管理系统
```shell
chkconfig --list  # 显示目前的所有服务
chkconfig --add 服务名  # 增加服务
chkconfig --del 服务命  # 删除服务
chkconfig 服务名 on  # 设置服务程序开机自动启动
chkconfig 服务名 off # 取消服务程序开机自动启动
```
**crontab**
定时任务配置
```shell
crontab -l #显示定时任务
crontab -e #编辑配置定时任务
```
#配置定时任务脚本格式
|Minute|hour|day|mouth|week|command|
|---|---|---|---|---|---|
|0-59|0-23|1-31|1-12|0-7|脚本位置和名称|
**常用于数据库文件中的自动备份任务管理**


## 文件打包解压缩

Linux 上常见的压缩格式
|文件后缀名 |	说明|
| --- | --- |
|*.zip |	zip 程序打包压缩的文件 | 
|*.rar |	rar 程序压缩的文件 | 
|*.7z |	7zip 程序压缩的文件  |
|*.tar |	tar 程序打包，未压缩的文件  |
|*.gz |	gzip 程序（GNU zip）压缩的文件 | 
|*.xz |	xz 程序压缩的文件   |
|*.bz2 |	bzip2 程序压缩的文件  |
|*.tar.gz| 	tar 打包，gzip 程序压缩的文件 | 
|*.tar.xz |	tar 打包，xz 程序压缩的文件  |
|*tar.bz2 |	tar 打包，bzip2 程序压缩的文件|  
|*.tar.7z |	tar 打包，7z 程序压缩的文件|  

## 编译安装（rmp / yum）
常见的两种直接安装方式
- rmp（对应的是包）
```shell
 rmp（RPM Pacakage Manager）
    -rpm -l 列表显示
    -rpm -a 显示所有的安装的包
    -rpm -q 查询(主要是配 -a 和 -l 使用 能够查到安装位置)
    -i filename 安装
    -U 升级
    -F 升级
    -e PACKAGE_NAME 卸载
    -ivh package1.rpm 安装指定安装包，并解决安装依赖
    --nodeps 后缀命令 跳过软件安装依赖的验证
```
- yum（对应从仓库中获取安装） 类似 C/S 结构，能够自动解决依赖关系
集群如果能够连接外网仓库配置位置 /etc/yum/yum.repos.d,可更换到国内阿里或网易的源  
集群如果不能连接外网，内部搭建挂载光盘 nginx 服务。
```shell
yum
    -yum search filename #查找文件名称
    -yum install filename #安装文件
    -yum update #升级更新所有系统和内核中的包
    -yum upgrate #操作系统中的包进行更新，内核不更新
    -yum makecache #设置缓存，元数据
    -yum clean all #清楚缓存
    -yum remove filename #删除卸载已经安装过的包
    -yum repolist #查看仓库中包数量
    -yum grouplist/ 
```
**编译安装**  
对于模块化的应用安装包，可能需要自行编译安装。
1. 安装的话先查看是否有配置文件 MakeFile 文件
2. 如果有的话进行编译命令：make
3. 最后进行安装： make install

如果没有 MakeFile 文件需要查看 READEME 文件根据操作要求进行命令编辑。
安装好对于依赖后和编译环境检查后，会产生 MakeFile 文件,再使用上面的步骤进行编译(命令映射到MakeFile文件中))


![Linux-make.png](http://images.vsnode.com/Linux-make.png)



### Linux 防火墙
RHEL7 中有多种防火墙共存：firewalld、iptables、 ebtables， 默认是使用 firewalld 来管理netfilter 子系统，底层命令任然是 iptables
- firewalld 可以动态的修改单条规则，不需要像 iptables 修改后需要全部刷新才能生效。
- firewalld 默认是拒绝，iptables 默认是允许，需要做拒绝限制才能生效。

#### iptables 防火墙命令
查看防火墙状态
```bash
    service iptables status 
```
停止防火墙
```bash
    service iptables stop
```

启动防火墙
```bash
    service iptables start
```
重启防火墙
```bash
    service iptables restart
```
永久关闭防火墙
```bash
    chkconfig iptables off
```
永久关闭后重启
```bash
    chkconfig iptables on
```
开启 80 端口
```bash
    vim /etc/sysconfig/iptables
    # 增加diamagnetic
    -A INPUT -m state --state NEW -m tcp -p tcp -dport 80 -j ACCEPT
```
保存退出后重启防火墙
```bash
    service iptables restart
```

#### firewalld 防火墙
查看 firewalld 服务状态

```bash
    systemctl status firewalld
```
产看 firewalld 的状态
```bash
    firewall-cmd -state
```
开启、重启、关闭、 firewalld.service 服务
```bash
#开启
    service firewalld start
#重启
    service firewalld restart
#关闭
    service firewalld stop
```
查看防火墙规则
```bash
    firewalld-cmd --list-all
```
查看、开放、关闭端口
```bash
    #产看端口是否开放
    firewall-cmd --query-port=8080/tcp
    #开放 80 端口
    firewall-cmd --permanet --add-port=80/tcp
    #移除端口
    firewall-cmd --permanet --remove-port=8080/tcp
    #重启防火墙（修改配置后需要重启）
    firewall-cmd --reload
```
查看防火墙规则
- firewall-cmd： 是 Linux 提供的操作 firewall 的工具
- --permanet： 表示设置为持久化
- --add-port： 标识添加的端口
常用网络端口
- netstat -nlp 产看所有端口的监听情况















