---
title: 2017-03-24 博客开通暨Hexo博客程序搭建和维护
categories:   # 这里写的分类会自动汇集到 categories 页面上，分类可以多级
 - 开发工具
tags: # 这里写的标签会自动汇集到 tags 页面上
 - Hexo
 - Git
---
最近整理学习资料,发现在学习过程中总结的一些知识点都很分散,也不系统,考虑再三后还是决定自己搭建一个博客平台。再深入比较ghost,Jekyll,和hexo后选择了Hexo。

该方案基于 Hexo + nextT + github。参考了这篇[hexo入门](http://www.maintao.com/2014/hexo-beginner%27s-guide/)指南用起来确实很爽,第一篇博文留给Hexo,以表敬意！

Hexo中文官网: [Hexo中文](https://hexo.io/zh-cn/docs/index.html)！   
NextT主题官网: [hexo-theme-next](http://theme-next.iissnan.com/)   
Markdown语法: [Markdown](http://www.appinn.com/markdown/)   

<!--more-->

## 第一节 博客搭建

### 新建新文章

``` bash
$ hexo new "new article"
```
此时在source/_posts目录下面，多了一个new-article.md的文件。

使用MD编辑器打开后就可以开始写作。

更多信息: [写作](https://hexo.io/zh-cn/docs/writing.html)

### 服务器

``` bash
$ hexo server
```

更多信息: [服务器](https://hexo.io/zh-cn/docs/server.html)

### 生成静态文件

``` bash
$ hexo generate
```

更多信息: [生成文件](https://hexo.io/zh-cn/docs/generating.html)

### 部署到远程网站

``` bash
$ hexo deploy
```

更多信息: [部署](https://hexo.io/zh-cn/docs/deployment.html)

### 快速发布

``` bash
$ hexo clean
清除缓存文件 (db.json) 和已生成的静态文件 (public)。
在某些情况（尤其是更换主题后），如果发现您对站点的更改无论如何也不生效，您可能需要运行该命令。

$ hexo generate(g)
生成静态文件。
-d, --deploy	文件生成后立即部署网站
-w, --watch	监视文件变动

$ hexo deploy(d)
部署网站。
-g, --generate	部署之前预先生成静态文件
```
### 博客的站点配置和Next主题配置

[徒手教你建自己的博客](https://123sunxiaolin.github.io/2016/08/27/%E5%BE%92%E6%89%8B%E6%95%99%E4%BD%A0%E5%BB%BA%E8%87%AA%E5%B7%B1%E7%9A%84%E5%8D%9A%E5%AE%A2/)
见 3.0.4 节 和 3.0.5 节


参考链接:
0,[使用Hexo + Next搭建静态博客](http://www.jianshu.com/p/f66103553c45)
1,[在Github上利用hexo搭建个人博客](http://js.sunansheng.com/p/9df4aba9c25a)
2,[HEXO+Github,搭建属于自己的博客](http://www.jianshu.com/p/465830080ea9)
3,[hexo deploy报错解决](http://www.yczmm.com/hexo-deploy%E6%8A%A5%E9%94%99%E8%A7%A3%E5%86%B3.html)
4,[hexo博客更换主题](http://www.tuicool.com/articles/zeIZJzv)
5,[20分钟教你使用hexo搭建github博客](http://www.jianshu.com/p/e99ed60390a8)
6,[从零开始定制hexo主题](http://www.maintao.com/2014/hexo-theme-from-scratch/)
7,[模范网站1](http://jovey-zheng.github.io/blog/)
8,[模范网站2](http://notes.iissnan.com/)
9,[模范网站3](http://blog.guowenfh.com/)
10,[史上最详细的Hexo博客搭建图文教程](https://xuanwo.org/2015/03/26/hexo-intor/)


## 第二节 博客维护

博客程序搭建完毕后就设计到了多台电脑共同管理hexo博客的课题。经过思考和实践。采取在 username.github.io 仓库设置分支 hexo-source 来托管代码源文件。而 master 分支来托管生成的静态网页。方案步骤如下：


### 第一步 搭建博客程序

(详情见第一节)

此时远程仓库只有一个 master分支 托管的为生成的静态网页，文件列表如下:

``` bash
2017/03
archives
css
images
js/src
lib
index.html

```

### 第二步 创建本地仓库和远程分支

1,先删除主题文件下的.git文件，可以直接删除或者执行下面的命令

``` bash
$ rm -rf .git
```

2,仓库和分支操作

以下操作均在本地博客程序根目录操作

``` bash
$ git init    //初始化本地仓库
$ git checkout -b hexo-source    //创建本地分支 hexo-source 并切换到该分支
或者(git branch hexo-source; git checkout hexo-source;)

$ git remote add origin git@github.com:username/username.github.io.git  //关联本地分支到远程库
$ git add . 
$ git commit -m "提交说明"
$ git push origin hexo-source //推送代码源文件到远程 hexo-source 分支
```

此时远程仓库新增了一个 hexo-source 分支 托管的为代码源文件，文件列表如下:

``` bash
scaffolds
source/_posts
themes
.gitignore
.npmignore
_config.yml
package.json
readme.md
```

至此，代码源文件和生成的静态文件均托管完毕。另外需要修改远程仓库hexo-source分支为默认分支。方便我们下次推送和换电脑后的博客更新

### 第三步 日常的改动的流程

在本地对博客进行修改（添加新博文、修改样式等等）后，通过下面的流程进行管理:

1,将修改后的代码源文件推动到github远程分支 hexo-source

在本地博客程序根目录依次执行:

``` bash
$ git branch hexo-source  //切换到本地 hexo-source 分支
$ git add .
$ git commit -m "提交说明"
$ git push origin hexo-source  //推送代码源文件到 github 远程 hexo-source 分支

```
此时代码源文件已经更新并推动到github远程分支 hexo-source

2,发布网站到远程仓库master分支上

在本地博客程序根目录依次执行:

``` bash
$ hexo clean
$ hexo g -d

```
此时生成的静态网页已经更新并推动到github远程分支master，同时完成网站发布


### 第四步 新电脑上修改博客流程

1,克隆远程仓库 hexo-source 分支(修改远程仓库hexo-source分支为默认分支)

``` bash
$ git clone git@github.com:username/username.github.io.git blog(本地博客程序仓库名)
```
2,在 blog 仓库下依次执行

```bash
npm install hexo
npm install
npm install hexo-deployer-git
```
来安装hexo博客程序运行需要的node程序模块。如果博客程序根目录的.gitignore文件中没有node_modules/。
这时候就不需要上面的命令了。不过代码托管量就变大了很多。会影响每次推送的速度。


自此，一个方便快捷，易于管理，可移植，抗风险的博客程序就完成了,接下来就好好享受写作的乐趣吧。
 

参考链接:
0,[多台电脑共同管理hexo博客](https://vonfly.github.io/2016/02/18/hexo-version-control/)
1,[使用hexo，如果换了电脑怎么更新博客？](https://www.zhihu.com/question/21193762/answer/20811453)
