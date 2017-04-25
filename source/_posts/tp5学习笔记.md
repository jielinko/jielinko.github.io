## 1,执行php命令行程序，显示无法识别  

方法:配置php环境变量:因为安装了wamp，所以配置wamp中的php的路径

计算机 - 右键 - 高级系统设置 -  环境变量 - Jielinko 的用户变量 - PATH - 新增 D:\anzhuanglujin\wamp\bin\php\php5.5.12;(可以不需要文件夹下面的php.exe) - 重新打开CMD - 运行 php -V
显示php版本号 - 安装成功！

## 2,怎样在单一系统上运行多个网站,使用apache来配置虚拟主机

1,先找到 httpd.conf 文件，wamp/bin/apache/apache2.4.9/conf/

2,启用Apache的vhost功能, 修改文件

``` 
#Virtual hosts

#Include conf/extra/httpd-vhosts.conf
```
改成

```
#Virtual hosts

Include conf/extra/httpd-vhosts.conf
```
让Apache支持 vhosts

3,配置Apache的vhost功能,找到 conf/extra/httpd-vhosts.conf 文件打开,添加如下代码,如果需要多个也可以复制多个。

```
<VirtualHost *:80>
    DocumentRoot "D:/anzhuanglujin/wamp/www/tp5/public"
    ServerName tp5.com
</VirtualHost>
```

![](http://i.imgur.com/oloHbA0.png)

4,修改本机的hosts文件把tp5.com指向本地127.0.0.1

进入C盘的C:\Windows\System32\drivers\etc这个目录下面找到hosts这个文件

添加如下代码:

```
127.0.0.1 tp5.com
```

使得 tp5.com 指向本地 127.0.0.1

5,重启Apache后，输入tp5.com 来访问。

访问成功！

6,此时可能影响到其他项目的访问，恢复的话需要

注释 hosts 文件的

```
#127.0.0.1 tp5.com
```

和 更改 conf/extra/httpd-vhosts.conf

```
<VirtualHost *:80>
    DocumentRoot "D:/anzhuanglujin/wamp/www/"
    ServerName tp5.com
</VirtualHost>
```
## 3,修改工程文件夹路径和目录

1，把 pubilc 文件夹中的index.php 

```
// 定义应用目录
define('APP_PATH', __DIR__ . '/../application/');
// 加载框架引导文件
require __DIR__ . '/../thinkphp/start.php';
```
修改为:

```
// 定义应用目录为apps
define('APP_PATH', __DIR__ . '/apps/');
// 加载框架引导文件
require __DIR__ . '/think/start.php';
```

2,修改上图对应的 框架目录 和 应用目录 路径和目录名。

3,因命令行程序 think 与 框架目录 冲突，暂时改为 think-revise

修改完成后工程目录结构:

```
tp5
├─index.php       应用入口文件
├─apps            应用目录
├─public          资源文件目录
├─runtime         运行时目录
└─think           框架目录
```
4，因入口文件index.php 直接位于工程目录，httpd-vhosts.conf 文件修改为:

```
<VirtualHost *:80>
    DocumentRoot "D:/anzhuanglujin/wamp/www/tp5/"
    ServerName tp5.com
</VirtualHost>
```

5, 访问 tp5.com 成功 

##4,生成URL地址

定义路由规则之后，我们可以通过Url类来方便的生成实际的URL地址（路由地址）

 在 Index.php 添加一下代码

```
namespace app\index\controller;

use think\Url;

class Index 
{
	public function index (){
		return Url::build('blog/read','name=thinkphp');
	}
}
```
注意: build方法的第一个参数使用路由定义中的完整路由地址

便会生成 实际的URL地址
















