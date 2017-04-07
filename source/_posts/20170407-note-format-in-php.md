---
title: note-format-of-class-in-php
date: 2017-04-07 11:28:57
categories: # 这里写的分类会自动汇集到 categories 页面上，分类可以多级
 - web开发
tags: # 这里写的标签会自动汇集到 tags 页面上
 - php
 - 后端开发
 - php注释
---
## PHP注释规范

注释在写代码的过程中非常重要，好的注释能让你的代码读起来更轻松，在写代码的时候一定要注意注释的规范。

### 单行注释

```bash
$n = 10; //数量n，这是单行注释
```

### 多行注释

```bash
/* php注释语法
   这是多行注释。*/ 
```

### 文件头的注释

介绍文件名，功能以及作者版本号等信息

```bash   
/** 
*文件名简单介绍
* 
*文件功能。 
* @author      alvin 作者
* @version     1.0 版本号
*/ 
```
### 函数的注释

函数作用，参数介绍及返回类型

```bash
/**  
* 函数的含义说明 
* 
* @access public 
* @param mixed $arg1 参数一的说明 
* @param mixed $arg2 参数二的说明 
* @param mixed $mixed 这是一个混合类型 
* @return array 返回类型
*/  
```

### 类的注释

类名及介绍

```bash
/** 
* 类的介绍 
* 
* 类的详细介绍（可选。）。 
* @author      alvin 作者 
* @version     1.0 版本号
*/ 
```

### PHPDocumentor 代码注释规范

[phpdoc官网](https://www.phpdoc.org/docs/latest/index.html)

### 附录 文档标记

所有的文档标记都是在每一行的 * 后面以@开头。如果在一段话的中间出来@的标记，这个标记将会被当做普通内容而被忽略掉。

#### @access 
使用范围：class,function,var,define,module 
该标记用于指明关键字的存取权限：private、public或proteced的使用范围：class,function,var,define,module

#### @author 
指明作者
 
#### @copyright 
使用范围：class，function，var，define，module，use 
指明版权信息 

#### @deprecated 
使用范围：class，function，var，define，module，constent，global，include 
指明不用或者废弃的关键字
 
#### @example 
该标记用于解析一段文件内容，并将他们高亮显示。Phpdoc会试图从该标记给的文件路径中读取文件内容 

#### @const 
使用范围：define 
用来指明php中define的常量
 
#### @final 
使用范围：class,function,var 
指明关键字是一个最终的类、方法、属性，禁止派生、修改。
 
#### @filesource 
和example类似，只不过该标记将直接读取当前解析的php文件的内容并显示。
 
#### @global 
指明在此函数中引用的全局变量
 
#### @ingore 
用于在文档中忽略指定的关键字 

#### @license 
相当于html标签中的<a>,首先是URL，接着是要显示的内容 
例如<a href=”http://www.baidu.com”>百度</a> 
可以写作 @license http://www.baidu.com 百度
 
#### @link 
类似于license 
但还可以通过link指到文档中的任何一个关键字 

#### @name 
为关键字指定一个别名。 

#### @package 
使用范围：页面级别的-> define，function，include 
类级别的->class，var，methods 
用于逻辑上将一个或几个关键字分到一组。
 
#### @abstrcut 
说明当前类是一个抽象类 

#### @param 
指明一个函数的参数 

#### @return 
指明一个方法或函数的返回指 

#### @static 
指明关建字是静态的。 

#### @var 
指明变量类型
 
#### @version 
指明版本信息 

#### @todo 
指明应该改进或没有实现的地方
 
#### @throws 
指明此函数可能抛出的错误异常,极其发生的情况 

上面提到过，普通的文档标记标记必须在每行的开头以@标记，除此之外，还有一种标记叫做inline tag,用{@}表示，

具体包括以下几种：
    
{@link}    
用法同@link 
   
{@source}    
显示一段函数或方法的内容  

参考链接:
[php中如何给类规范的注释](https://zhidao.baidu.com/question/546752811.html)
[php注释语法的规范](http://www.cnblogs.com/ysdong/p/6028191.html)
[PHPDocument 代码注释规范总结](https://sjolzy.cn/Specification-Summary-PHPDocument-code-comments.html)

