---
title: 2017-03-24 Hello World!
---
最近整理学习资料,发现在学习过程中总结的一些知识点都很分散,也不系统,考虑再三后还是决定自己搭建一个博客平台。再深入比较ghost,Jekyll,和hexo后选择了Hexo。

该方案基于 Hexo + nextT + github。参考了这篇[hexo入门](http://www.maintao.com/2014/hexo-beginner%27s-guide/)指南用起来确实很爽,第一篇博文留给Hexo,以表敬意！

Hexo中文官网: [Hexo中文](https://hexo.io/zh-cn/docs/index.html)！
NextT主题官网: [hexo-theme-next](https://github.com/iissnan/hexo-theme-next)

## 快速开始

### 新建新文章

``` bash
$ hexo new "My New Post"
```

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
9,[史上最详细的Hexo博客搭建图文教程](https://xuanwo.org/2015/03/26/hexo-intor/)
