---
title: thinkPHP 5.0 笔记
date: 2017-04-05 10:15:00
categories: # 这里写的分类会自动汇集到 categories 页面上，分类可以多级
 - web开发
tags: # 这里写的标签会自动汇集到 tags 页面上
 - php
 - 后端开发
 - thinkPHP
---

THINKPHP_模版系统变量$Think

## 系统变量输出

普通的模板变量需要首先赋值后才能在模板中输出，但是系统变量则不需要，可以直接在模板中输出，系统变量的输出通常以 **{$Think** 打头，

例如:

```bash
{$Think.server.script_name} // 输出$_SERVER['SCRIPT_NAME']变量

{$Think.session.user_id} // 输出$_SESSION['user_id']变量

{$Think.get.pageNumber} // 输出$_GET['pageNumber']变量

{$Think.cookie.name}  // 输出$_COOKIE['name']变量

支持输出 $_SERVER、$_ENV、 $_POST、 $_GET、 $_REQUEST、$_SESSION和 $_COOKIE变量。
```

<!-- more -->

## 常量输出

例如:

```bash
{$Think.const.APP_PATH}

或者直接使用

{$Think.APP_PATH}
```

## 配置输出

输出配置参数使用:

例如:

```bash
{$Think.config.default_module}

{$Think.config.default_controller}
```

## 系统语言变量

输出语言变量可以使用:

例如:

```bash
{$Think.lang.page_error}

{$Think.lang.var_error}
```


参考链接:[THINKPHP_模版系统变量$Think](https://my.oschina.net/miaowang/blog/299755)
[系统变量](http://www.kancloud.cn/manual/thinkphp5/125004)