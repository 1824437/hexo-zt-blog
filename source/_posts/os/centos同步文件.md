---
title: CentOS学习
categories: os
tags: [linux, rsync]
---

## [rsync](https://www.samba.org/ftp/rsync/rsync.html) 

[参数详解](http://www.cnblogs.com/f-ck-need-u/p/7221713.html)

### 同步文件的模式
1. 检查模式
2. 同步模式

### 工作方式 3种

### 参数说明

 > 同步是**增量的! 增量的! 增量的！**，即使`-R`在目标文件夹内创建文件夹也不会将已存在的同名文件夹内容清除

**options**

* `-r` 此选项告诉rsync以递归模式拷贝目录。
	
	不带r只能同步文件，如果不带r 同步目录则不会干活，提示`skipping directory .`
	
**eg**
	
>带和不带尾`/`是有区别的。 
	
`rsync -r ./from ./to` //这样写会把from 同步到to, to会生成一个from  
`rsync -r ./from/ ./to` //这样写会把from里的所有(file,directory)同步到to
	
**options**
	
* `-R` 表示需要将源路径上的目录创建在目标文件夹内（所建文件夹的所有下级都会被增量复制)
	
**eg**  
	
`rsync -R from/abc/a.txt /to` 同步a.txt，并创建from,abc文件夹。  
**如果只复制路径的一部分则：用`.`隔开**
`rsync -R from/./abc/a.txt /to` 同步a.txt, 并创建abc文件夹。
	
	
**options**
	
* `--backup` 对目标文件夹内的**相同的文件**做备份

	备份默认使用`~`,可以使用`--suffix <symbol>`  
	`rsync -ri --backup --suffix=% from/ to`  
	也可以指定备份的路径，但路径必须存在.`--backup-dir=./to_back`
	
#### **不同主机同步**

**options**

* `-e`指定所要使用的远程shell程序，默认为ssh

	`rsync -ir -e "ssh -p 1080" from/static t.z@138.128.197.152:~/static`  
	注意: 我的web root 是`/home/www/static`，但t.z没有w权限，所以先暂存到有权限的`~`下，然后在通过`sudo rsync -ri ~/static/ /home/www/static`

**options**

* `--existing` 只更新目标端已存在的文件
* `--ignore-existing` 只更新目标端不存在的文件


