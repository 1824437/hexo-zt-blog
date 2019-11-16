---
title: CentOS学习
categories: os
tags: [shell, linux]
---

## 基础知识
1. 常见缩写

	+ UID : 用户ID
	+ EUID : 用于判断对系统资源的访问权限
	+ GID : 用户所属组
	+ PID : 进程
	+ PPID : 父进程
	+ PGID : 进程所属组

2. 


## 命令大全

### [cp](#cp) [xxd](#xxd) [sed](#sed) [curl](#crul) [ssh-keygen](#ssh-keygen) [lsof](#lsof) [ps](#ps) [traceroute](#traceroute) [chmod](#chmod) [tail](#tail)



<x id="cp">

### cp
文本，目录复制

* 参数
	* -v 显示过程
	* -r 复制目录  
	`cp ./wp ./wps -r`  
	复制wp下的文件及子文件夹到 wps

<x id="xed">

### sed
在线编辑器

* 参数
	* -n 仅显示script后的结果  
		`sed -n`  
	* l 列出不能打印字符的清单  
		`sed l`
	
	
<x id="xxd">

###  xxd
作为16进制的输出

* 参数
	* -b 用二进制显示  
	`xxd -b`

<x id="crul">

###  curl 

文件传输工具

* 参数 
	* -s / -silent 静音模式，不输出任何东西。
		`curl -s https://www.baidu.com | head -1`  
		只输出第一行。  
		`curl -s https://www.baidu.com | head -1 | xxd`  
		用十六进制输出1行
		`curl -s https://www.baidu.com | head -1 | sed -n l`  
		输出1行，并输出不能打印的字符
		
	`curl '<url>' -H 'Accept: application/json' -H 'Cookie: <cookie>'`  
带cookie访问  
	
### ssh

<x id="ssh-keygen">


### ssh-keygen

生成密钥。  
免密登录：  
	1. 可以生成公私钥。rsa , rsa.pub两个文件。
	2. 登录某台需免密的机器。将rsa.pub的文件内容粘贴到.ssh

<x id="lsof">
### lsof

用于查看你进程开打的文件，打开文件的进程，进程打开的端口(TCP、UDP)。找回/恢复删除的文件

 + -c <process name> 指定进程名
 + -p <pid> 指定进程名
 + -i <condition> 指定条件

	`lsof -i 4`  查询使用第4版ip协议的进程  
	`lsof -i :80`  查询监听80端口的进程  
	`lsof -i @127.0.0.1` 查询监听本机的进程  
	`lsof -i tcp` 查询使用tcp协议的进程

<x id="ps">
### ps 

 有时候系统管理员可能只关心现在系统中运行着哪些程序，而不想知道有哪些进程在运行。由于一个应用程序可能需要启动多个进程。所以在同等情况下，进程的数 量要比程序多的多。为此从阅读方面考虑，管理员需要知道系统中运行的具体程序。要实现这个需求的话，就需要利用命令ps来帮忙。  
要对进程进行监测和控制，首先必须要了解当前进程的情况，也就是需要查看当前进程，而 ps 命令就是最基本同时也是非常强大的进程查看命令。使用该命令可以确定有哪些进程正在运行和运行的状态、进程是否结束、进程有没有僵死、哪些进程占用了过多 的资源等等。总之大部分信息都是可以通过执行该命令得到的。

**ps可显示的所有字段**

`ps -o "user,pid,args,rss"`

* user 用户名
* uid 用户号
* pid 进程号
* ppid 父进程号
* size 内存大小, Kbytes字节.
* vsize 总虚拟内存大小, bytes字节(包含code+data+stack)
* share 总共享页数
* nice 进程优先级(缺省为0, 最大为-20)
* priority(pri) 内核调度优先级
* pmem 进程分享的物理内存数的百分比
* pcpu 占用的cpu百分比
* trs 程序执行代码驻留大小
* rss 进程使用的总物理内存数, Kbytes字节
* time 进程执行起到现在总的CPU暂用时间
* stat 进程状态
* cmd(args) 执行命令的简单格式

-

* 查看 所有的进程

	`ps -A`
	
* 查看指定用户的进程

	`ps -u root`

主要应用  
`ps aux` 列表所有程序及进程

<x id="nslookup">
### nslookup 
查服务器名
`nslookup 10.86.44.130`

<x id="tail">
### tail
tail命令用于输入文件中的尾部内容。

+ -c *num* 指定尾部多少行
+ -f/--follow 显示文件追加的内容

`tail -f /tmp/access.log` 查看日志，并监控文件尾部增加 

<x id="echo">
### echo

参数含义
显示方式	意义
0	终端默认设置
1	高亮显示
4	使用下划线
5	闪烁
7	反白显示
8	不可见
前景色	背景色	颜色
30	40	黑色
31	41	红色
32	42	绿色
33	43	黃色
34	44	蓝色
35	45	紫红色
36	46	青蓝色
37	47	白色

* 要打彩色注意与串中间加空格。
`echo -e "\033[4;40;37m 黑底白字 \033[0m"`

<x id="scp">
### scp 

linux之间复制文件

`scp tao.zhu@192.168.237.21:~/a/temp.json ~`

+ -r 递归复制整个目录

	`scp -r 192.168.0.23:~/a ~/.ssh` 将192.168.0.23主机下~/a文件夹递归复现到本机的~/.ssh文件夹下

<x id="traceroute">
### traceroute

查看数据包到主机的路径

<x id="chmod">
### chmod 权限操作命令

权限基础
![](_source/chmod1.png)
第一列，权限列 ，10-12个字符组成

第1个字符：文件类型，字符含义：

![](_source/chmod2.png)

第2-4（文件所有者）  
5-7（文件所有者所在的组）  
8-10（其它人）的权限；    
三个权限字符分别代表读取，写入，执行权限。  
权限代号表  

|类型|字母|数字|
|---|---|---| 
读取|r|		4  
写入	|w|		2  
执行	|x|		1  
无权	|-|		0  
还有s,当文件被执行时，根据who参数指定的用户类型设置文件的setuid或者setgid权限。（待理解）

如一个文件，所有者有3权，其组有读权，其他人无权，应该显示
-rwxr--r--

第11个字符表示acl和selinux属性
第12个字符是个结尾符

### 查看文件夹占用的磁盘空间

`du -sh`

### adduser/useradd 创建用户

[infos](http://www.cnblogs.com/the-tops/p/5632828.html)

### mv

mv 文件改名

### 删除用户x

1. `user del x`
2. `rm -rf /home/x`
3. `rm -rf /var/spool/mail/x`

### 查看系统信息

	`rpm -qi centos-release`

### 常用检测进程命令

1.先使用ps -e | grep nginx查看是否已经启动了nginx

2.如果没有的话则按照提示，查看0.0.0.0:80端口谁占用了，使用netstat -ltunp命令