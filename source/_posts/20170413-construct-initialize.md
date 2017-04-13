---
title: （转载）彻底搞懂_initialize()与__construct()的用法
date: 2017-04-13 15:42:57
categories: # 这里写的分类会自动汇集到 categories 页面上，分类可以多级
 - web开发
tags: # 这里写的标签会自动汇集到 tags 页面上
 - php
 - 后端开发
 - thinkPHP
---

[转载链接](http://blog.csdn.net/cnyygj/article/details/53647934)

## Thinkphp自带的Controlle类

地址为ThinkPHP\Library\Think\Controller.class.php

```bash   

    public function __construct(Request $request = null)
    {
        if (is_null($request)) {
            $request = Request::instance();
        }
        $this->view    = View::instance(Config::get('template'), Config::get('view_replace_str'));
        $this->request = $request;

        // 控制器初始化
        $this->_initialize();

        // 前置操作方法
        if ($this->beforeActionList) {
            foreach ($this->beforeActionList as $method => $options) {
                is_numeric($method) ?
                $this->beforeAction($options) :
                $this->beforeAction($method, $options);
            }
        }
    }
```
<!-- more -->

从Controller类中的构造函数中可以知道，该构造函数会判断对象中是否有_initialize方法，如果有，就执行先_initialize方法.

因此，如果我们在自己定义的控制器中，

## 有重写构造函数：

### 在重写的构造中有实现父类的构造函数（parent::construct()）

 如果该控制器中有定义_initialize()方法，那么，我们在调用该控制器中的方法时，会先执行_initialize()方法，然后再执行我们需要的方法

```bash   
<?php  
namespace Home\Controller;  
use think\Controller;  
class Index extends Controller {  
  
    public function __construct() {  
        parent::__construct();  
        self::b();  
        echo '我是构造<br />';  
    }  
    public function _initialize() {  
        echo '我先来<br />';  
        // parent::_initialize();  
    }  
    public function index(){  
        self::b();  
        echo '这是index';  
    }  
  
    public function b() {  
        echo 'bbbb<br />';  
    }  
}  
  
/* 
当执行index方法时，打印结果： 
我先来 
bbbb 
我是构造 
bbbb 
这是index 
*/  
```

### 在重写的构造中没有实现父类的构造函数

 执行方法时，定义的_initialize()方法则没有作用（不会在执行方法时，先执行_initialize方法）

```bash
<?php  
namespace Home\Controller;  
use think\Controller;  
class Index extends Controller {  
  
    public function __construct() {  
        // parent::__construct();  
        self::b();  
        echo '我是构造<br />';  
    }  
    public function _initialize() {  
        echo '我先来<br />';  
        // parent::_initialize();  
    }  
    public function index(){  
        self::b();  
        echo '这是index';  
    }  
  
    public function b() {  
        echo 'bbbb<br />';  
    }  
}  
  
/* 
当执行index方法时，打印结果： 
bbbb 
我是构造 
bbbb 
这是index 
*/  
```

注：这里面的所说的先执行_initialize()方法，是在parent::__construct();前没有任何函数调用，如果你非得在parent::__construct();前来个self::b()，那没得说，肯定是先执行b(),不过一般不这样写，在实现父类的构造函数前一般没有任何输出和配置
再有，如果是继承，如果父类有构造函数，子类在其构造函数一般先把父类的构造函数先初始化，确保代码的原始性和完整性

## 没有重写构造函数，也就是说在我们定义的控制器中没有声明构造函数

这种情况，如果在控制器中有定义_initialize()方法，则当我们调用其他方法时，会先调用_initialize()方法

```bash
<?php  
namespace Home\Controller;  
use think\Controller;  
class Index extends Controller {  
  
    // public function __construct() {  
    //  // parent::__construct();  
    //  self::b();  
    //  echo '我是构造<br />';  
    // }  
    public function _initialize() {  
        echo '我先来<br />';  
        // parent::_initialize();  
    }  
    public function index(){  
        self::b();  
        echo '这是index';  
    }  
  
    public function b() {  
        echo 'bbbb<br />';  
    }  
}  
  
/* 
当执行index方法时，打印结果： 
我先来 
bbbb 
这是index 
*/  
```

## _initialize()还可以用来继承

父类 Base

```bash
<?php  
namespace Home\Controller;  
use think\Controller;  
class Base extends Controller {  
    public function __construct() {  
        parent::__construct();  
  
        echo '我是父类<br />';  
    }  
  
    public function _initialize() {  
        echo '我先来<br />';  
    }  
  
    public function a() {  
        echo 'aaaa<br />';  
    }  
}  
```

子类 Index

```bash
<?php  
namespace Home\Controller;  
use think\Controller;  
class Index extends Base {  
  
    public function __construct() {  
        parent::__construct();  
        self::b();  
        echo '我是构造<br />';  
    }  
    public function _initialize() {  
        parent::_initialize();  
        echo '我是子类先来<br />';  
    }  
    public function index(){  
        self::b();  
        echo '这是index';  
    }  
  
    public function b() {  
        echo 'bbbb<br />';  
    }  
}  
  
/* 
当执行index方法时，打印结果： 
我先来 
我是子类先来 
我是父类 
bbbb 
我是构造 
bbbb 
这是index 
*/  
```

注意：如果父类的构造函数中没有parent::construct()，定义的_initialize()也不起作用

那么，同时存在__construct()（该构造函数初始化了父类的构造函数）和_initialize() ，到底先执行哪个呢？
答案是——先执行_initialize()方法，也就是说，在满足条件下，_initialize()函数是在任何方法执行之前，都要执行的，包括构造函数，
当然，如果你在要执行的方法中又调用的另一个或者多个方法，在另外调用那些方法时，_initialize()方法是不会再执行了，它关联的是你首次调用的方法，也就是说，方法里面干什么，它管不着了。

##结语

+ _initialize() 为thinkphp封装的函数,__construct 为PHP类的构造函数,调用子类的构造函数会覆盖父类的构造函数。同时调用则还是需要调用 parent::__construct();
+ ThinkPHP中的_initialize()的出现只是方便程序员在写子类的时候避免频繁的使用parent::__construct(),同时正确的调用框架内父类的构造函数;
+ 所以,我们在ThinkPHP中初始化子类的时候要用_initialize(),而不用__construct();
+ 调用子类_initialize()的同时也会调用父类的构造函数 __construct() ,但是不能调用父类的_initialize();
+ 如果想调用父类的_initialize()，则还是需要在之类的_initialize里面加上 parent::_initialize();
+ 可以通过修改框架将_initialize()函数修改为自己喜欢的函数;

参考链接:

[thinkphp篇之__construct()和__initialize()](http://blog.csdn.net/cnyygj/article/details/53647934)
[ThinkPHP中_initialize()与__construct()用法](http://www.52codes.net/article/721.html)
[细说ThinkPHP的_initialize()与__construct()用法](http://www.52codes.net/article/881.html)