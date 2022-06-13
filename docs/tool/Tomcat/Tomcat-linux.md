# Tomcat安装

## ——1

### 1、下载Linux对应版本的Tomcat

​	  下载压缩文件     apache-tomcat-7.0.68.tar.gz         

### 2、解压压缩文件

```shell
tar -xvf   apache-tomcat-7.0.68.tar.gz -C /usr/local/  #解压到的路径 
```

### 3、配置环境变量

```shell
vim  /etc/profile

#自己定义为位置
export CATALINA_BASE=/usr/local/apache-tomcat-7.0.68
export PATH=$CATALINA_BASE/bin:$PATH

```

### 4、使用环境变量生效

````shell
source /etc/profile
````

### 5、启动Tomcat服务

* 注：要是远程访问你关注防火墙问题

启动Tomcat服务：

````shell
./startup.sh
````


启动Tomcat并输出启动日志 :

````shell
  ./startup.sh & tail -f  ../logs/catalina.out
````

