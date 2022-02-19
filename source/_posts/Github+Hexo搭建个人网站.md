---
title: Github+Hexo搭建个人博客过程
date: 2022-02-17 14:13:48
img: /cover-images/github+hexo.jpg
tags: 
- Hexo
- Github
- 搭建博客
categories: 个人博客
comments: false
---

> 历时三天，终于搭建好了博客，记录一下过程和遇到的问题！

## Github创建个人仓库

登陆到github，点击New repository创建新仓库，仓库名必须是 **username**.github.io，这是固定写法。

## 安装Git

### 在 MAC 上安装Git 

如果你的电脑上Mac，有两种安装Git的方法。

一是从[homebrew](https://brew.sh/)安装git：

```bash
# 安装homebrew
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
# 用brew安装Git
brew install git
```

 <br/>       
第二种方法是直接从AppStore安装Xcode，Xcode集成了Git，不过默认没有安装，你需要运行Xcode，选择菜单“Xcode”->“Preferences”，在弹出窗口中找到“Downloads”，选择`Command Line Tools`，点“Install”就可以完成安装了。Xcode是苹果官方IDE，是开发MAC和IOS APP的必选。

### 在Windows上安装Git

在Windows上使用Git，可以从[Git官网](https://git-scm.com/downloads)直接下载安装程序，然后按默认选项安装即可，只不过最后一步添加路径时选择`Use Git from the Windows Command Prompt`，这样我们就可以直接在命令提示符里打开git了。

安装完成后，在开始菜单里找到“Git”->“Git Bash”，蹦出一个类似命令行窗口的东西，就说明Git安装成功。或者在命令提示符中输入`git --version`验证是否成功安装。

### 连接Github和本地

右键打开git bash或者打开终端，然后输入以下命令：

```bash
git config --global user.name "Github用户名"
git config --global user.email "注册邮箱"
# 生成密钥SSH Key
ssh-keygen -t rsa -C “注册邮箱”
```
直接三个回车即可，默认不需要设置密码。

然后打开Github，点击头像下面的`settings`，再点击`SSH and GPG keys`，新建一个SSH，名字随意。

```bash
# 复制密钥
cat ~/.ssh/id_rsa.pub
```

将上面输出的内容复制到框中；或者找到生成的.ssh的文件夹中的id_rsa.pub密钥，复制全部内容，点击确定保存。

输入`ssh -T git@github.com`，如果如下图所示，出现你的用户名，那就成功了。

![Git设置成功](/images/git.png)

## 安装Node.js

Hexo基于Node.js，下载地址为 [Download | Node.js](https://nodejs.org/en/download)。安装后，检测Node.js和npm是否安装成功，在命令行中输入`node -v`和`npm -v`:

![安装node.js](/images/nodejs.png)

国内建议安装cnpm速度快。输入：

```bash
npm install -g cnpm –registry=https://registry.npm.taobao.org
```
添加环境变量后，输入`cnpm -v`，检测是否正常。

自此，安装Hexo的环境已经全部搭建完成了！

## 安装Hexo

Hexo就是我们的个人博客网站的框架， 这里需要自己在电脑常里创建一个文件夹，可以命名为`blog`，Hexo框架与以后你自己发布的网页都在这个文件夹中。创建好后，进入文件夹中，打开命令行。

使用npm命令安装Hexo，输入：

```bash
npm install -g hexo-cli

# 验证是否安装成功
hexo -v
```

安装完成后，初始化博客：

```bash
hexo init blog
```

此时，通过`hexo s`命令即可在本地启动您的博客站点了。

```bash
$ hexo s
INFO  Validating config
INFO  Start processing
INFO  Hexo is running at http://localhost:4000 . Press Ctrl+C to stop.
```

### Hexo文件说明

**_config.yml**: 用来配置博客相关的参数，初始化时自动创建。具体参数设置，可参照[Hexo 配置](https://hexo.io/zh-cn/docs/configuration)文档。

**node_modules** 和 **package.json**都是在初始化时自动创建。

 - node_modules用来存储已安装的各类依赖包。
 - package.json用来查看 Hexo 的版本以及相关依赖包的版本。

Hexo 会默认安装：

 - hexo：主程序
 - hexo-deployer-git：实现 git 部署方式
 - hexo-generator-archive：存档页面生成器
 - hexo-generator-category：分类页面生成器
 - hexo-generator-index：index 生成器
 - hexo-generator-tag：标签页面生成器
 - hexo-renderer-ejs：支持 EJS 渲染
 - hexo-renderer-marked：Markdown 引擎
 - hexo-renderer-stylus：支持 stylus 渲染
 - hexo-server：支持本地预览，默认地址 localhost:4000

通过`npm install xx`安装的新依赖包，也会保存在`node_module`文件夹下。

**scaffold**: 模板文件夹，初始化时自动创建。包含page，post，draft三种模板，分别对应 页面、要发布的文章、草稿。

**themes**: 主题文件夹，初始化时自动创建。每一个主题，都有一个单独的文件夹。默认主题为 [landscape](https://github.com/hexojs/hexo-theme-landscape)。

**source**: 资源文件夹。用来存放图片、Markdown 文档（文章、草稿）、各种页面（分类、关于页面等）。
**public**: 将 source 文件夹里的 Markdown 文档，转换成 index.html。再结合主题进行渲染，就是我们最终看到的博客。
**.deploy_git**: 将 public 文件夹的内容提交到 Github 后生成，内容与 public 文件夹基本一致。

这三者的关系大致是：source -> public -> .deploy_git:
执行hexo generate，根据 source，更新 public。
执行hexo deploy，根据 public，更新 .deploy_git。

### Hexo常用命令

常用指令及说明

 - `npm install hexo -g` 安装Hexo
 - `npm update hexo -g` 升级
 - `hexo init blog` 初始化博客

 - `hexo n "我的博客" == hexo new "我的博客"` 新建文章
 - `hexo clean` 清除缓存，若是网页正常情况下可以忽略这条命令，执行该指令后,会删掉站点根目录下的 public 文件夹
 - `hexo g == hexo generate` 生成静态网页。会在站点根目录下生成 public 文件夹, hexo 会将 ”/blog/source/“ 下面的 .md 后缀的文件编译为 .html 后缀的文件,存放在 ”/blog/public/“ 路径下
 - `hexo d == hexo deploy` #部署
 - `hexo s == hexo server` 启动本地服务器，用于预览主题。Hexo 会监视文件变动并自动更新，除修改站点配置文件外,无须重启服务器,直接刷新网页即可生效。
 - `hexo server -s` 以静态模式启动
 - `hexo server -p 5000` 更改端口为5000
 - `hexo server -i “IP地址”` #自定义 IP

## 推送Hexo到Github

上面只是在本地预览，接下来要做的就是就是推送网站，也就是发布网站，让我们的网站可以被其他人访问。将我们的Hexo与GitHub关联起来，打开站点的配置文件_config.yml，翻到最后修改为：

deploy:
  type: git
  repo: 这里填入你之前在GitHub上创建仓库的完整路径，记得加上 .git
  branch: main

repo地址建议复制github上的SSH地址，如图：

![github-SSH地址](/images/github-SSH.png)

前面的url改为： `https://<Github用户名>.github.io`，author改为自己的名字，language改为zh-CN。

然后在博客根目录下打开命令行，通过`npm install hexo-deployer-git --save`安装Git部署插件，输入`hexo d`上传到Github，这时打开你的github.io主页就能看到发布的文章了。

### Hexo部署出现错误err: Error: Spawn failed解决方式

部署过程中可能会出现错误：

```bash
fatal: unable to access 'https://github.com/ljq0067/ljq0067.github.io/': Encountered end of file
FATAL {
  err: Error: Spawn failed
      at ChildProcess.<anonymous> (/blog/node_modules/hexo-util/lib/spawn.js:51:21)
      at ChildProcess.emit (events.js:376:20)
      at Process.ChildProcess._handle.onexit (internal/child_process.js:277:12) {
    code: 128
  }
} Something's wrong. Maybe you can find the solution here: %s https://hexo.io/docs/troubleshooting.html
```

解决方式一：

```bash
##进入站点根目录
cd /blog/

##删除git提交内容文件夹
rm -rf .deploy_git/

##执行
git config --global core.autocrlf false

##最后
hexo clean && hexo g && hexo d
```

解决方式二：有可能是你的git repo配置地址不正确,可以将http方式变更为ssh方式

解决方式三：不建议

```bash
##进入站点根目录
cd /blog/

##进入deploy文件夹
cd .deploy_git/

##强制推送
git push -f
```

## 参考文献

> Git安装：https://www.liaoxuefeng.com/wiki/896043488029600
> homebrew: https://brew.sh/
> Hexo 部署出现错误 Spawn failed解决方式: https://www.jianshu.com/p/c60ad2a33a1e 