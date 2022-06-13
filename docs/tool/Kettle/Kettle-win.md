

# Kettle安装




下载地址：[https://sourceforge.net/project](https://link.zhihu.com/?target=https%3A//sourceforge.net/projects/pentaho/files/Data%20Integration/)

下载完成解压到任意路径

![img](https://cdn.jsdelivr.net/gh/onlymarryu/typora-ims-test@master/img/v2-9b34b600471d87b624e1a355af5e1a77_720w.png)



打开文件夹，找到Spoon.bat，创建桌面快捷方式，打开

![img](https://cdn.jsdelivr.net/gh/onlymarryu/typora-ims-test@master/img/v2-304e8e08f977c9e1f6c3c1aef07d76ad_720w.png)

![img](https://cdn.jsdelivr.net/gh/onlymarryu/typora-ims-test@master/img/v2-5a6b50aa38075f30a80868319b57189a_720w.jpg)

成功打开，安装完成



最后还要配置下oracle的驱动

找到oracle的安装目录搜索关键字：ojdbc

把ojdbc5.jar文件复制到ETL的lib目录下

![img](https://cdn.jsdelivr.net/gh/onlymarryu/typora-ims-test@master/img/v2-dd6bd35ae080600c32d3cf75ccc40389_720w.jpg)



这样就可以在kettle里连接到数据库了

![img](https://cdn.jsdelivr.net/gh/onlymarryu/typora-ims-test@master/img/v2-79043e230a5f8c040c429855c8766b9c_720w.jpg)



