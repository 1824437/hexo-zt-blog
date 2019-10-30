---
title: CentOS学习
categories: os
tags: [启动盘, linux]
---

在Mac环境下，将Linux的ISO镜像生成一个Linux的安装盘，和Linux下差不多，只是Mac下有一些自己的工具。

步骤如下：

1、在终端下，将ISO镜像转换为DMG格式：

`$ hdiutil convert -format UDRW -o ~/linux.dmg /tmp/linux.iso`

 
2、插入USB盘，然后在终端下，查找该盘的设备名：

$ diskutil list
/dev/disk0
  #:                      TYPE NAME                    SIZE      IDENTIFIER
  0:      GUID_partition_scheme                        *121.3 GB  disk0
  1:                        EFI                        209.7 MB  disk0s1
  2:                  Apple_HFS Macintosh HD            120.5 GB  disk0s2
  3:                Apple_Boot Recovery HD            650.0 MB  disk0s3
/dev/disk1
  #:                      TYPE NAME                    SIZE      IDENTIFIER
  0:    FDisk_partition_scheme                        *15.8 GB    disk1
  1:              Windows_NTFS wxy-u3                  15.8 GB    disk1s1

以上显示该盘的设备名是： /dev/disk1
 
也可以通过“磁盘工具”里面选择磁盘（不是分区），来查看其信息，获得设备名。
 
3、卸载USB盘（插入时已经自动挂载了），但是不要推出。在终端下：

$ diskutil umountDisk /dev/disk1
Unmount of all volumes on disk1 was successful

也可以通过“磁盘工具”进行卸载。
 
4、镜像上面生成的DMG内容到USB盘，在终端下：

$ sudo dd if=linux.dmg of=/dev/rdisk1 bs=1m
Password:
2151+1 records in
2151+1 records out
2256076800 bytes transferred in 90.277905 secs (24990354 bytes/sec)

此处要千万注意，指定的of别写错了，否则悔之晚矣。另外，of参数指定的设备名，可以用上面找到的/dev/disk1，也可以用/dev/rdisk1，此处的“r”据说会写入较快。
 
另外，如果报错：“dd: Invalid number `1m'”，可能是使用的不同版本的dd，可以换为bs=1M试试。
 
如果报错：“dd: /dev/diskN: Resource busy”，可能是上面的步骤中没有完成卸载USB盘。
 
5、推出USB盘。在上面复制之后，系统可能会报错，“此电脑不难读取能插入的磁盘”，不必理会，直接推出即可。也可以在终端下推出：

$ diskutil eject /dev/disk1

或者在“磁盘工具”里面推出。

使用该USB盘引导启动即可进行Linux的安装。