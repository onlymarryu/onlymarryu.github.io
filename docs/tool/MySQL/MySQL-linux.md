# MySQL 安装

## ——1.1

**参考文档  ** *

https://blog.csdn.net/qq_37598011/article/details/93489404

缺少  ***\*libaio.so.1\**** 手动下载

```shell
#查看
# whereis libaio.so.1
libaio.so: /usr/lib64/libaio.so.1

#下载
yum install -y libaio 
```



- [x] ### 1、下载MySQL

​	下载完成之后上传到Linux上，**版本可根据下载地址中的序列化修改**，本文：5.7.27

```shell
https://cdn.mysql.com//archives/mysql-5.7/mysql-5.7.27-1.el7.x86_64.rpm-bundle.tar
```

### 2、解压并安装MySQL

```shell
#创建mysql目录
mkdir mysql
#解压
tar -zxvf mysql-5.7.27-1.el7.x86_64.rpm-bundle.tar -C /usr/local/
#进入文件夹
cd /usr/local/mysql
#开始安装服务
rpm -ivh mysql-community-common-5.7.27-1.el7.x86_64.rpm

rpm -ivh mysql-community-libs-5.7.27-1.el7.x86_64.rpm

rpm -ivh mysql-community-libs-compat-5.7.27-1.el7.x86_64.rpm

rpm -ivh mysql-community-server-5.7.27-1.el7.x86_64.rpm
```

### 3、 启动mysql并设置开机自启

```shell
systemctl start mysqld
systemctl enable mysqld
systemctl status mysqld
```

### 4、 获取mysql临时密码，设置mysql的root用户密码

```shell
#前往日志文件查找临时密码 
grep "password" /var/log/mysqld.log　　
```

>  `2019-06-02T04:11:28.935057Z 1 [Note] A temporary password ``is` `generated ``for` `root@localhost: zS+u&ro49wbo` 

```shell
#利用临时密码进入mysql
mysql -uroot -pzS+u&ro49wbo

#修改密码
alter user 'root'@'localhost' identified by '123456';
#报错：不符合密码策略
ERROR 1819 (HY000): Your password does not satisfy the current policy requirements
#修改密码策略
set global validate_password_policy=0;
set global validate_password_length=1;
alter user 'root'@'localhost' identified by '123456';

 #刷新
FLUSH PRIVILEGES;                                   

```

### 5、远程连接测试

​	[防火墙](#preblem_2)问题外，出现`1130`时：

 ![img](https://img-blog.csdnimg.cn/20190625110140950.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM3NTk4MDEx,size_16,color_FFFFFF,t_70) 

```shell
use mysql                                            #访问mysql库
update user set host = '%' where user = 'root';      #使root能再任何host访问
FLUSH PRIVILEGES;                                    #刷新
```

**安装成功**





## ——1.2

### 1、下载MySQL

```shell
wget https://dev.mysql.com/get/mysql80-community-release-el8-1.noarch.rpm
```

### 2、安装数据源

```shell
yum install mysql80-community-release-el8-1.noarch.rpm
```

### 3、检查数据源

查看mysql源是否安装成功：

```shell
yum repolist enabled | grep "mysql.*-community.*"
```

### 4、安装数据库

```shell
yum install mysql-community-server

如果过期可以在运行安装程序之前导入密钥：
rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2022
```

### 5、启动mysql

```shell
service mysqld start

service mysqld status
```

### 6、显示mysql的随机密码

```shell
grep 'temporary password' /var/log/mysqld.log
```

### 7、登录并修改mysql密码

登录：**（输入上面生成的密码）**

```shell
mysql -u root -p
```

###  8、修改密码：

```shell
ALTER USER 'root'@'localhost' IDENTIFIED BY 'Root_21root';
```

​		查看密码策略：

```shell
SHOW VARIABLES LIKE 'validate_password%';
#修改密码长度：
set global validate_password_length=1;
#修改密码等级：
set global validate_password_policy=0;
#设置自己想要的密码
ALTER USER 'root'@'localhost' IDENTIFIED BY '123456';
```



##  ——2

### 1、查询镜像

```shell
docker search mysql
```

### 2、拉取镜像

```shell
docker pull mysql:5.7
```

### 3、构建容器

先自己创建配置文件基础模板（**要是后期要修改配置文件必须加入 模板中的两个元素，不然容器启动会出错**）

```shell
mkdir -p /root/mysql/conf
vim  /root/mysql/conf/my.cnf
```

**模板内容：**

```shell
# Copyright (c) 2016, 2021, Oracle and/or its affiliates.
#
# This program is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License, version 2.0,
# as published by the Free Software Foundation.
#
# This program is also distributed with certain software (including
# but not limited to OpenSSL) that is licensed under separate terms,
# as designated in a particular file or component or in included license
# documentation.  The authors of MySQL hereby grant you an additional
# permission to link the program and your derivative works with the
# separately licensed software that they have included with MySQL.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License, version 2.0, for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program; if not, write to the Free Software
# Foundation, Inc., 51 Franklin St, Fifth Floor, Boston, MA  02110-1301 USA


!includedir /etc/mysql/conf.d/
!includedir /etc/mysql/mysql.conf.d/

[mysql]
default-character-set=utf8
[mysqld]
character_set_server=utf8
init_connect='SET NAMES utf8'
lower_case_table_names = 1

```

**构建容器**

```shell
docker run \
-p 3306:3306 \
-v /root/mysql/data:/var/lib/mysql \
-v /root/mysql/logs:/logs \
-v /root/mysql/conf/my.cnf:/etc/mysql/my.cnf \
-e MYSQL_ROOT_PASSWORD=root \
--name mysql \
--hostname node1 \
--restart=always \
-d mysql:5.7
```

**注**：出错时将本地映射文件检查一下，要是不需要配置，只需映射直接删掉就可，以后再改配置文件

### 4、进入容器

```shell
docker exec -it mysql /bin/bash
或者
docker exec -it mysql mysql -uroot -p
```



### 5、查看远程连接

还有一些方法也可以试一下

```shell
1.赋予权限格式：grant 权限 on 数据库对象 to 用户@IP(或者相应正则)

　　　　注：可以赋予select,delete,update,insert,index等权限精确到某一个数据库某一个表。

　　　　GRANT ALL PRIVILEGES ON *.* TO '用户名'@'%' IDENTIFIED BY '密码' WITH GRANT OPTION;
	  GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '123456' WITH GRANT OPTION;

　　　　这里表示赋予该用户所有数据库所有表（*.*表示所有表），%表示所有IP地址。

2.刷新权限：FLUSH PRIVILEGES;

3.查看权限：select user,host from mysql.user;

二.意外
　　1.配置文件种指定了blind-address：
　　　　查看Mysql配置文件种（一般是/etc/my.cnf种）是否指定了blind-address，这表示只能是某个或某几个ip能连接。如果有就将它注释了，前面加#号注释。然后从启mysql。
    　重启mysql：service mysqld restart,如果安装的是Mariadb（我的就是）,则需要使用systemctl restart mariadb.service
    　
    2.防火墙的原因：
　　　　可能会报：ERROR 2003 (HY000): Can't connect to MySQL server on '你要连接的IP' (111)。

　　　　原因：1.可能是Mysql端口不对（默认是3306），只需加参数 -P 你的端口指定就行；
　　　　
　　　2.还有可能是有防火墙阻止，可以通过telnet来测试（可以直接关闭防火墙）。
　　　　　　　　　　防火墙相关命令：

　　　　　　　　　　　　（1）查看防火墙状态：service  iptables status或者systemctl status firewalld或者firewall-cmd --state

　　　　　　　　　　　　（2）暂时关闭防火墙：systemctl stop firewalld或者service  iptables stop或者systemctl stop firewalld.service

　　　　　　　　　　　　（3）永久关闭防火墙：systemctl disable firewalld或者chkconfig iptables off或者systemctl disable firewalld.service

　　　　　　　　　　　　（4）重启防火墙：systemctl enable firewalld或者service iptables restart  或者systemctl restart firewalld.service

 　　　　　　　　　　　　 (5)永久关闭后重启：chkconfig iptables on

　　

　　3.端口未开启：（我遇到的就是这个原因）
　　　　　　Mysql：ERROR 2003 (HY000) 110（连接超时）

　　　　　　查看你的服务器是否把对应端口打开，未打开启动就行了。　
　　　　　　
   4.Navicat连接MySQL，出现2059 - authentication plugin 'caching_sha2_password'的解决方案
  	
  	0先进入容器，
  	docker exec -it mysql mysql -uroot -p
  	
  	1#修改加密规则password是自己的密码，root也是登陆账户，下同。
  	use mysql;
    ALTER USER 'root'@'localhost' IDENTIFIED BY 'root' PASSWORD EXPIRE NEVER; 
    
	2 #更新一下用户的密码 
	ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'root';
	
	3#刷新权限 
	FLUSH PRIVILEGES; 
	
	4#更新一下用户的密码
	ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'root'; 

```



