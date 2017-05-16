---
title: CentOS7安装配置Apache和SSL
date: 2017-05-17 11:31:49
categories: # 这里写的分类会自动汇集到 categories 页面上，分类可以多级
 - web开发
tags: # 这里写的标签会自动汇集到 tags 页面上
 - 后端开发
 - linux
 - SSL
 - Apache
   
---

## CentOS7安装配置Apache HTTP Server

### 安装apache # yum install httpd
	[root@VM_30_43_centos ~]# yum install httpd
	Installing:
	 httpd
	Installing for dependencies: //安装httpd会自动安装一下依赖包:
	 apr
	 apr-util
	 httpd-tools
	 mailcap


### 查看apache # rpm -qi httpd  
	[root@VM_30_43_centos ~]# rpm -qi httpd
	Name        : httpd
	Version     : 2.4.6
	Release     : 45.el7.centos.4
	Architecture: x86_64
	Install Date: Tue May 16 23:44:25 2017
	Group       : System Environment/Daemons
	Size        : 9823677
	License     : ASL 2.0
	Signature   : RSA/SHA256, Thu Apr 13 09:04:44 2017, Key ID 24c6a8a7f4a80eb5
	Source RPM  : httpd-2.4.6-45.el7.centos.4.src.rpm
	Build Date  : Thu Apr 13 05:05:23 2017
	Build Host  : c1bm.rdu2.centos.org
	Relocations : (not relocatable)
	Packager    : CentOS BuildSystem <http://bugs.centos.org>
	Vendor      : CentOS
	URL         : http://httpd.apache.org/
	Summary     : Apache HTTP Server
	Description :
	The Apache HTTP Server is a powerful, efficient, and extensible
	web server.

### 查看是否启动成功 
	[root@VM_30_43_centos ~]# ps -ef|grep httpd
	root      9557  9409  0 02:19 pts/0    00:00:00 grep --color=auto httpd
	[root@VM_30_43_centos ~]# netstat -lntup|grep httpd
	[root@VM_30_43_centos ~]#                              #未启动
	[root@VM_30_43_centos ~]# systemctl start httpd.service #启动
	[root@VM_30_43_centos ~]# ps -ef|grep httpd 
	root      9653     1  0 02:21 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
	apache    9654  9653  0 02:21 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
	apache    9655  9653  0 02:21 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
	apache    9656  9653  0 02:21 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
	apache    9657  9653  0 02:21 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
	apache    9658  9653  0 02:21 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
	root      9660  9409  0 02:21 pts/0    00:00:00 grep --color=auto httpd
	[root@VM_30_43_centos ~]# netstat -lntup|grep httpd
	tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      9653/httpd 


### apache 启动 停止 重启 设置开机启动
	[root@VM_30_43_centos ~]# systemctl start httpd.service #启动
	[root@VM_30_43_centos ~]# systemctl stop httpd.service #停止
	[root@VM_30_43_centos ~]# systemctl restart httpd.service #重启
	[root@VM_30_43_centos ~]# systemctl enable httpd.service #设置apache开机启动

### apache 默认重要配置信息
	配置文件:/etc/httpd/conf/http.conf 
	默认访的访问页面:/etc/httpd/conf.d/welcome.conf
	服务器的根目录:/var/www/html 
	访问日志文件:/var/log/httpd/access_log 
	错误日志文件:/var/log/httpd/error_log 
	运行apache的用户:apache 
	运行apache的组:apache
	端口:80
	模块存放路径:/usr/lib/httpd/modules

### 修改配置文件
	[root@VM_30_43_centos /]# cd /etc/httpd/conf
	[root@VM_30_43_centos conf]#ls 
	httpd.conf magic
	[root@VM_30_43_centos conf]#cp httpd.conf httpd.conf.origin //将原有配置文件备份
	[root@VM_30_43_centos conf]# ls
	http.conf.origin  httpd.conf  magic
	[root@VM_30_43_centos conf]# more httpd.conf
	//查看配置文件,我们注意到以一配置：
	DocumentRoot"/var/www/html"
	  
	//特别是要注意这个配置
	//这是Apache 2.4的一个新的默认值，拒绝所有的请求！
	  
	<Directory />
	   AllowOverride none
	   Require all denied
	</Directory>

	//设置为自动启动
	[root@VM_30_43_centos conf]# systemctl enable httpd.service
	Created symlink from /etc/systemd/system/multi-user.target.wants/httpd.service to /usr/lib/systemd/system/httpd.service.
	//在centos7中chkconfig httpd on 被替换成 systemctl enable httpd

### 新建index.html放入服务器的根目录:/var/www/html 

### 接下来我们要为apache启用ssl，首先我们需要为Apache安装mod_ssl模块提供TLS/SSL功能：

#### https是通过mod_ssl实现的，因此检查并安装mod_ssl：
	[root@VM_30_43_centos /]# ls /etc/httpd/modules/ | grep "mod_ssl" #默认没有安装mod_ssl

#### 查看mod_ssl的安装包信息
	[root@VM_30_43_centos /]# yum list all mod_ssl
	Loaded plugins: fastestmirror, langpacks
	Loading mirror speeds from cached hostfile
	Available Packages
	mod_ssl.x86_64                                  1:2.4.6-45.el7.centos.4                                   updates
	[root@VM_30_43_centos /]# 

#### 安装mod_ssl
	[root@VM_30_43_centos /]# yum install mod_ssl

	Installed:
  	mod_ssl.x86_64 1:2.4.6-45.el7.centos.4                                                                         

	Complete!


#### 检查mod_ssl的安装结果
	[root@VM_30_43_centos /]# rpm -qc mod_ssl
	/etc/httpd/conf.d/ssl.conf #mod_ssl的配置文件存放位置
	/etc/httpd/conf.modules.d/00-ssl.conf 

#### mod_ssl安装完成
	[root@VM_30_43_centos /]# ls /etc/httpd/modules/ | grep "mod_ssl"
	mod_ssl.so


### 申请安全证书
	此步略过
	
### 配置安全证书

#### 预备工作

一般把.crt格式的放到/etc/pki/tls/certs/下，把.key格式的放到/etc/pki/tls/private/下。

#### 整合apache和ssl

1）配置ssl.conf文件中证书路径。

	SSLCertificateFile        /etc/pki/tls/certs/www.domain.com.cer  
	SSLCertificateKeyFile     /etc/pki/tls/private/www.domain.com.key    
	SSLCACertificateFile      /etc/pki/tls/certs/www.domain.com_ca.crt  
	
2）把ssl.conf中的服务器名称指向域名监听443端口，这样就可以https访问了。

	DocumentRoot "/var/www/html"
	ServerName www.domain.com:443

把这两行前的#号去掉并且填上正确的域名信息。
 
3）检查httpd.conf文件是否读入ssl.conf，打开httpd.conf文件，查看
是否被注释，如果注释了去掉前面的“#”：

注意，此时我们并没有看到读入ssl.conf等代码。最后发现是centos7下的module模块的存放路径，其实centos7和centos6之前的版本有很大差别，不管从命令上还是部分文件格式上都有差别，比如module_ssl在centos6之前都是在httpd.conf文件内，而对于centos7来说就在独立的目录下：cd /etc/httpd/conf.modules.d下了

Centos 6 apache开启ssl :

		打开 apache安装目录/conf/httpd.conf 文件,找到 里面两行      
        #LoadModule ssl_module modules/mod_ssl.so
        将行首的#去掉,保存文件

Centos 7 apache开启ssl :

	[root@VM_30_43_centos httpd]# cd conf.modules.d
	[root@VM_30_43_centos conf.modules.d]# ls
	00-base.conf  00-dav.conf  00-lua.conf  00-mpm.conf  00-proxy.conf  00-ssl.conf  00-systemd.conf  01-cgi.conf
	[root@VM_30_43_centos conf.modules.d]# 

	[root@VM_30_43_centos conf.modules.d]# vim 00-ssl.conf 

	LoadModule ssl_module modules/mod_ssl.so
	
	[root@VM_30_43_centos modules]# ls | grep "mod_ssl.so"
	mod_ssl.so
	[root@VM_30_43_centos modules]# 



4)最后重启apache服务。使用https安全方式访问你的域名成功，即配置成功。


5)

!!!注意：首页index.html 的文件权限为755，否则将会出现如上提示：

Forbidden

Youdon't have permission to access /main.html on this server.

解决方法：修改首页index.html读写权限。

#Chmod755  index.html









### 配置WEB站点 (假设使用/wwwroot目录下的文档)
	//创建两个网站的目录结构及测试用页面文件
	# mkdir /wwwroot/www
	# echo "www.bigcloud.local" > /wwwroot/www/index.html
	  
	# mkdir/wwwroot/crm
	# echo "crm.bigcloud.local" > /wwwroot/crm/index.html

	//配置虚拟机主机
	[root@VM_30_43_centos wwwroot]# cd /etc/httpd/
	[root@VM_30_43_centos httpd]# ls
	conf  conf.d  conf.modules.d  logs  modules  run
	
	[root@VM_30_43_centos httpd]# mkdir vhost-conf.d
	[root@VM_30_43_centos httpd]# ls
	conf  conf.d  conf.modules.d  logs  modules  run  vhost-conf.d
	[root@VM_30_43_centos httpd]# echo "Include vhost-conf.d/*.conf" >> conf/httpd.conf
	[root@VM_30_43_centos httpd]# vi /etc/httpd/vhost-conf.d/vhost-name.conf

	//添加如下内容
	<VirtualHost *:80>
	  ServerNamewww.bigcloud.local
	  DocumentRoot /wwwroot/www/
	</VirtualHost>
	<Directory /wwwroot/www/>
	  Requireall granted
	</Directory>
	  
	<VirtualHost *:80>
	  ServerNamecrm.bigcloud.local
	  DocumentRoot /wwwroot/crm/
	</VirtualHost>
	<Directory /wwwroot/crm/>
	  Require ip192.168.188.0/24   //可以设置访问限制
	</Directory>

### 参考链接
[centos7.2 利用yum安装配置apache2.4多虚拟主机](http://blog.csdn.net/wusilen/article/details/54317822)
[Centos7.2 搭建Apache+Php+Mysql环境](http://www.th7.cn/Program/php/201607/890421.shtml)
[CentOS7安装配置Apache HTTP Server](http://leaus.blog.51cto.com/9273485/1540717)
[CentOS 7.2 配置Apache服务（httpd）--上篇](http://blog.csdn.net/wh211212/article/details/52982917)
[Centos7下Apache详细安装配置及证书申请SSL配置介绍](http://www.iyunv.com/thread-84322-1-1.html)
[CentOS服务器下安装配置SSL](http://www.linuxidc.com/Linux/2014-12/110747.htm)
[Linux下部署apache+ssl](http://jingyan.baidu.com/article/adc815138136e8f722bf7353.html)
[自助安装SSL证书教程](http://www.west.cn/faq/list.asp?Unid=1406)