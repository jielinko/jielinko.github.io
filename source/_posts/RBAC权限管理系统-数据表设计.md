#RBAC权限管理系统---(1)数据表设计

##一、什么是RBAC
基于角色的访问控制（Role-Based Access Control）作为传统访问控制（自主访问，强制访问）的有前景的代替受到广泛的关注。
在RBAC中，权限与角色相关联，用户通过成为适当角色的成员而得到这些角色的权限。这就极大地简化了权限的管理。
在一个组织中，角色是为了完成各种工作而创造，用户则依据它的责任和资格来被指派相应的角色，用户可以很容易地从一个角色被指派到另一个角色。角色可依新的需求和系统的合并而赋予新的权限，而权限也可根据需要而从某角色中回收。角色与角色的关系可以建立起来以囊括更广泛的客观情况。

##二、ThinkPHP中的RBAC
通过 5 张表实现权限控制：用户表(think_user),角色表(think_role)-也是用户分组表,权限表(think_node)-操作节点,用户角色表(think_role_user)-用户和角色(用户组)的对应,角色权限表(think_access)-各个操作和角色(用户组）的对应(node_role);

###数据表分析

think_user:    
```
id(用户标识)
username(用户名)
password(密码)
logintime(登录时间)
loginip(登录IP)
status(状态)
```

think_role:   
```
id(角色标识)
name(名称)
pid(角色识别)
status(状态)
remark(备注)
```

think_role_user:    
```
role_id(角色标识)
user_id(用户标识)
```

think_node:   
```
id(权限标识)
name(权限英文名称)
title(权限中文名称)
status(状态)
remark(备注)
sort(排序)
pid(父标识)
level(层次)
```

think_access:   
```
role_id(角色标识)
node_id(权限标识)
level(层次)
pid(父标识)
module(模块)
```

###创建数据库


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
) ENGINE=MyISAM DEFAULT CHARSET=utf8 AUTO_INCREMENT=1 ;

-- --------------------------------------------------------

--
-- 表的结构 `think_role`
--

CREATE TABLE IF NOT EXISTS `think_role` (
  `id` smallint(6) unsigned NOT NULL AUTO_INCREMENT,
  `name` varchar(20) NOT NULL,
  `pid` smallint(6) DEFAULT NULL,
  `status` tinyint(1) unsigned DEFAULT NULL,
  `remark` varchar(255) DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `pid` (`pid`),
  KEY `status` (`status`)
) ENGINE=MyISAM DEFAULT CHARSET=utf8 AUTO_INCREMENT=1 ;

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
  `logintime` int(11) DEFAULT NULL,
  `loginip` varchar(30) DEFAULT NULL,
  `status` tinyint(1) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=MyISAM DEFAULT CHARSET=utf8 AUTO_INCREMENT=1 ;
```


###参考链接:

[ThinkPHP的RBAC（基于角色权限控制）详解](http://www.cnblogs.com/tanteng/archive/2012/11/25/2787597.html)
[RBAC权限设计](http://itopic.org/rbac-design.html)
[权限设计管理](https://wenku.baidu.com/view/48f548c76137ee06eff918fe.html)
[扩展RBAC用户角色权限设计方案](http://rongxh2010.iteye.com/blog/930648)
[RBAC权限管理](http://blog.csdn.net/painsonline/article/details/7183613/)
[ThinkPHP中关于RBAC使用详解](https://www.lyblog.net/detail/552.html)
[ThinkPHP后台登录拦截器](http://blog.sina.com.cn/s/blog_99730a660102vj7b.html)




