

# Docker-Compose安装

## 方式一

官网地址：https://docs.docker.com/compose 

**推荐**： 国内地址：http://get.daocloud.io/#install-compose

```shell
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker- compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

速度比较慢的话使用下面的地址： 

```shell
curl -L https://get.daocloud.io/docker/compose/releases/download/1.25.0/docker- compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
```

修改文件夹权限 

```shell
chmod +x /usr/local/bin/docker-compose
```

建立软连接 

```shell
ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```

校验是否安装成功

```shell
docker-compose --version
```



## 方式二

**手动下载 docker-compose 到本地，然后上传到 linux 服务器的 /usr/local/bin 路径下**

（1）百度网盘：链接: https://pan.baidu.com/s/1o_2XsILfwcA7mRn-f7A1wA 提取码: qiue  —— 网盘中 docker-compose 版本：2.2.2

（2）也可以去GitHub上下：https://github.com/docker/compose/releases

![img](https://cdn.jsdelivr.net/gh/onlymarryu/typora-ims-test@master/img/1376119-20211222005944642-1364547608.png)

 

5. 重命名：

mv docker-compose-linux-x86_64 docker-compose

6. 授权：

 chmod +x ./docker-compose 

7. 查看版本：

docker-compose --version

![img](https://cdn.jsdelivr.net/gh/onlymarryu/typora-ims-test@master/img/1376119-20211222005757511-1189151957.png)

 







## 升级（方式三）

- 下载，命令如下👇：

```shell
sudo curl -L https://get.daocloud.io/docker/compose/releases/download/1.26.2/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
```

> 因Github国内访问不太稳定所以使用DaoCloud提供加速：[链接](http://get.daocloud.io/#install-compose)，你**可以通过URL中的版本号，自定义下载你所需要的版本文件。**

- 对命令进行一个授权

```shell
sudo chmod +x /usr/local/bin/docker-compose
```

- 查看compose版本命令

```shell
docker-compose --version
```

- 卸载

```shell
sudo rm /usr/local/bin/docker-compose
```

