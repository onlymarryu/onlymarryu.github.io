# 问题总结



## 0、布置在服务器中的东西，通过URL 无法访问

> ​	1、防火墙问题
>
> ​	2、端口 释放问题 ，**·我们要手动去配置我们服务器的安全组·**



<div name="perblem-1">


## 1、解决：CentOS 7 每次进入要重新加载环境变量

```shell
1.进入系统配置文件
vim ~/.bashrc

2.末尾添加如下代码
source /etc/profile

保存即可
:wq
```



## 2、端口占用、查看端口

```shell
查看是否运行端口   
netstat -tln 8080

查看端口属于哪个程序？端口被哪个进程占用(得到PID)	
lsof -i :8080  

关闭PID对应的程序            
kill -9 (PID)
```

<div name="preblem_2">


## 3、防火墙关闭、开启

```shell
关闭防火墙 ，重启失效(Linux系统一重启Linux中的防火墙又会被开起)
service firewalld stop

禁用防火墙，永久有效
systemctl disable firewalld
systemctl disable firewalld.service

启动防火墙 (对禁用的防火墙进行启动)
systemctl enable firewalld

防火墙相关命令：

1）查看防火墙状态：
service  iptables status或者systemctl status firewalld或者firewall-cmd --state

2）暂时关闭防火墙：
systemctl stop firewalld或者service  iptables stop或者systemctl stop firewalld.service

3）永久关闭防火墙：
systemctl disable firewalld或者chkconfig iptables off或者systemctl disable firewalld.service

4）重启防火墙：
systemctl enable firewalld或者service iptables restart  或者systemctl restart firewalld.service

5)永久关闭后重启：
chkconfig iptables on
```

## 4、failure: repodata/repomd.xml from epel: [Errno 256] No more mirrors to try.

http://mirrors.aliyun.com/epel/5/x86_64/repodata/repomd.xml: [Errno 14] HTTP Error 404 - Not Found的解决办法：

​	一直说那个XML文件不存在，以为是yum源是去读取xml当中的数据然后去下载。确实自己去访问也是404.还以	为是这阵子开会yum源都搞不能用了怎么。之前我用阿里用的挺好的。

​	最简单的办法就是删除 /etc/yum.repos.d/ 下所有的文件，重新来。

```shell
cd /etc/yum.repos.d/
mkdir repo_bak
mv *.repo repo_bak/
#下载缓存文件 版本自己控制
wget http://mirrors.aliyun.com/repo/Centos-7.repo
yum clean all
yum makecache

```

<div name="problem-5">


## 5、Xshell连接centos7能连上但是连接过程很慢

```shell
原因：因为在登录时，需要反向解析dns。
解决方法：修改linux配置文件，

vim /etc/ssh/sshd_config

将 # UseDNS yes 此处注释去掉
改为：UseDNS no
```

[跳转到Linux安装_4](#Linux_minInstall_4)

<div name="problem-6"></div>

## 6、Xshell 连接Centos7，root拒绝登录，而其他用户可登陆？———— root用户直接登录

**PermitRootLogin**  的值改成  yes  ，并保存 

```shell
vim /etc/ssh/sshd_config
```

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/onlymarryu/typora-ims-test@master/img/20210712112157753.png) 



 重启sshd 服务 

```shell
service sshd restart
```



 如果上面命令提示文件不存在，可以用下面的命令 

```
systemctl status sshd.service
```



 如果还不行，直接 重启服务器。。。 

```
reboot
```



意外的是，发现root依然不能登录。最后研究sshd_config的每一行意义，发现坑在这里： 

**注：有则看，无则跳**

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/2021071211252989.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1lhbmdfeWFuZ3lhbmc=,size_16,color_FFFFFF,t_70) 



文件的最后一行，有一行 ：AllowUsers xxx@192.168.1.*。这是写死的了，即：能远程登录的 用户名、IP 信息。难怪，其他用户一直无法登录。
所以，在后面追加：root@192.168.1.*
修改完的代码为：xxx@192.168.1.* root@192.168.1.*
然后再执行重启服务操作，然后发现root可以登录了！搞定！！！

> 注意：`xxx@192.168.1.* root@192.168.1.*` 之间要有空格。
>
> 或者直接将此行 代码直接注释掉，也可以。

[跳转到Linux安装_最小化安装_6](#Linux_minInstall_6)



<div name="problem-7"></div>

## 7、yum直接安装docker-ce报错找不到安装包

```shell
#更换成阿里云镜像仓库

yum-config-manager --add-repo   http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo


```

[回到Docker安装步骤3](#docker_install_3)



## 8、docker 查看日志

```shell
 docker logs [OPTIONS] CONTAINER
  Options:
        --details        显示更多的信息
    -f, --follow         跟踪实时日志
        --since string   显示自某个timestamp之后的日志，或相对时间，如42m（即42分钟）
        --tail string    从日志末尾显示多少行日志， 默认是all
        
    -t, --timestamps     显示时间戳
        --until string   显示自某个timestamp之前的日志，或相对时间，如42m（即42分钟）		
        
```

​	查看指定时间后的日志，只显示最后100行：

```shell
$ docker logs -f -t --since="2018-02-08" --tail=100 CONTAINER_ID
```

​	查看最近30分钟的日志:

```shell
$ docker logs --since 30m CONTAINER_ID
```

​	查看某时间之后的日志：

```shell
$ docker logs -t --since="2018-02-08T13:23:37" CONTAINER_ID
```

​	查看某时间段日志：

```shell
$ docker logs -t --since="2018-02-08T13:23:37" --until "2018-02-09T12:23:37" CONTAINER_ID
```



## 9 、如何解决error: failed to push some refs to ‘https://gitee.com/

​	出现错误的主要原因是gitee(github)中的README.md文件不在本地代码目录中

​	此时我们要执行git pull --rebase origin master命令**README.md**拉到本地

```shell
git pull --rebase origin master
```

然后执行git push origin master

```shell
git push origin master
```

就ok啦！





## 10 、idea 官网下载插件过慢

 [(29条消息) Intellij IDEA下载插件太慢，怎么办？_Real_csdn_User的博客-CSDN博客_idea下载插件很慢](https://blog.csdn.net/Real_CSDN_User/article/details/113944496) 

### （1）查询自己的网络服务提供商

访问[iP138查询网](https://www.ip138.com/)，查看自己的网络服务提供商并记下它。比如我访问该网站截图如下：
![我的网络服务提供商是移动（为保护隐私，部分内容打码）](https://cdn.jsdelivr.net/gh/onlymarryu/typora-ims-test@master/img/20210222191130433.png#pic_center)
如果你已经知道自己的网络服务提供商就不需要这一步。

### （2）查找访问插件网站最快的IP

Intellij IDEA的插件主页地址是[https://plugins.jetbrains.com](https://plugins.jetbrains.com/)。打开[网站测速 - 站长工具](http://tool.chinaz.com/speedtest/)，将插件的主页地址填入输入框内，点击查看分析按钮。一段时间后，就可以看到全中国各个地区访问插件主页的速度。点击表头“总耗时”右侧的小按钮，令全国各地访问插件主页的总耗时按增长顺序排列，这样耗时最短、速度最快的行就在最上方。从表格中找到与自己网络服务提供商相同的行，记下对应的IP地址。下面是我测量的访问插件主页的速度的结果：
![我的网络服务提供商是移动，我记下的IP是54.192.23.52](https://cdn.jsdelivr.net/gh/onlymarryu/typora-ims-test@master/img/20210222193138206.png)

### （3）在hosts文件中为plugins.jetbrains.com添加相关条目

用文本编辑器打开C:\Windows\System32\drivers\etc\hosts文件。在文件最下方添加一行文字：
[你记下的IP地址] plugins.jetbrains.com
[你记下的IP地址]用你记下的IP地址代替，记住不要带上方括号，而且IP地址和plugins.jetbrains.com之间有空格。例如我在文件中添加的文字是：
13.225.160.7 plugins.jetbrains.com
注意：编辑hosts文件需要管理员权限。

### 三、题外话

这种方法并不只限于加快下载IDEA插件的速度，只要你知道网络资源的网址，而且该网络资源使用了下载加速服务器，都可以用这种方法。