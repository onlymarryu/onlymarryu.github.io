# Zookeeper安装

## ——1

### 1     Zookeeper简介

​	zookeeper分布式管理软件。常用它做注册中心（依赖zookeeper的发布/订阅功能）、配置文件中心、分布式锁配置、集群管理等。

​	zookeeper一共就有两个版本。主要使用的是java语言写的。

### 2     安装

#### 2.1   上传压缩文件

​	上传到 /usr/local/tmp中

### 2.2   解压

```
 tar zxf apache-zookeeper-3.5.5-bin.tar.gz
 cp -r apache-zookeeper-3.5.5-bin ../zookeeper
```

### 2.3   新建data目录

进入到zookeeper中

```
 cd /usr/local/zookeeper
 mkdir data
```

### 2.4   修改配置文件

进入conf中

```
 cd conf
 cp zoo_sample.cfg zoo.cfg
 vim zoo.cfg
```

修改dataDir为data文件夹路径

```
dataDir=/usr/local/zookeeper/data
```



### 2.5   启动zookeeper

进入bin文件夹

```
 cd /usr/local/zookeeper/bin
 ./zkServer.sh start
```

通过status查看启动状态。稍微有个等待时间

```shell
  ./zkServer.sh status
```







## ——2

**资料：**

https://www.cnblogs.com/kingkoo/p/8732448.html

https://www.bilibili.com/video/av80874666?from=search&seid=1629145025211999104&spm_id_from=333.337.0.0

### 1     Zookeeper简介

​	zookeeper分布式管理软件。常用它做注册中心（依赖zookeeper的发布/订阅功能）、配置文件中心、分布式锁配置、集群管理等。

​	zookeeper一共就有两个版本。主要使用的是java语言写的。

### 2     安装

#### 2.1   拉取镜像

 * 自定义zookeeper目录

    ```shell
    mkdir /root/zookeeper
    
    mkdir /root/zookeeper/data #原因参照上面 -在docker中data其实也不用在宿主机行自己生成(目前如此)
    
    #仓库官网查看版本或者用命令直接查
    docker search zookeeper
    
    #拉取特定版本（3.5）
    docker pull zookeeper:3.5
    
    ```

#### 2.2 创建并启动容器

​	**注：就目前而且 好像直接在docker中使用 zookeeper 不需要过多的配置，直接用命令创建吧，下面的解释先不看，具体原因后面补充**

命令：

```shell
docker run -d --name zookeeper -v /root/zookeeper/data:/data -p 2181:2181 --restart always zookeeper:3.5
```

- `--restart always`：始终重启
- https://www.cnblogs.com/ageovb/p/15328796.html

**解释**

* 1、这里为啥只挂载了 data 目录--》因为当我们 没有直接的安装压缩包时，我们的就没有配置文件，所以我们直接生成，容器中就回自动生成conf文件，其中就有我们需要的 一系列配置文件，我们第一次创建的时候就可以如此，我们在进**行配置文件的修改完成**后，要将所有配置文件放到我们  bash 目录中的 conf 文件中去。

* 2、第一次进入容器

    * 在bash目录下进入bin 目录然后输入开启客户端命令

        ```shell
        ./zkCli.sh
        ```

* 3、第二次进入容器

    ```shell
    docker exec -it zookeeper /bin/bash  -p    端口号
    或
    docker exec -it zookeeper zkCli.sh
    ```

**只看到这就可*，容器创建成功就可以直接用了，还没有出现问题。有待补充**



#### 2.3   修改配置文件

* 配置文件来源
    * 1、可以去我们的资料中直接解压出**conf**目录粘贴到我们的 zookeeper 目录中去
    * 2、可以直接进行第 3 步，完事之后直接会在容器中生成。找到conf文件，修改其中的 zoo.cfg文件，

进入容器后，找到conf中的 **zoo.cfg文件，接下来就是修改 **dataDir** 路径为容器中的 data文件目录。

前提是要下载 vim ，因为容器中没有，命令是：

```
apt-get update

apt-get install -y vim

cd conf
# 没有zoo.cfg 就先拷贝 zoo_sample.cfg 两个其实是一个文件，内容相同
#cp zoo_sample.cfg zoo.cfg
vim zoo.cfg
```

修改dataDir为data文件夹路径

```
dataDir=/usr/local/zookeeper/data
```

