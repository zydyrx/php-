# [模板变量详解]
[TOC]
## 1.变量输出
1.标量输出
 boolean 布尔型
 integer 整型
 float 浮点型（也做double）
string (字符串)

2.数组输出
 索引数组 
```php
<{$arr[1]}>
```
关联数组
```php
<{$arr['key1']}>
<{$arr.key1}>
```
3.对象输出
```php
<{$obj:name}>
<{$obj->name}>
```

## 2.系统变量
参见手册:模板引擎->系统变量
```table
用法 |  含义 |  例子 
$Think.server | 获取$_SERVER |  {$Think.server.php_self} 
$Think.get |  获取$_GET |  {$Think.get.id} 
$Think.post |  获取$_POST |  {$Think.post.name} 
$Think.request |  获取$_REQUEST |  {$Think.request.user_id} 
$Think.cookie |  获取$_COOKIE |  {$Think.cookie.username} 
$Think.session |  获取$_SESSION |  {$Think.session.user_id} 
$Think.config |  获取系统配置参数 |  {$Think.config.app_status} 
$Think.lang |  获取系统语言变量 |  {$Think.lang.user_type} 
$Think.const |  获取系统常量 |  {$Think.const.app_name}或{$Think.APP_NAME} 
$Think.env  | 获取环境变量 |  {$Think.env.HOSTNAME} 
$Think.version |  获取框架版本号 |  {$Think.version} 
$Think.now |  获取当前时间 |  {$Think.now} 
$Think.template |  获取当前模板 |  {$Think.template} 
$Think.ldelim |  获取模板左界定符 |  {$Think.ldelim} 
$Think.rdelim |  获取模板右界定符 |  {$Think.rdelim} 
```

## 3.使用函数
```php
    //在控制器中传一个值
    $this->assign('name','霍德明');
    //在模板中输出
    <{$name}>
    //下面一行相当于调用了md5函数.
    <{$name|md5}>
    //多个参数用=参数,用三个#号占位,表示$time所在位置
    <{$time|data='Y-m-d H:i:s',###}>
    //如想查看,找编译后的文件在Runtime\Cache目录下.
```

## 4.默认值
<{$name|default='这是默认值'}>

## 5.运算符
//如果传给模板一个数字,可以进行运算. + - * / %
<{$num+1}>


