---
title: 使用GitHub+Hexo搭建博客
tags: [github, hexo]
categories: blog
---
虽然感觉来得有些晚了，但是为了总结昨天，积累今天，明天搞事情，还是开个博客吧。

这里记录用Hexo在GitHub上折腾着搭建博客的过程。

## 环境准备
主要需要Hexo和git/GitHub，还需要在你的系统中支持node.js。

Hexo是一个博客框架。

node.js是一个运行环境。Hexo是基于node.js的,所以要让hexo运行,node.js环境是必不可少的。

GitHub是使用Git工具进行版本管理的代码托管平台，允许大多数个人用户免费创建公共的代码仓库。
其中的GitHub Pages本来是用于介绍托管在GitHub的项目，这里就用它来发布博客。
***
### 安装node.js
参考：[Node.js 安装配置](http://www.runoob.com/nodejs/nodejs-install-setup.html)
***
### 安装Hexo
在shell下，输入
```
$ npm install -g hexo-cli
```
npm是node.js的模块管理器，通过上述命令全局安装了hexo。
***
### 安装GitHub

[GitHub下载地址](https://desktop.github.com/)

[GitHub简单介绍](https://help.github.com/articles/set-up-git/)

另，本人以前翻译的一篇Git的入门用法：[15 分钟学会使用 Git 和远程代码库](http://blog.jobbole.com/53573/)

[GitHub Pages用法](https://pages.github.com/)
***
## 使用Hexo
创建一个文件夹，在文件夹下执行命令进行Hexo的初始化：
```
$ hexo init
```
这样一条命令就搞定了基本内容！

接下来在本地试运行一下安装好的Hexo，生成博客用的静态文件。
```
$ hexo g
```

启动本地服务器，可以在本地预览博客。
```
$ hexo s
```
服务器默认端口号是4000，可以通过`-p`参数更改。

然后用浏览器访问`http://localhost:4000/`，就可以看到一个漂亮的博客了。当然这个博客只是
在本地的，需要发布到GitHub上才能被所有人看到。

### 创建新博客
```
$ hexo n "new post"
```
新建的文件会在`.\source\_post\`下，可以自定义分类标签内容，
```
title: new post
tags: [tag1, tag2]
categories: blog
---
正文用Markdown语法编写。
```

## 部署博客到GitHub
### 在GitHub上创建仓库
GitHub的注册、ssh登录等基本配置都妥当后，首先要在GitHub(`https://github.com`)上新建一个代码仓库（repository)。
博客用的User Page要求仓库名为`username.github.io`，如`cjpan.github.io`。
确认在Repository下的"Setting"-"GitHub Pages"已经被启用，域名应为`http://cjpan.github.io`。

### Hexo的部署配置
编辑Hexo下的_config.yml文件，在文件最下方的`Deployment`部分添加如下配置：
```
# Deployment
deploy:
  type: git
  repo: git@github.com:cjpan/cjpan.github.io.git
  branch: master
```
要注意冒号之后空格不能少！
保存之后运行下面的命令，就能把博客部署到GitHub上去了。

`$ hexo g`

`$ hexo d`

之后就可以到`http://cjpan.github.io`上去访问了。

也可以为`cjpan.github.io`仓库多创建一个分支（branch），比如叫`source`，然后在`source`下做修改、编写博客；
而发布的内容则在默认的`master`上。这样做到了编辑和发布的分离。

## 日常的发布
1. `hexo new "my new post"` 编写文章
2. `hexo clean` 清空缓存
3. `hexo g` 重新生成静态文件
4. `hexo s` 本地预览效果
5. `hexo d` 同步到GitHub上

## 后话
编辑Hexo下的_config.yml文件的`template`部分，可以选择其他模板。在[Hexo](https://hexo.io/)主页上有不少资源可以选择。
这里先选择了`jacman`模板，它支持了很多功能，其中不少是中国本地化的。
接下来想对界面再做一些自定义的修改。
不管怎么样，房子先住进去，边住边装修吧。
