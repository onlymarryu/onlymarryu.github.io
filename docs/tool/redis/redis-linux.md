## redis安装

### ——1

#### 1.安装依赖C语言依赖

​	redis使用C语言编写，所以需要安装C语言库

```
 yum install -y gcc-c++ automake autoconf libtool make tcl 
```

​	

#### 2.上传并解压

​	把redis-5.0.5.tar.gz上传到/usr/local/tmp中，解压文件

```
 cd /usr/local/tmp

 tar zxf redis-5.0.5.tar.gz
```

 

#### 3.编译并安装

​	进入解压文件夹

```
  cd /usr/local/tmp/redis-5.0.5/
```

​	编译

```
 make
```

​	安装	

```
 make install PREFIX=/usr/local/redis
```

 

#### 4.开启守护进程

​	复制cd /usr/local/tmp/redis-5.0.5/中redis.conf配置文件	

```
 cp redis.conf /usr/local/redis/bin/
```

 **修改配置文件**	

```
 cd /usr/local/redis/bin/

 vim redis.conf
```

​	把daemonize的值由no修改为yes

![](https://cdn.jsdelivr.net/gh/onlymarryu/typora-ims-test@master/Redis-01.jpg)

#### 5.修改外部访问

​	在redis5中需要修改配置文件redis.conf允许外部访问。需要修改两处。

​	注释掉下面

​	bind 127.0.0.1

```
 bind 127.0.0.1
```

​	protected-mode yes 改成 no

![](https://cdn.jsdelivr.net/gh/onlymarryu/typora-ims-test@master/Redis-02.jpg)

#### 6.启动并测试

​	启动redis

 ```shell
./redis-server redis.conf
 ```

​	重启redis

```
./redis-cli shutdown
./redis-server redis.conf
```

​	启动客户端工具

​	在redis5中客户端工具对命令会有提供功能。

 ```shell
./redis-cli 
 ```



###  ——2

#### 1、拉取镜像文件

```shell
docker pull redis[:(版本号)]
```

#### 2、去gitee库中下载redis配置文件（docker不自带配置文件）【也可不进行此步骤】

**建议看完，先别操作，再看三，看完三之后再决定如何操作**



然后按照文章内容进行，忽略下载。

文章地址： https://www.jb51.net/article/203274.htm 

、



![1645177895314](https://cdn.jsdelivr.net/gh/onlymarryu/typora-ims-test@master/1645177895314.png)





 **docker 镜像中没有redis.conf文件，要自己配置** 

```undefined
git pull --rebase https://gitee.com/zjj3366/mydemo.git master
```

#### 3、创建一个redis 服务容器 

**第二步进行了的**

```shell
docker run -p 6379:6379 \
--name myredis  \
-v /usr/local/docker/redis.conf:/etc/redis/redis.conf   \
-v /usr/local/docker/data:/data  \
-d redis  \
redis-server  /etc/redis/redis.conf  \
--appendonly yes

```

**第二步没有进行的**

```shell
docker run -p 6379:6379  \
--name myredis  \
-v /root/myredis/data:/data \
-v /root/myredis/conf/redis.conf:/etc/redis/redis.conf  \
-d redis:4.0  \
redis-server /etc/redis/redis.conf  \
--appendonly yes

```

**区别：**

​	其实也没有什么区别，只是映射的文件不同，也可以将第二步的文件直接创建到第二种创建服务容器的配置文件地址中去，这样也可使用第二个创建方式。







![1645177877202](https://cdn.jsdelivr.net/gh/onlymarryu/typora-ims-test@master/1645177877202.png)





![1645177918077](https://cdn.jsdelivr.net/gh/onlymarryu/typora-ims-test@master/1645177918077.png)



