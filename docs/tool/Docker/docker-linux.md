# Docker安装

docker官网，自己选定自己的Linux版本，按照官方文档配置即可，以下CentOS7的

https://docs.docker.com/engine/install/centos/

## 1、卸载原有的环境：

```shell
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-selinux \
                  docker-engine-selinux \
                  docker-engine
```



## 2、安装对应的依赖环境和镜像地址

````shell
sudo yum install -y yum-utils 
````

官方安装(慢)

```shell
sudo yum-config-manager \ --add-repo \ https://download.docker.com/linux/centos/docker-ce.repo
```

安装过慢设置镜像 

````shell
sudo yum-config-manager \ --add-repo \ http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
````

## 3、直接安装docker CE

````shell
sudo yum install -y docker-ce docker-ce-cli containerd.io
````

报错时，大概率为**镜像仓库问题** ，直接执行 **步骤五** 的补充，再安装

## 4、启动docker服务

````shell
sudo systemctl start docker 
````

## 5、查看docker的版本

````shell
sudo docker version 
````

**补充：通过官方的镜像地址下载docker会比较慢， **

* 配置阿里云的镜像地址： **经历过3的镜像库问题也就是配置阿里云镜像地址**

````shell
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
````

* yum更新下即可：

````shell
yum makecache fast 
````

## 6、开机启动docker

````shell
sudo systemctl enable docker
````

* 补充：docker pull 下载镜像太慢，更换源进行网络加速的解决方案

```shell
步骤1：
sudo mkdir -p /etc/docker

步骤2：
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://glhlrb75.mirror.aliyuncs.com"]
}
EOF
或者（老师的）
sudo tee /etc/docker/daemon.json <<-'EOF'
{ 
	"registry-mirrors": ["https://v9j5rufo.mirror.aliyuncs.com"] 
}
EOF

步骤3：
sudo systemctl daemon-reload

步骤4：
sudo systemctl restart docker
```

## 7、使用



[**阿里云安装docker：**](https://blog.csdn.net/qq_25760623/article/details/88657491)

 https://blog.csdn.net/qq_25760623/article/details/88657491 

