# jdk安装

## 1、下载对应的版本

**1.1下载Linux对应版本的JDK**

```shell
getconf  LONG_BIT
```

**1.2下载压缩文件**   

> jdk-8u261-linux-x64.tar.gz         

## 2、解压压缩文件

```shell
tar -xvf   jdk-8u261-linux-x64.tar.gz  -C  /usr/local/  
```

## 3、配置环境变量

```shell
vim  /etc/profile

#自己选定的目录
export JAVA_HOME=/usr/local/jdk1.8.0_261
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export PATH=$PATH:$JAVA_HOME/bin
```

## 4、使用环境变量生效

```shell
source /etc/profile
```

[长久生效---0](/problem/problem.md)

## 5、测试JDK是否安装成功

```shell
java -version
```

