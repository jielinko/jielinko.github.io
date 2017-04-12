---
title: elitename 项目总结
date: 2017-04-08 19:45:12
tags:
---
<input type=image 自动提交表单问题

自动提交表单
回车和点击时会自动提交表单，查看它的定义：创建一个图像控件，该控件点击后将导致表单立即被提交。这就是为什么表单提交，但JS中的验证等没有执行的原因。
在网上查了下，处理方法后加上return false可以解决这个问题，即onclick="check();reture false;"在项目中用了下，果然可以。
但又遇到了个问题，当我页面中有多个,比如要录入一个对象，我要先查出一个它必填项，其中查询键没有加onclick，而是用Jquery直接写成$("#search").click(function (){...});，保存键,这样查询可以正常运行，而点保存键时，又出现没有进行JS验证而自动提交表单的问题！！！
多个都使用onclick="method();return false;"写法就不会出问题。