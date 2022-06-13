# Docsify搭建

官网：https://docsify.js.org/#/zh-cn/



前提

>要配置好 `nodeJs` 、`npm`



## 安装 Docsify 插件

> 自己创建一个要下载到本地的文件夹，然后在这里直接利用命令下载

推荐全局安装 `docsify-cli` 工具，可以方便地创建及在本地预览生成的文档。

```sh
npm i docsify-cli -g
```



​	下载完成之后，进入 `./node_modules/.bin` 使用终端查看安装是否成功 

```sh
docsify -v
```



![image-20220507180321976](https://cdn.jsdelivr.net/gh/onlymarryu/typora-ims-test@master/img/image-20220507180321976.png)

出现版本号，表明安装成功，自己再将该路径定义为 **系统环境变量**





## [初始化项目](https://docsify.js.org/#/zh-cn/quickstart?id=初始化项目)

如果想在项目的 `./docs` 目录里写文档，直接通过 `init` 初始化项目。

```bash
# init 初始化 + 初始化文件
docsify init ./docs
```



初始化成功：

![image-20220507180556746](https://cdn.jsdelivr.net/gh/onlymarryu/typora-ims-test@master/img/image-20220507180556746.png)

![image-20220507180639225](https://cdn.jsdelivr.net/gh/onlymarryu/typora-ims-test@master/img/image-20220507180639225.png)



## [开始写文档](https://docsify.js.org/#/zh-cn/quickstart?id=开始写文档)

初始化成功后，可以看到 `./docs` 目录下创建的几个文件

- `index.html` 入口文件
- `README.md` 会做为主页内容渲染
- `.nojekyll` 用于阻止 GitHub Pages 忽略掉下划线开头的文件

直接编辑 `docs/README.md` 就能更新文档内容，当然也可以[添加更多页面](https://docsify.js.org/#/zh-cn/more-pages)。



## [本地预览](https://docsify.js.org/#/zh-cn/quickstart?id=本地预览)

​	通过运行 `docsify serve` 启动一个本地服务器，可以方便地实时预览效果。

```bash
docsify serve docs
```

​	

​	**默认访问地址：**   [http://localhost:3000](http://localhost:3000/) 。

用 `IDE` 有可能遇到的问题。

![image-20220507182733021](https://cdn.jsdelivr.net/gh/onlymarryu/typora-ims-test@master/img/image-20220507182733021.png)

解决办法：

https://blog.csdn.net/Bkhole/article/details/124636916



访问结果：

![image-20220507183235754](https://cdn.jsdelivr.net/gh/onlymarryu/typora-ims-test@master/img/image-20220507183235754.png)



和 `README.md` 文件内容一致。

> 我们在开发的时候 所有内容都是动态更新的



## 功能开发

> 主要内容还是参考 [官网 ](https://docsify.js.org/#/zh-cn/)，这里只做简单描述。



### 分页

​	如果需要创建多个页面，或者需要多级路由的网站，在 docsify 里也能很容易的实现。例如创建一个 `guide.md` 文件，那么对应的路由就是 `/#/guide`。

假设你的目录结构如下：

```text
.
└── docs
    ├── README.md
    ├── guide.md
    └── zh-cn
        ├── README.md
        └── guide.md
```

那么对应的访问页面将是

```text
docs/README.md        => http://domain.com
docs/guide.md         => http://domain.com/guide
docs/zh-cn/README.md  => http://domain.com/zh-cn/
docs/zh-cn/guide.md   => http://domain.com/zh-cn/guide
```



### [定制侧边栏](https://docsify.js.org/#/zh-cn/more-pages?id=定制侧边栏)

为了获得侧边栏，您需要创建自己的 `_sidebar.md`，你也可以自定义加载的文件名。默认情况下侧边栏会通过 Markdown 文件自动生成，效果如当前的文档的侧边栏。

首先配置 `loadSidebar` 选项，具体配置规则见[配置项#loadSidebar](https://docsify.js.org/#/zh-cn/configuration?id=loadsidebar)。

```html
<!-- index.html -->

<script>
  window.$docsify = {
      <!-- 打开配置 -->
    loadSidebar: true
  }
</script>
<script src="//cdn.jsdelivr.net/npm/docsify/lib/docsify.min.js"></script>
```

接着创建 `_sidebar.md` 文件，内容如下

```markdown
<!-- docs/_sidebar.md -->

* [首页](zh-cn/)
* [指南](zh-cn/guide)
```

注：

> 需要在 `./docs` 目录创建 `.nojekyll` 命名的空文件，阻止 GitHub Pages 忽略命名是下划线开头的文件。



###[显示目录](https://docsify.js.org/#/zh-cn/more-pages?id=显示目录)

自定义侧边栏同时也可以开启目录功能。设置 `subMaxLevel` 配置项，具体介绍见 [配置项#subMaxLevel](https://docsify.js.org/#/zh-cn/configuration?id=submaxlevel)。

```html
<!-- index.html -->

<script>
  window.$docsify = {
    loadSidebar: true,
    subMaxLevel: 2
  }
</script>
<script src="//cdn.jsdelivr.net/npm/docsify/lib/docsify.min.js"></script>
```



### [自定义导航栏](https://docsify.js.org/#/zh-cn/custom-navbar?id=自定义导航栏)



#### [HTML](https://docsify.js.org/#/zh-cn/custom-navbar?id=html)

如果你需要定制导航栏，可以用 HTML 创建一个导航栏。

注意：文档的链接都要以 `#/` 开头。

```html
<!-- index.html -->

<body>
  <nav>
    <a href="#/">EN</a>
    <a href="#/zh-cn/">中文</a>
  </nav>
  <div id="app"></div>
</body>
```



### [配置文件](https://docsify.js.org/#/zh-cn/custom-navbar?id=配置文件)

那我们可以通过 Markdown 文件来配置导航。首先配置 `loadNavbar`，默认加载的文件为 `_navbar.md`。具体配置规则见[配置项#loadNavbar](https://docsify.js.org/#/configuration?id=loadnavbar)。

```html
<!-- index.html -->

<script>
  window.$docsify = {
    loadNavbar: true
  }
</script>
<script src="//cdn.jsdelivr.net/npm/docsify/lib/docsify.min.js"></script>
<!-- _navbar.md -->

* [En](/)
* [中文](/zh-cn/)
```

**注：**

> 你需要在 `./docs` 目录下创建一个 `.nojekyll` 文件，以防止 GitHub Pages 忽略下划线开头的文件。

`_navbar.md` 加载逻辑和 `sidebar` 文件一致，从每层目录下获取。例如当前路由为 `/zh-cn/custom-navbar` 那么是从 `/zh-cn/_navbar.md` 获取导航栏。





### [封面](https://docsify.js.org/#/zh-cn/cover?id=封面)

通过设置 `coverpage` 参数，可以开启渲染封面的功能。具体用法见[配置项#coverpage](https://docsify.js.org/#/configuration?id=coverpage)。



#### [基本用法](https://docsify.js.org/#/zh-cn/cover?id=基本用法)

封面的生成同样是从 markdown 文件渲染来的。开启渲染封面功能后在文档根目录创建 `_coverpage.md` 文件。渲染效果如本文档。

*index.html*

```html
<!-- index.html -->

<script>
  window.$docsify = {
    coverpage: true
  }
</script>
<script src="//cdn.jsdelivr.net/npm/docsify/lib/docsify.min.js"></script>
<!-- _coverpage.md -->

![logo](_media/icon.svg)

# docsify <small>3.5</small>

> 一个神奇的文档网站生成器。

- 简单、轻便 (压缩后 ~21kB)
- 无需生成 html 文件
- 众多主题

[GitHub](https://github.com/docsifyjs/docsify/)
[Get Started](#docsify)

```



#### [自定义背景](https://docsify.js.org/#/zh-cn/cover?id=自定义背景)

目前的背景是随机生成的渐变色，我们自定义背景色或者背景图。在文档末尾用添加图片的 Markdown 语法设置背景。

```
_coverpage.md
<!-- _coverpage.md -->

# docsify <small>3.5</small>

[GitHub](https://github.com/docsifyjs/docsify/)
[Get Started](#quick-start)

<!-- 背景图片 -->

![](_media/bg.png)

<!-- 背景色 -->

![color](#f0f0f0)
```



#### [封面作为首页](https://docsify.js.org/#/zh-cn/cover?id=封面作为首页)

通常封面和首页是同时出现的，当然你也是当封面独立出来通过设置[onlyCover 选项](https://docsify.js.org/#/zh-cn/configuration?id=onlycover)。







### 部署

和 GitBook 生成的文档一样，我们可以直接把文档网站部署到 GitHub Pages 或者 VPS 上。



#### [GitHub Pages](https://docsify.js.org/#/zh-cn/deploy?id=github-pages)

GitHub Pages 支持从三个地方读取文件

- `docs/` 目录
- master 分支
- gh-pages 分支

我们推荐直接将文档放在 `docs/` 目录下，在设置页面开启 **GitHub Pages** 功能并选择 `master branch /docs folder` 选项。



#### 配置过程

1、创建一个GitHub仓库

> 要求：
>
> 1、仓库名格式必须符合：github_username.github.io
>
> 



2、配置 GithubPage

> 随便选一个主题，毕竟我们也不用

![image-20220507185116977](https://cdn.jsdelivr.net/gh/onlymarryu/typora-ims-test@master/img/image-20220507185116977.png)

![image-20220507185152734](https://cdn.jsdelivr.net/gh/onlymarryu/typora-ims-test@master/img/image-20220507185152734.png)

![image-20220507185320819](https://cdn.jsdelivr.net/gh/onlymarryu/typora-ims-test@master/img/image-20220507185320819.png)

![image-20220507185336250](https://cdn.jsdelivr.net/gh/onlymarryu/typora-ims-test@master/img/image-20220507185336250.png)



3、上传项目

> 直接将我们的项目上传到这个库中即可。



--考虑到是保姆级教程我们将上传步骤也写一下



（1）首先我们将仓库中的信息拉取下来

​	这是第二步配置结束之后会产生`GitHubPage`的配置文件

![image-20220507185418951](https://cdn.jsdelivr.net/gh/onlymarryu/typora-ims-test@master/img/image-20220507185418951.png)

（2）将配置文件与我们的docs文件一起重新提交到仓库中

```git
git add xx

git commit xx -m"注释"

git push xx 仓库地址 分支

```



我这里直接使用工具 `Sourcetree` 提交。

![image-20220507190045839](https://cdn.jsdelivr.net/gh/onlymarryu/typora-ims-test@master/img/image-20220507190045839.png)



4、上传成功，部署成功

直接访问：

https://onlymarryu.github.io/#/



![image-20220507184626485](https://cdn.jsdelivr.net/gh/onlymarryu/typora-ims-test@master/img/image-20220507184626485.png)

> ​	如果刷新不是想要的结果，只要确认之前所有本地测试结果正取，只是部署之后结果不对，这就是部署时间的问题，我们等一会刷新即可。