# [ThinkPHP 3.1.2 输出和模型使用]
[TOC]
## 一.ThinkPHP的输出 - 重点
1.  通过echo等原生的方式输出.但不符合MVC模式.我们只需要方法中写一个方法,就可以将模板文件输出.
2. 通过display()方法输出,(如果想分配变量,可使用assign()方法).
```php
//例如:我们在Index/index方法中写:$this->display(); 这时报错,'模板不存在[./Home/Tpl/Index/index.html]'
    public function index(){
     //echo "Hello World!";
     $this->display();
	}
```
按要求创建[./Home/Tpl/Index/index.html]
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		<h1>Hello index</h1>
	</body>
</html>
```
同理练习:创建其他方法对应的模板文件.下面练习给模板文件传变量
```php
    public function index(){
    	$name = '霍老';
    	$this->assign('data',$name);
    	$this->display();
	}
```
对应的index.html文件中内容
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		<h1>Hello {$data}</h1>
	</body>
</html>
```
由于ThinkPHP模板文件中的定界符是{},往往我们在写JS或CSS代码时也用到,所以我们要改ThinkPHP的定界符.
3.  修改左右定界符
修改[项目目录/Home/Config.php]
'TMPL_L_DELIM'=>'<{',            //修改左定界符
'TMPL_R_DELIM'=>'}>',            //修改右定界符
## 二.ThinkPHP模型的使用 - 重点
需要要方法中new Model('表名')的型式操作数据库
```php
    public function index(){
        //操作数据库,前提是需要配置好数据库信息,建好库,表
    	$m = new Model('User');
		$arr = $m->select();
		var_dump($arr);

    	$name = '霍德明';
    	$this->assign('name',$name);
    	$this->display();
	}
```
数据库配置
```php
    //'URL_PATHINFO_DEPR'=>'-',   //配置URL分隔符
    'TMPL_L_DELIM'=>'<{',            //修改左定界符
    'TMPL_R_DELIM'=>'}>',            //修改右定界符
	//配置数据库[见下面部分]
	'DB_TYPE'=>'mysql',             	//数据库引擎
	'DB_HOST'=>'127.0.0.1',        		//服务器IP或域名
	'DB_PORT'=>'3306',             		 //端口
	'DB_NAME'=>'thinkphp',      		//数据库名
	'DB_USER'=>'root',                  //登陆用户
	'DB_PWD'=>'123',                    //密码,没有写''
	'DB_PREFIX'=>'th_',             	//表前缀	
```
上面的配置项可以写成一句DNS方式,保留表前缀
```php
	//'DB_TYPE'=>'mysql',             	//数据库引擎
	//'DB_HOST'=>'127.0.0.1',        		//服务器IP或域名
	//'DB_PORT'=>'3306',             		 //端口
	//'DB_NAME'=>'thinkphp',      		//数据库名
	//'DB_USER'=>'root',                  //登陆用户
	//'DB_PWD'=>'123',                    //密码,没有写 ''
    'DB_PREFIX'=>'th_',             	//表前缀	
    'DB_DSN'=>'mysql://root:123@127.0.0.1:3306/thinkphp',
```
如果两种数据库配置方式都存在,以DSN方式优先.
### 大M方法
M方法等同于 new Mode()语句.thinkphp提供的简便方法
```php
    public function index(){
        //操作数据库,前提是需要配置好数据库信息,建好库,表
    	//$m = new Model('User');
    	$m = M('User');
		$arr = $m->select();
		var_dump($arr);
//    	$name = '霍德明';
//    	$this->assign('name',$name);
//   	$this->display();
	}
```
### 模型增删改查:CDUR
便用模型的实例可以对数据进行操作,操作的工作一般是就是对数据库进行增删改查:CURD
```table
操作 | 相应表示 | 实现方法
增 | C  | $m->add()
删 | D | $m->delete()
改 | U | $m->save()
查 | R | $m->select()
```
## 补充
### 控制器从M层拿数据->V层遍历数组演示.
我们从数据库模型中查询到一些数据,结果是数组,把它传给模板来处理
```php
//php文件代码
<?php
// 本类由系统自动生成，仅供测试用途
class IndexAction extends Action {
    public function index(){
    	//我们要把从数据库读取的数据给前台去显示
    	$m = new Model('User');
		$arr = $m->select();
		var_dump($arr);
		$this->assign('data',$arr);
		//$this->display();

    	$name = '霍德明';
    	$this->assign('name',$name);
    	$this->display();
	}
} 
```
注意volist标签:<volist name='data' id='vo'> name是传过来的变量名,id则是每次循环时的行.
```html
//前台模板文件
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		<h1>Hello <{$name}></h1>
		<!--
        	作者：huodeming1@163.com
        	时间：2017-04-26
        	描述：thinkphp模板中遍历数组的标签:volist标签
        -->
		<volist name='data' id='vo'>
			<{$vo.id}>----<{$vo.username}>----<{$vo.sex}><br>
		</volist>
	</body>
</html>
```
### 开启调试功能中的page_trace
需要分两步配置
1. 开启调试功能
 在index.php中申明常量:
 define('APP_DEBUG',true);
2. 设置配置文件,开始页面trace
 'SHOW_PAGE_TRACE'=>true,//开启页面trace
注意:如果没有加载模板,则页面trace的右下角图标不显示.所以要有display()方法加载模板才有效.
    





