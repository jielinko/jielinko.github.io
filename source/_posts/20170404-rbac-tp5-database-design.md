---
title: RBAC权限管理系统---(1)数据表设计
date: 2017-04-03 11:31:49
categories: # 这里写的分类会自动汇集到 categories 页面上，分类可以多级
 - web开发
tags: # 这里写的标签会自动汇集到 tags 页面上
 - php
 - 后端开发
 - thinkPHP
 - RBAC
---

# RBAC权限管理系统---(1)数据表设计

## 一、RBAC所依赖的5张数据表

<!--more-->

### 数据表分析

think_user:    
```
id(用户ID-用户标识)
username(用户名)
password(密码)
create_time(用户注册时间-时间戳)
update_time(用户更新时间-时间戳)
status(启用状态:0表示禁用;1表示启用)
```

think_role:   
```
id(角色ID-角色标识)
rolename(角色名称)
pid(角色识别-父角色对应ID)
status(启用状态)
remark(备注)
```

think_role_user:    
```
role_id(角色ID-角色标识)
user_id(用户ID-用户标识)
```

think_node:   
```
id(节点ID-权限标识)
name(节点(权限)名称-(英文名，对应应用控制器、应用、方法名))
title(节点(权限)中文名)
status(启用状态)
remark(备注)
sort(排序值(默认为50))
pid(父节点ID（如:方法pid对应相应的控制器）)
level(节点类型：1:表示应用（模块）；2:表示控制器；3：表示方法)
```

think_access:   
```
role_id(角色ID-角色标识)
node_id(节点ID-权限标识)
level(层次)
pid(父标识)
module(模块)
```

### 创建数据库


```
-- Database: `tp5-rbac`
--

-- --------------------------------------------------------

--
-- 表的结构 `think_access`
--

CREATE TABLE IF NOT EXISTS `think_access` (
  `role_id` smallint(6) unsigned NOT NULL,
  `node_id` smallint(6) unsigned NOT NULL,
  `level` tinyint(1) NOT NULL,
  `pid` smallint(6) unsigned NOT NULL,
  `module` varchar(50) DEFAULT NULL,
  PRIMARY KEY (`role_id`,`node_id`),
  KEY `groupId` (`role_id`),
  KEY `nodeId` (`node_id`)
) ENGINE=MyISAM DEFAULT CHARSET=utf8;

-- --------------------------------------------------------

--
-- 表的结构 `think_node`
--

CREATE TABLE IF NOT EXISTS `think_node` (
  `id` smallint(6) unsigned NOT NULL AUTO_INCREMENT,
  `name` varchar(20) NOT NULL,
  `title` varchar(50) DEFAULT NULL,
  `status` tinyint(1) DEFAULT '0',
  `remark` varchar(255) DEFAULT NULL,
  `sort` smallint(6) unsigned DEFAULT NULL,
  `pid` smallint(6) unsigned NOT NULL,
  `level` tinyint(1) unsigned NOT NULL,
  PRIMARY KEY (`id`),
  KEY `level` (`level`),
  KEY `pid` (`pid`),
  KEY `status` (`status`),
  KEY `name` (`name`)
) ENGINE=MyISAM DEFAULT CHARSET=utf8;

-- --------------------------------------------------------

--
-- 表的结构 `think_role`
--

CREATE TABLE IF NOT EXISTS `think_role` (
  `id` smallint(6) unsigned NOT NULL AUTO_INCREMENT,
  `rolename` varchar(20) NOT NULL,
  `pid` smallint(6) DEFAULT NULL,
  `status` tinyint(1) unsigned DEFAULT NULL,
  `remark` varchar(255) DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `pid` (`pid`),
  KEY `status` (`status`)
) ENGINE=MyISAM DEFAULT CHARSET=utf8;

-- --------------------------------------------------------

--
-- 表的结构 `think_role_user`
--

CREATE TABLE IF NOT EXISTS `think_role_user` (
  `role_id` mediumint(9) unsigned NOT NULL DEFAULT '0',
  `user_id` char(32) NOT NULL DEFAULT '',
  PRIMARY KEY (`role_id`,`user_id`),
  KEY `group_id` (`role_id`),
  KEY `user_id` (`user_id`)
) ENGINE=MyISAM DEFAULT CHARSET=utf8;

-- --------------------------------------------------------

--
-- 表的结构 `think_user`
--

CREATE TABLE IF NOT EXISTS `think_user` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `username` varchar(20) NOT NULL,
  `password` varchar(32) NOT NULL,
  `create_time` int(11) DEFAULT NULL,
  `update_time` int(11) DEFAULT NULL,
  `status` tinyint(1) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=MyISAM DEFAULT CHARSET=utf8;

```

### 五张表的关系

这五张表的关系如下：   
一个用户对应着多个角色；一个角色可以属于多个用户；是多对多的关系，需要用个中间表即role_user表；
一个角色可以有多个权限；一个权限可以属于多个角色；是多对多的关系，需要有个中间表即access表；

那node表示什么？

node表是**记录权限的表**，说白了就是记录应用，控制器，及方法的表，即**要访问资源的集合**；
比如要访问的Admin应用下的Index控制器下的index方法；

从这里可以明确了TP5的**RBAC是控制用户对控制器及方法的访问权限进行权限的管理**。

![](http://i.imgur.com/8FChxoZ.png)

### 授权中心-Auth类的几个方法以及三个核心方法

1,checkAccess()方法 检测当前模块和操作是否需要验证，返回bool类型

2,checkLogin()方法 检测登录

3,getAccessList()获取权限列表

可以看出方法是依次获取项目模块，控制器，动作方法的权限的。

4,saveAccessList()检测用户的权限列表，并将权限存储到SESSION中

5,AccessDecision()权限决策，就是判断用户拥有哪些权限



从代码中可以看出，权限判断是有两种方式一种是根据session中的权限列表进行校验，一种是每次都查询数据库进行校验（下面配置参数说明的时候会说明）

在AccessDecision()方法中调用checkAccess()方法进行验证模块的过滤即去除不需要验证的模块，控制器和方法。知道了RBAC的实现思路，下面来使用Auth类进行权限验证。

### 参考链接:

[ThinkPHP的RBAC（基于角色权限控制）详解](http://www.cnblogs.com/tanteng/archive/2012/11/25/2787597.html)
[Thinkphp3.2.3中的RBAC权限验证](http://blog.csdn.net/zp_00000/article/details/51236719)
[RBAC权限设计](http://itopic.org/rbac-design.html)
[权限设计管理](https://wenku.baidu.com/view/48f548c76137ee06eff918fe.html)
[扩展RBAC用户角色权限设计方案](http://rongxh2010.iteye.com/blog/930648)
[RBAC权限管理](http://blog.csdn.net/painsonline/article/details/7183613/)
[ThinkPHP中关于RBAC使用详解](https://www.lyblog.net/detail/552.html)
[ThinkPHP后台登录拦截器](http://blog.sina.com.cn/s/blog_99730a660102vj7b.html)





