---
title: CentOS学习
categories: os
tags: CentOS
---

#### 资料

ip命令[http://blog.csdn.net/hunyxv/article/details/51900482](http://blog.csdn.net/hunyxv/article/details/51900482)  
centOS网络相关[http://blog.chinaunix.net/uid-26495963-id-3230810.html](http://blog.chinaunix.net/uid-26495963-id-3230810.html)


-
####查看系统版本   
`cat /etc/redhat-release`  
####查看系统网络信息  
`ip addr`  设备及状态  
`ip -s link`查看网络接口的统计数据  
####查看bash版本  
`bash -version`
####查看哪个包提供了某个命令  
`yum provides <commandname>``yum whatprovides <commandname>`    
####vi切换到命令行`ctrl+z`,再切换到vi`fg`  
####包管理器yum  

1.`yum list installed`查看已安装的包

####有线网卡连接网络 

**!!!一定要注意网线连好了** 

1. 配置文件路径`/etc/sysconfig/network-script/ifcfg-<ethernet name>`
2. 配置项`ONBOOT=yes`
3. 通过`ip addr`查看网卡mac地址，在配置文件中增加
	`HWADDR=00:0c:bd:05:4e:cc​`
2. 重启网卡服务`systemctl restart network``service network restart`
3. 设定开机启动服务`systemctl enable NetworkManager-wait-online.service`

####cenos命令行 设置字号  

修改`etc/vconsole.conf` 改字体即可  
`setfont <fontname>` 修改字体即可达到修改字号

####CentOS 7电源管理设置
配置文件: `/etc/systemd/logind.conf`  
配置项`HandleLidSwitch=lock` 合盖不休眠  
配置完后重启配置, `systemctl restart systemd-logind`  

### 安装git 
`yum install git`

### 进入休眠状态
`sudo pm-hibernate` //休眠
`sudo pm-suspend`
`sudo pm-pm-suspend-hybrid `

### 创建账号及设置权限

系统的账号文件 /etc/passwd  
用户组列表文件：/etc/group  

1. 创建账号 `adduser t.z`
2. 添加sudo仅限

	sudoers文件 /etc/sudoers  
	在root下增加一行，保存即可
	
3. 切换用户 `su <username>`
4. 修改主机名 `hostnamectl set-hostname <hostname>`
5. 查看主机信息 hostnamectl status

-

## install software
### 安装nginx [跳转](../nginx/centos安装nginx.md)
	
### 安装epel源 
`sudo yum install epel-release`

### 安装nodejs [official guide](https://nodejs.org/en/download/package-manager/#header-enterprise-linux-and-fedora)

1. `curl --silent --location https://rpm.nodesource.com/setup_9.x | sudo bash -`
2. `sudo yum -y install nodejs`

### 安装 yarn

1. `curl -sL https://dl.yarnpkg.com/rpm/yarn.repo | sudo tee /etc/yum.repos.d/yarn.repo`
2.	`sudo yum install yarn`

### install rsync

`yum install rsync`

### install shadowsocks server

1. install pip package manager `sudo yum install python-setuptools && sudo easy_install pip`
2. `pip install shadowsocks`
	
3. 配置

	* 创建`/etc/shadowsocks.json` [详见](https://morning.work/page/2015-12/install-shadowsocks-on-centos-7.html)

		```
		{
    "server": "xxx",
    "server_port": xxx,
    "password": "xxx",
    "method": "aes-256-cfb"
}
```

		**说明：**
	
		method为加密方法，可选aes-128-cfb, aes-192-cfb, aes-256-cfb, bf-cfb, cast5-cfb, des-cfb, rc4-md5, chacha20, salsa20, rc4, table  
		server_port为服务监听端口  
		password为密码，可使用密码生成工具生成一个随机密码  
		以上三项信息在配置 shadowsocks 客户端时需要配置一致，具体说明可查看 shadowsocks 的帮助文档。
	
	* 配置自启动

		创建文件 `/etc/systemd/system/shadowsocks.service`
		
		```
		[Unit]
		Description=Shadowsocks
		
		[Service]
		TimeoutStartSec=0
		ExecStart=/usr/bin/ssserver -c /etc/shadowsocks.json
		
		[Install]
		WantedBy=multi-user.target
		```

	* 手动启动

		`systemctl enable shadowsocks`  
		`systemctl start shadowsocks`
	
	* 检查服务
	
		`systemctl status shadowsocks -l`
	 
### 安装 shadowsocks client
[详见](https://brickyang.github.io/2017/01/14/CentOS-7-%E5%AE%89%E8%A3%85-Shadowsocks-%E5%AE%A2%E6%88%B7%E7%AB%AF/)


### install network tools
必须安装，否则网络命令无法执行

`yum install net-tools`

### centos systemd 服务单元设置 system.service

设置为系统服务，开机自启动。

[详见](http://blog.csdn.net/younger_china/article/details/52539522)

### 安装 Let’s Encrypt 免费证书 [official](https://certbot.eff.org/lets-encrypt/centosrhel7-nginx)

1. `sudo yum -y install yum-utils`
2. `sudo yum-config-manager --enable rhui-REGION-rhel-server-extras rhui-REGION-rhel-server-optional`
3. `sudo yum install certbot-nginx`

	注意： certbot-nginx使用的是epel源，没有安装的话先安装  
	`yum install epel-release`; 如果装了还找不到包，可能是源没打开。需要配置一下  
	打开`/etc/yum.repos.d/epel.repo` 如果`enabled=0`,说明没启用， 改成1，然后再`yum repolist`一下，发现源配置了。再做第3步的安装。
	
4. 创建证书

	`sudo certbot --nginx`， 根据向导输入相应的信息会生成文件及修改nginx
	**每三月换一次，不用改nginx配置**
	`sudo certbot --nginx certonly`

### ssh 反空闲

1. 配置`/etc/ssh/sshd_config`

	```
	？、10分钟无动作登出
	ClientAliveInterval 60 //访问间隔，秒
	ClientAliveCountMax 10 //空闲次数
	```
	
2. 重启 ssh服务

	`service sshd restart`
	
	> **幺蛾子：**　sshd command not found	
	
	搬瓦工虚机把这个服务删除了。得重新装一下。  `sudo yum install openssh-server`
	
	
### 账号管理|登录管理

1. 限制远程登录尝试密码的次数

[infos](http://www.cnblogs.com/zhming26/p/6186402.html)
	
	


### 异常：字符集异常处理
 
 ```
 ocale: Cannot set LC_CTYPE to default locale: No such file or directory
 locale: Cannot set LC_ALL to default locale: No such file or directory
 
 ```
 [参考](https://www.cyberciti.biz/faq/failed-to-set-locale-defaulting-to-c-warning-message-on-centoslinux/)
 
 1. `sudo vi /etc/profile.d/my-custom.lang.sh`创建一个脚本

	写入内容：
		 
		
		
			## US English ##
			export LANG=en_US.UTF-8
			export LANGUAGE=en_US.UTF-8
			export LC_COLLATE=C
			export LC_CTYPE=en_US.UTF-8

2. 执行 `source my=custom.lang.sh`