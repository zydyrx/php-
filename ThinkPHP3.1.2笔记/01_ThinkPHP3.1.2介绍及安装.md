# [ThinkPHP3.1.2介绍及安装] 
[TOC]
## 一. ThihkPHP的介绍 - 了解
一个基于MVC的PHP框架,简单介绍MVC
> M:   Model 中文:模型 - 负责数据库操作
> V:    View 视图(模板) - 负责前台页面的显示
> C:    Controller 控制器(模块) - 描述功能
想用此版本,PHP版本必须不得太低,建议用5.3版本,具体参见官方说明

## 二.ThinkPHP的获取 - 了解
去官方网站下载:[http://thinkphp.cn/down.html](http://thinkphp.cn/down.html)
## 三.ThinkPHP核心文件介绍 - 了解
```
	├─ThinkPHP.php 框架入口文件
	├─Common  框架公共文件
	├─Conf  框架配置文件
	├─Extend 框架扩展目录
	├─Lang 核心语言包目录
	├─Lib 核心类库目录
	│  ├─Behavior 核心行为类库
	│  ├─Core 核心基类库
	│  ├─Driver 内置驱动
	│  │  ├─Cache 内置缓存驱动
	│  │  ├─Db 内置数据库驱动
	│  │  ├─TagLib 内置标签驱动
	│  │  └─Template 内置模板引擎驱动
	│  └─Template 内置模板引擎
	└─Tpl 系统模板目录
```

## 四.实验环境搭建 - 了解
建议搭建wampserver,当然phpstudy等也是不错的选择,建议用wampserver2.2e或以上版本,我用的2.4版本
## 五.项目搭建 - 重点
1.在www目录下创建ThinkPHP_V3.1.2目录,把3.1.2的核心包解压进去,www/ThinkPHP_V3.1.2/ThinkPHP/
2.在ThinkPHP_V3.1.2目录下创建index.php,内容如下:
```php
<?php
	//要便用ThinkPHP3.1.2,分三步就可以调用
	
	//1.确定应用名称,一般项目有两个,前台(HOME)与后台(ADMIN)
	define('APP_NAME','Home');
	//2.确定应用路径,让得后面加/,而且必须有,不信自己实验,会很乱
	define('APP_PATH','./Home/');
	//3.引入核心文件,请严格区分大小写,因为怕项目迁移到LUNUX下去,是严格区分大小写的.
	require './ThinkPHP/ThinkPHP.php';
?>
```
为了测试,再创建一个后台应用:ThinkPHP3.1.2/Admin.php,内容如下:
```php
	//1.确定应用名称,一般项目有两个,前台(HOME)与后台(ADMIN)
	//define('APP_NAME','Home');--第一步好像注释也不报错,后面再说.因为3.2.3中就没有这一步.
	//2.确定应用路径,让得后面加/,而且必须有,不信自己实验
	define('APP_PATH','./Admin/');
	//3.引入核心文件,请严格区分大小写,因为怕项目迁移到LUNUX下去,是严格区分大小写的.
	require './ThinkPHP/ThinkPHP.php';
?>
```
### 项目目录结构及说明 - 了解
		Home 前台应用文件夹
		├─Common 项目公共文件目录
		├─Conf 项目配置目录
		├─Lang 项目语言目录
		├─Lib 项目类库目录
		│  ├─Action Action类库目录
		│  ├─Behavior 行为类库目录
		│  ├─Model 模型类库目录
		│  └─Widget Widget类库目录
		├─Runtime 项目运行时目录
		│  ├─Cache 模板缓存目录
		│  ├─Data 数据缓存目录
		│  ├─Logs 日志文件目录
		│  └─Temp 临时缓存目录
		└─Tpl 项目模板目录
## 六. 补充说明 - 了解
后期我们用到的前台与后台公共文件我们放哪?   
   可以参考: ThinkPHP3.1.2/public/
我们后期上传的一些文件呢?
    ThinkPHP3.1.2/uploads/
以此类推....
本章学习重点是项目搭建.注意要能自己动手完成.








