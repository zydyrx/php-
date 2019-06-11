# ThinkPHP中CURD特性
[TOC]
##  一.ThinkPHP3.1.2中CURD介绍 - 了解
便用模型的实例可以对数据进行操作,操作的工作一般是就是对数据库进行增删改查:CURD
```table
操作 | 相应表示 | 实现方法
增 | C  | $m->add()
删 | D | $m->delete()
改 | U | $m->save()
查 | R | $m->select()
```
## 二.ThinkPHP3.1.2读取数据 - 重点
### select()方法 - 获取所有数据
```php
//对数据的读取
$m = new Model('User');//$m = new Model('表名');
//$m = M("User");//等同于上面方法.
$arr = $m->select();//查询结果以数组返回.
```
###1. find()方法 - 获取单条数据
```php
//对数据的单条读取
$m = new Model('User');//$m = new Model('表名');
//$m = M("User");//等同于上面方法.
		/*
		 mysql> select * from th_user;
		+----+-----------+-----+
		| id | username  | sex |
		+----+-----------+-----+
		|  1 | huodeming |   1 |
		|  2 | meimei    |   0 |
		|  4 | 1004      |   0 |
		|  6 | 1006      |   1 |
		+----+-----------+-----+
		 */
//$arr = $m->find();//查询出ID为1的数据
//$arr = $m->find(1);//查询出ID为1的数据
//$arr = $m->find(2);//查询出ID为2的数据
//$arr = $m->find(3);//无结果返回
//$arr = $m->find(4);//查询出ID为4的数据
//$arr = $m->find(5);//无结果返回
//$arr = $m->find(6);//查询出ID为6的数据
```

### getField(字段名)方法 - 获取一个具体的字段
 ```php
 		//$arr = $m->getField('username'); //返回ID为1的username字希
		//练习:查出ID为4的username字段的值
		$arr = $m->where('id=4')->getField('username');//相当于sql语句中的查询条件过滤:通过页面调试trace可以得到语句:SELECT `username` FROM `th_user` WHERE ( id=4 )
		var_dump($arr);
 ```

## 三.ThinkPHP3.1.2插入数据 - 重点
```php
		//插入数据,按需求给字段赋值,然后执行add()方法.
		$m->id = 8;//给ID字段赋值
		$m->username = "1008";//给姓名字段赋值
		$m->sex = 0;//给性别字段赋值
		$m->add();//把值修改到数据库,通过页面trace可以看到语句:INSERT INTO `th_user` (`id`,`username`,`sex`) VALUES (8,'1008',0)
		//注意:插入数据有返回值,是它的ID号
```

## 四.ThinkPHP3.1.2删除数据 - 重点
```php
		//删除数据:注意删除条件
		$m->delete(8);//删除ID字段为8的数据行:DELETE FROM `th_user` WHERE ( `id` = 8 )
		$m->delete();//如果不加任何条件,则不执行删除.
		$m-> where('id=6')->delete();//DELETE FROM `th_user` WHERE ( id=6 ) 
		//有返回值,受引响行数.
```

## 五.ThinkPHP3.1.2更新数据 - 重点
```php
		//更新数据
		//$data['id']=4;
		//$data['username']='李四';
		//$data['sex'] = 1;
		//$count = $m->save($data);
		//echo $count;//输出 1 是受引响行数 UPDATE `th_user` SET `username`='李四',`sex`=1 WHERE ( `id` = 4 ) 
		
		//但下面语句没有更新,没ID列.
		
		$data['username'] = '王二';
		$data['sex'] = 1;
		$count = $m->save($data);
		echo $count;//没有输出
```

