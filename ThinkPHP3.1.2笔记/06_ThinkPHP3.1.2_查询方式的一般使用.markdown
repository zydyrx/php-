#[查询方式的一般使用]
[TOC]
## 1.普通查询 - 重点
分两种查询
1.  字符串 - 不推荐
在IndexAction模块下写show()方法进行测试
```php
	public function show(){
		$m = M('User');//注意U大写
		//$arr = $m->select();	//成功查询出所有用户
		//前面讲的几种查询方法都能与where方法组合使用.如下:
		//$arr = $m->where('sex=1')->select(); //SELECT * FROM `th_user` WHERE ( sex=1 ) 
		//$arr = $m->where('name like %h%')->select();//查询无结果,SELECT * FROM `th_user` WHERE ( name like %h% ) 
		//$arr = $m->where('name like \'%h%\'')->select();//有结果:SELECT * FROM `th_user` WHERE ( name like '%h%' )
		//相信看出来了,因为用字符串方法很容易忽略单引号而导致查询无结果....不推荐原因
		var_dump($arr);	//输出数组查看查询结果.
		$this->display();
	}
```
2. 数组方式 - 推荐使用
```php
        //1.用数组进行多条件查询 ,默认都是用and连接
        //$data['sex'] = 1;
		//$data['name'] = 'huodeming2';
		//$arr = $m->where($data)->select();//SELECT * FROM `th_user` WHERE ( `sex` = 1 ) AND ( `name` = 'huodeming2' )
		//var_dump($arr);	//输出数组查看查询结果.
		//2.用['_logic'] 元素改变逻辑表达
		$data['sex'] = 1;
		$data['name'] = 'huodeming2';
		$data['_logic'] = 'or';	//增加逻辑表达为or
		$arr = $m->where($data)->select();//SELECT * FROM `th_user` WHERE ( `sex` = 1 ) OR ( `name` = 'huodeming2' )
		var_dump($arr);	//输出数组查看查询结果.
		//3.后面学习内容or 与 and 一起使用.
		//$data["_complex"]['sex'] = 1;
		//$data["_complex"]['name'] = 'huodeming2';
		//$data["_complex"]['_logic'] = 'or';	//增加逻辑表达为or
		//$data['id'] = 2;
		//$data['_logic'] = 'and';	
		//$arr = $m->where($data)->select();//SELECT * FROM `th_user` WHERE ( ( `sex` = 1 ) OR ( `name` = 'huodeming2' ) ) AND ( `id` = 2 )
```
## 2.表达式查询 - 重点

```table
表达式 | SQL表达式
EQ | 等于（=） 
NEQ | 不等于（<>） 
GT | 大于（>） 
EGT | 大于等于（>=） 
LT | 小于（<） 
ELT | 小于等于（<=） 
[NOT]LIKE | 模糊查询(注意not与like之间没空格)
[NOT] BETWEEN | （不在）区间查询 
[NOT] IN | （不在）IN 查询 
EXP  | 表达式查询，支持SQL语法 
```
示例如下：
EQ ：等于（=）
例如：
```php
$map['id'] = array('eq',100);
```
和下面的查询等效
```php
$map['id'] = 100;
```
表示的查询条件就是 id = 100

NEQ： 不等于（<>）
例如：
```php
$map['id'] = array('neq',100);
```
表示的查询条件就是 id <> 100

GT：大于（>）
例如：
```php
$map['id'] = array('gt',100);
```
表示的查询条件就是 id > 100

EGT：大于等于（>=）
例如：
```php
$map['id'] = array('egt',100);
```
表示的查询条件就是 id >= 100

LT：小于（<）
例如：
```php
$map['id'] = array('lt',100);
```
表示的查询条件就是 id < 100

ELT： 小于等于（<=）
例如：
```php
$map['id'] = array('elt',100);
```
表示的查询条件就是 id <= 100

[NOT] LIKE： 同sql的LIKE
例如：
```php
$map['name'] = array('like','thinkphp%');
```
查询条件就变成 name like 'thinkphp%'
如果配置了DB_LIKE_FIELDS参数的话，某些字段也会自动进行模糊查询。例如设置了：
'DB_LIKE_FIELDS'=>'title|content'
的话，使用
```php
$map['title'] = 'thinkphp';
```
查询条件就会变成 name like '%thinkphp%'
支持数组方式，例如
```php
$map['a'] =array('like',array('%thinkphp%','%tp'),'OR');
$map['b'] =array('notlike',array('%thinkphp%','%tp'),'AND');
```
生成的查询条件就是：
(a like '%thinkphp%' OR a like '%tp') AND (b not like '%thinkphp%' AND b not like '%tp')
```php
		//$m = M('User');
		//$data['username'] = array('like','%huo%');//SELECT * FROM `th_user` WHERE ( `username` LIKE '%huo%' )
		//$data['username'] = array('like',array('huo','de'));//SELECT * FROM `th_user` WHERE ( (`username` LIKE 'huo' OR `username` LIKE 'de') )
		//$data['username'] = array('like',array('huo','de'),'and');//SELECT * FROM `th_user` WHERE ( (`username` LIKE 'huo' AND `username` LIKE 'de') )
		//$arr = $m->where($data)->select();
		//var_dump($arr);
```
如果没有第三个参数,默认是or连接.

[NOT] BETWEEN ：同sql的[not] between， 查询条件支持字符串或者数组，例如：
```php
$map['id'] = array('between','1,8');
```
和下面的等效：
```php
$map['id'] = array('between',array('1','8'));
```
查询条件就变成 id BETWEEN 1 AND 8

[NOT] IN： 同sql的[not] in ，查询条件支持字符串或者数组，例如：
```php
$map['id'] = array('not in','1,5,8');
```
和下面的等效：
```php
$map['id'] = array('not in',array('1','5','8'));
```
查询条件就变成 id NOT IN (1,5, 8)
EXP：表达式，支持更复杂的查询情况
例如：
```php
$map['id'] = array('in','1,3,8');
```
可以改成：
```php
$map['id'] = array('exp',' IN (1,3,8) ');
```
exp查询的条件不会被当成字符串，所以后面的查询条件可以使用任何SQL支持的语法，包括使用函数和字段名称。查询表达式不仅可用于查询条件，也可以用于数据更新，例如：
```php
$User = M("User"); // 实例化User对象
//要修改的数据对象属性赋值
$data['name'] = 'ThinkPHP';
$data['score'] = array('exp','score+1');// 用户的积分加1
$User->where('id=5')->save($data); // 根据条件保存修改的数据
```

## 3.区间查询 - 重点
```php
		//$data['id'] = array(array('gt',4),array('lt',8));//SELECT * FROM `th_user` WHERE ( (`id` > 4) AND (`id` < 8) )
		$data['id'] = array(array('gt',8),array('lt',4),'or');//SELECT * FROM `th_user` WHERE ( (`id` > 8) OR (`id` < 4) )
		//如果是多条件可以继续写下去,最后一个参数用or或and就行.
```

## 4.统计查询 - 重点
count
```php
		//$count = $m->count(); //SELECT COUNT(*) AS tp_count FROM `th_user`
		
		$data['id'] = array('gt',3);
		$count = $m->where($data)->count();//SELECT COUNT(*) AS tp_count FROM `th_user` WHERE ( `id` > 3 )
		echo $count;
```
max
```php
    $max = $m->max('id');//SELECT MAX(id) AS tp_max FROM `th_user`
```
min(同max)
avg
```php
    $avg = $m->avg('id');
	echo $avg;//SELECT AVG(id) AS tp_avg FROM `th_user`
```
sum
```php
		$sum = $m->sum('id');
		echo $sum;//SELECT SUM(id) AS tp_sum FROM `th_user`
```

## 5.SQ直接查询 - 重点
在一些查询太复杂的时候,我们可以直接用SQL语句查询
###  query()方法:主要处理读取数据
```php
		$m = M('User');
		$arr = $m->query("select * from th_user where id > 2");//select * from th_user where id > 2
		var_dump($arr);
```
注:query方法成功返回查询的结果值,失败boolean的false;
###  execute()方法,主要处理非查询数据
```php
		$m = M('User');
		$result = $m->execute("insert into th_user(username,sex) values('huodeming11',1)");
		echo $result;
```
注:execute方法成功返回引响行数,失败boolean的false;



