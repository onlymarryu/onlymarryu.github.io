# Kafka安装

​	kafka 的安装包其实没有分window 还是 linux, 所以下载的安装包还是之前的安装包，直接解压出来即可；



![img](a.assets/v2-5ea0647a0e4acab3a7c6de90df11fe3e_720w.jpg)



启动Zookeeper 服务端命令

```text
./bin\windows\zookeeper-server-start.bat  ./config\zookeeper.properties 
```

这边会报一个奇葩的错误，命令行太长，直接将压缩包解压到根目录或者桌面进行操作



![img](a.assets/v2-0f8d03dbe85bdf6f44e05a20d87bc60b_720w.png)



启动成功



![img](https://cdn.jsdelivr.net/gh/onlymarryu/typora-ims-test@master/img/v2-348c2d162934ac0844103a2cfcf2f16c_720w.jpg)



启动kafka服务端命令

```text
 ./bin\windows\kafka-server-start.bat  ./config\server.properties
```

启动成功



![img](a.assets/v2-4f9527f5ed1fb0a09b90cd598b5c09c3_720w.jpg)

