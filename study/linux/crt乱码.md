# [SecureCRT中文显示乱码](https://www.cnblogs.com/totozlj/archive/2012/09/01/2666804.html)



环境：SecureCRT登陆REDHAT5.3 LINUX系统

问题：vi编辑器编辑文件时文件中的内容中文显示乱码，但是直接使用linux系统terminal打开此文件时中文显示正常，确诊问题出现在客户端即SecureCRT的显示问题

解决方法：

1、修改远程linux机器的配置 

[root@rhel ~]#vi /etc/sysconfig/i18n  

把LANG改成支持UTF-8的字符集 
如： LANG=”zh_CN.UTF-8″ 或者是 LANG=”en_US.UTF-8″  本文修改为后者

2、修改Secure CRT的Session Options

Options->Session Options->Appearance->Font->新宋体 字符集：中文GB2312 ->Character encoding 为UTF-8

3、OK.

 

SecureFX登陆中文乱码

#### 问题描述

SecureCRT与SecureFX的常规选项里面已经设置成了UTF-8，但是在SecureCRT中新建的中文文件夹，在SecureFX里面仍是乱码，这个问题，找了很多的方法，最后还是解决了，在这里和大家分享下。

#### 查看服务器编码

查看linux的编码，修改为自己需要的，本文将已UTF-8为例进行说明。
修改Linux服务器的配置文件：
[root@iitshare ~]# vi /etc/sysconfig/i18n
如果安装系统的时候选择了中文系统，则把LANG字段改为：
LANG="zh_CN.UTF-8"
如果安装系统的时候选择的英文系统，则把LANG字段改为：
LANG="en_US.UTF-8"

 

#### 一般解决办法

\1. 右键点击SecureCRT的连接标签。
[![SecureCRT中文乱码设置](http://www.iitshare.com/wp-content/uploads/2013/07/SecureCRT%E4%B8%AD%E6%96%87%E4%B9%B1%E7%A0%81%E8%AE%BE%E7%BD%AE.png)](http://www.iitshare.com/wp-content/uploads/2013/07/SecureCRT%E4%B8%AD%E6%96%87%E4%B9%B1%E7%A0%81%E8%AE%BE%E7%BD%AE.png)
\2. 在弹出的窗口中，左边栏选择“外观”选项卡，在右边的窗口中选择UTF8，如图所示：
[![SecureCRT中文乱码设置方法](http://www.iitshare.com/wp-content/uploads/2013/07/SecureCRT%E4%B8%AD%E6%96%87%E4%B9%B1%E7%A0%81%E8%AE%BE%E7%BD%AE%E6%96%B9%E6%B3%95-300x276.jpg)](http://www.iitshare.com/wp-content/uploads/2013/07/SecureCRT%E4%B8%AD%E6%96%87%E4%B9%B1%E7%A0%81%E8%AE%BE%E7%BD%AE%E6%96%B9%E6%B3%95.jpg)
\3. 此时，SecureCRT中即可正常显示中文了：
[![SecureCRT中文乱码设置方法2](http://www.iitshare.com/wp-content/uploads/2013/07/SecureCRT%E4%B8%AD%E6%96%87%E4%B9%B1%E7%A0%81%E8%AE%BE%E7%BD%AE%E6%96%B9%E6%B3%952-300x48.jpg)](http://www.iitshare.com/wp-content/uploads/2013/07/SecureCRT%E4%B8%AD%E6%96%87%E4%B9%B1%E7%A0%81%E8%AE%BE%E7%BD%AE%E6%96%B9%E6%B3%952.jpg)
此时虽然可以显示中文，但是在SecureFX中新建的中文文件夹在SecureCRT中仍然会显示乱码，此问题如何解决了？需要通过修改配置文件进行配置，下面将进行具体说明。

#### 配置文件进行设置

\1. 找到SecureFX配置文件夹（选项--全局选项，常规下的配置文件夹），比如：C:\Users\ZhangYQ\AppData\Roaming\VanDyke\Config；
\2. 在配置文件夹下的Sessions子目录中，找到SecureCRT连接对应的Session文件（.ini扩展名），双击打开；
\3. 查找Filenames Always Use UTF8，将=号后面的参数改成00000001，保存退出即可。