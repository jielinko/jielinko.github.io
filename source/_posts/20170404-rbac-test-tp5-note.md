---
title: RBAC权限管理笔记ThinkPHP（3.2.3和5.0）
date: 2017-04-04 21:58:09
categories: # 这里写的分类会自动汇集到 categories 页面上，分类可以多级
 - web开发
tags: # 这里写的标签会自动汇集到 tags 页面上
 - php
 - 后端开发
 - thinkPHP
 - RBAC
---

ThinkPHP权限管理（3.2.3-5.0）

## 关于此项目的说明

[链接地址](http://www.kancloud.cn/a113211/alls)

包含三套源码: 
1,前端模版源码
2,thinkphp3.2.3权限管理demo源码
3,thinkphp5.0权限管理demo源码

demo地址: http://www.sucaime.com/think_right/

账号: test
密码: 123456

<!-- more -->
## ThinkPHP 3.2.3 权限管理

项目目录改为 rbac-test-3.2.3，安装成功后 去登录后访问

[后台](http://localhost/rbac-test-3.2.3/index.php) 或者 [后台](http://localhost/rbac-test-3.2.3/index.php/Home/Index/index/)

默认账号：test 
默认密码：12345 

超级管理员 admin   
密码为：123456

### 管理设置 (test)

#### 个人中心 

两个权限控制：   
1. 页面的访问权限: Admin/User/my_center   
2. 修改资料的权限: Admin/User/change_msg

### 权限控制 (admin)

#### 权限管理 

四个权限控制：
1. 权限管理页面: Admin/Rule/rule_list
2. 添加权限功能: Admin/Rule/add
3. 修改权限功能: Admin/Rule/edit
4. 删除权限功能: Admin/Rule/delete

#### 角色管理

五个权限控制：
1. 角色管理页面: Admin/Rule/rule_group
2. 新增角色功能: Admin/Rule/add_group
3. 修改角色功能: Admin/Rule/edit_group
4. 删除角色功能: Admin/Rule/delet_group
5. 分配角色功能: Admin/Rule/rule_distribution

#### 用户管理

三个权限控制：
1. 用户管理页面: Admin/User/index
2. 新增用户功能: Admin/User/add_user
3. 修改用户功能: Admin/User/edit_user

### 系统设置 (admin)

#### 文章管理

四个权限控制：
1. 文章管理页面: Admin/Article/index
2. 新增文章功能: Admin/Article/add_art
3. 编辑文章功能: Admin/Article/adi_art
4. 修改删除功能: Admin/Article/del_art

## ThinkPHP 5.0 权限管理

项目目录改为 rbac-test-5.0， 安装成功后 需要新建并先导入数据库，并在congfig.php配置资源路径:
   
```bash
 "web_root"   => "/rbac-test-5.0/public/static",
```

5.0 访问 [后台](http://localhost/rbac-test-5.0/public/index.php/Home/index/index/)

http://localhost/rbac-test-5.0/public/index.php 这个链接访问的是Index模块的Index控制器的index方法

默认账号：test 
默认密码：12345 

超级管理员 admin   
密码为：123456

