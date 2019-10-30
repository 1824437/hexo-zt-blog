---
title: CentOS学习
categories: os
tags: [Mac,OSX]
---

## 目录结构

[https://jingyan.baidu.com/article/20095761ce2b4fcb0621b453.html](https://jingyan.baidu.com/article/20095761ce2b4fcb0621b453.html)

* ~/Applications 

	包含一些只有当前用户可以使用的程序，比如我们安装了一个程序，安装时选Applications，应用程序将会默认安装到这里！
* ~/Documents 文稿
* ~/cert
* ~/etc
* ~/Libary 包括应用程序设置、预置及其它用户指定的系统资源或设置。
* ~/Public 你可以把需要与其它用户共享的文件放在这个目录中，默认状态下，这个目录可以被其它所有用户访问。
* ~/Desktop 桌面
	

### 配置文件

/etc/profile #全局配置，登录时默认读取（all users）   
/etc/paths # 全局配置，登录时默认读取（all users）  
/etc/bashrc #全局配置，bash shell执行时会读取（all users ）  
~/.bash_profile #当前用户配置，存在此文件则忽略~/.bash_login,~/.bashrc，通常该配置文件还会配置成去读取~/.bashrc。  
~/.bash_login #当login的方式执行时，并且无~/.bash_profile时，读取此文件  
~/.profile #若~/.bash_profile,~/.bash_login都不存在讲，读取此文件；  
~/.bashrc #以non-login执行时，会执行此文件  
~/.bash_logout #退出bash时，执行此文件  


### 安装app影响的目录

~/Library

~/Library/Application Support

~/Library/Application Support/CrashReporter

~/Library/Caches

~/Library/Containers

~/Library/LaunchAgents

~/Library/Preferences

~/Library/PreferencePanes


/Library
/Library/Application Support  
/Library/Extensions  
/Library/LaunchAgents  
/Library/LaunchDaemons  
/Library/PreferencePanes  
/Library/Preferences  


一般来说/etc下一般是系统全局性的公共文件目录，而/usr/local/etc下一般指代用户级的公共文件目录。

## 命令
1. osx默认应用程序来源于store或store及信任的开发者，如需"所有来源"选项，则停用Gatekeeper。  
	`sudo spctl --master-disable`  
	`sudo spctl --master-enable`
	
2. `sudo killall -HUP mDNSResponder` 

### LF, CRLF



##系统异常

```
	
	xcrun: error: invalid active developer path (/Library/Developer/CommandLineTools), missing xcrun at: /Library/Developer/CommandLineTools/usr/bin/xcrun
	Error: Failure while executing: git config --local --replace-all homebrew.private true
```

```
解决方法：
终端执行sudo xcode-select -switch /Applications/Xcode.app/Contents/Developer命令，然后再输入一次系统的密码，完成。
```
	
-

	
1. 安装iterm2

	* www.iterm2.com下载安装
	* http://iterm2colorschemes.com/ 下载配色并导入。

2. 安装nvm

	* https://github.com/creationix/nvm下载安装
	* 创建~/.bashrc文件(如果没有的话)
	* 或直接`curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash`
	* source ~/.bashrc

3. 用nvm安装nodejs
	
	每个版本下都得重新装一遍nodejs应用。
	
4. 安装brew

	`/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`  
	会自动装上command line tools for xcode
	
5. 安装git

	`brew install git`
	
6. finder显示所有文件夹

	`defaults write com.apple.finder AppleShowAllFiles -bool true;
KillAll Finder`

7. 配置git，配色方案，命令行等


	``` javascript 
	#load .bashrc
	if [ -f ~/.bashrc ];then
	 . ~/.bashrc
	fi

	#git配置
	. /usr/local/Cellar/git/2.16.2/etc/bash_completion.d/git-completion.bash #自动补全
	. /usr/local/Cellar/git/2.16.2/etc/bash_completion.d/git-prompt.sh #分支信息
	export GIT_PS1_SHOWDIRTYSTATE=1
	#带分支信息，多色提示符
	export PS1='\n\[\033[32m\]\u@\h\[\033[33m\]\w\[\033[36m\]$(__git_ps1 " (%s)")\[\033[0m\]\n\$ '
	
	#终端配色方案
	if brew list | grep coreutils > /dev/null ; then
	  PATH="$(brew --prefix coreutils)/libexec/gnubin:$PATH"
	  alias ls='ls -F --show-control-chars --color=auto'
	  eval `gdircolors -b $HOME/.dir_colors`
	fi
	
	```
	
8. 安装beyond compare 4 ,并配置到git 

	* `rm "/Users/$(whoami)/Library/Application Support/Beyond Compare/registry.dat"` 过期删除此文件
	* 配置git
	
	```
	[diff]
  	tool = bc
  [difftool]
      prompt = false
  [difftool "bc"]
  	path = /usr/local/bin/bcomp
  [merge]
  	tool = bc
  [mergetool "bc"]
  	path = /usr/local/bin/bcomp
	```
	
	
8. 安装 adobe acrobat reader
9. 安装 reeder 3 for mac(下载破解)
10. 安装 switchhosts
11. 安装 charles
12. 安装 yarn

	`brew install yarn --without-node`  
	如果安装不成功，可能是gz包下载失败(资源被重定向了)，解决:
	
	* `brew --cache`查看cache目录
	* 手动下载定向后的包放在这个文件夹。
	* brew安装时会去cache里面找包名，找到后直接安装。

13. 安装 nginx `brew install nginx`

12. 安装工程环境（node)

	* ykit
	
	
1. aria2
2. alfred


### install shadowsocks client
-

which aria2c 可以看到可执行文件在哪个目录  

ls -la /usr/local/bin/aria2c 可以看到软连接指向哪个目录

brew 安装的软件都在/usr/local/Cellar 这个目录下
	

-

## mac 命令

 * `networksetup -listallhardwareports`	列出所有接口

 * open . #打开当前目录的finder

	`open -a <app> <filename>`	用某程序打开某文件