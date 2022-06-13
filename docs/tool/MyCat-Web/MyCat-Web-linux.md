# mycat-web

## ——1

### 1、下载mycat-web安装包

​	官方地址：http://dl.mycat.org.cn/

### 2、解压安装包到/usr/local目录

```shell
tar -zxvf Mycat-web-1.0-SNAPSHOT-20170102153329-linux.tar.gz -C /usr/local/

```

## 3、进入mycat-web的目录运行启动命令

```shell
	./start.sh &

```

## 4、mycat-web的服务端口是8082，查看服务是否启动

```shell
	netstat -nlpt | grep 8082

```

## 5、通过地址访问服务

```shell
	192.168.1.164:8082/mycat/
```

## 6、mycat-web配置

### 6.1、配置zookeeper(可选)

```shell
cd /usr/local/mycat-web/mycat-web/WEB-INF/classes

#修改mycat.properties文件，可以修改zookeeper的地址
vim mycat.properties
```

### 6.2、添加mycat实例

	* 在页面的mycat配置
	
	* mycat服务管理中添加mycat实例，需要填写相关的参数

