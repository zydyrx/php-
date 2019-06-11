#[ThinkPHP 3.1.2 MVC模式和URL访问]
[TOC]
## 一.什么是MVC - 了解
     M:   Model 编写model类,对数据进宪操作
     V:    编写html文件,页面呈现
     C:   编写类文件(User5Action.class.php)

## 二.ThinkPHP的MVC特点 - 了解
三者互不依赖,在ThinkPHP中比较灵活.后期代码说明.

## 三.ThinkPHP的MVC 对应的目录 - 了解
    M   项目目录/应用目录/Lib/Model
    V   项目目录/应用目录/Tpl
    C   项目目录/应用目录/Lib/Action

## 四.URL访问C(类文件) - 了解
我们访问:[http://localhost/thinkPHP_V3.1.2/index.php](http://localhost/thinkPHP_V3.1.2/index.php)
实际上是访问了:[http://localhost/thinkPHP_V3.1.2/index.php/Index/index](http://localhost/thinkPHP_V3.1.2/index.php/Index/index),具体是Index模块下的index方法.文件地址:`项目目录/ThinkPHP/Home/Lib/Action/IndexAction.class.php`里面的index方法.以后建用户模块就是UserAction.class.php,商品模块就是GoodsAction.class.php
为了证明访问的文件.我们可以把`项目目录/ThinkPHP/Home/Lib/Action/IndexAction.class.php`里面的index方法中内容改成 `echo 'Hello World!';`可以测试效果.
练习:
```php
<?php
// 测试访问的页面,改写下index()方法中内容.
class IndexAction extends Action {
    public function index(){
	//$this->show('<style type="text/css">*{ padding: 0; margin: 0; } div{ padding: 4px 48px;} body{ background: #fff; font-family: "微软雅黑"; color: #333;} h1{ font-size: 100px; font-weight: normal; margin-bottom: 12px; } p{ line-height: 1.8em; font-size: 36px }</style><div style="padding: 24px 48px;"> <h1>:)</h1><p>欢迎使用 <b>ThinkPHP</b>！</p></div><script type="text/javascript" src="http://tajs.qq.com/stats?sId=9347272" charset="UTF-8"></script>','utf-8');
    echo "Hello World!";    //注释上面行,增加本行测试
	}
	//测试URL,增加一个show()方法
	public function show(){
		echo "访问了Index模块下的show()方法!";
	}
}
```
## 五.URL的四种访问方式 - 重点
### 1.PATHINFO模式 - 重点!!!
    http://域名/项目名/入口文件/模块名/方法名/键1/值1/键2/值2
```php
//我们在(项目目录/ThinkPHP/Home/Lib/Action/IndexAction.class.php)里面增加一个welcome方法测试
	public function welcome(){
    	echo '欢迎您:'.$_GET['name'];
	}
    //访问URL: http://localhost/thinkPHP_V3.1.2/index.php/index/welcome?name=huodeming 完全可以访问,但我们在PHTHINFO模试下不这么访问.我们以URL: http://localhost/thinkPHP_V3.1.2/index.php/index/welcome/name/huodeming 进行访问.(键值对模式:键/值),如果有多个参数则接着写,用/隔开就行.
    //通过修改配置文件,还可以写成: http://localhost/thinkPHP_V3.1.2/index.php/Index-welcome-name-huodeming 的样子(把/改成-)
    //配置文件:项目目录/Home/Conf/config.php 增加一行: 'URL_PATHINFO_DEPR'=>'-',
    //如果访问不成功,请删除Home目录下的缓存(Runtime)目录,如果每次都删太麻烦,我们可以开启调试模式,在index.php下开启,在开发时用,正试发布后关闭.开启后不创建缓存.
    define('APP_DEBUG',true);
```
开启调式模式
```PHP
<?php
	//要便用ThinkPHP3.1.2,分三步就可以调用
	//1.确定应用名称,一般项目有两个,前台(HOME)与后台(ADMIN)
	//define('APP_NAME','Home');
	//2.确定应用路径,让得后面加/,而且必须有,不信自己实验
	//define('APP_PATH','./Home/');
	
	//开启调试模式,在开发中非常重要
	define('APP_DEBUG',TRUE);
	//3.引入模板,请严格区分大小写,因为怕项目迁移到LUNUX下去,是严格区分大小写的.
	//require './ThinkPHP/ThinkPHP.php';
?>
```
### 2.普通模式 - 不建议便用
 http://域名/项目名/入口文件?m=模块名&a=方法名
    http://localhost/thinkPHP_V3.1.2/index.php?m=Index&a=show
    http://localhost/thinkPHP_V3.1.2/index.php?m=Index&a=index

 http://域名/项目名/入口文件?m=模块名&a=方法名&键1=值1&键2=值2
    http://localhost/thinkPHP_V3.1.2/index.php?m=Index&a=welcome&name=huodeming

### 3.REWRITE模式 - 不建议便用,太麻烦
1.支持它需要修改Apache的httpd.conf配置文件(查找rewrite 找到 #LoadModule rewrite_module modules/mod_rewrite.so 去掉前面的#就行)
2.需要写一个.htaccess文件存放于项目根目录下与index.php一个目录,里面是重写规则,详见某度.内容如下
```php
<IfModule mod_rewrite.c>
	RewriteEngine on
	RewriteCond %{REQUEST_FILENAME} !-d
	RewriteCond %{REQUEST_FILENAME} !-f
	RewriteRule ^(.*)$ index.php/$1 [QSA,PT,L]
</IfModule>
```
### 4.兼容模式 - 不建议使用
 http://域名/项目名/入口文件?s=模块名/方法名
http://localhost/thinkPHP_V3.1.2/index.php?s=Index/welcome/name/huodeming






