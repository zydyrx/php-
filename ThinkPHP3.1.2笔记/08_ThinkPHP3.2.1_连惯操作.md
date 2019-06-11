# [08_ThinkPHP3.1.2_连惯操作]
[TOC]
## 模拟写一个连惯操作的类 - 练习
```php
	class Model{
		private $tbName = null;//接收表名
		private $where = null;//接收条件字符串
		private $sql = null;//存储查询字符串
		function __construct($tbName){
			$this->tbName = $tbName;
			mysql_connect('localhost','root','123');
			mysql_select_db("thinkphp");
		}
		function select(){
			$arr = array();
			$sql = "select * from th_".strtolower($this->tbName);//转小写
			if($this->where){
				$sql = $sql." where ".$this->where;
			}
			$this->sql = $sql;
			//echo $sql;
			$result = mysql_query($sql);
			if($result && mysql_num_rows($result)>0){
				while($res = mysql_fetch_assoc($result)){
					$arr[] = $res;
				}
			}
			return $arr;
		}
		//传一个字符串进去,返回$this对象进行连惯操作.
		function where($where){
			$this->where = $where;
			return $this;
		}
		//写一个方法输出字符串
		function getSql(){
			echo '<hr>'.$this->sql;
		}
	}
	//调用上面类代码,模拟实现select()方法.
	$m = new Model('user');
	//$arr = $m->select();
	
	//$m2 = $m->where("id > 2");
	//$arr = $m2->select();
	
	$arr = $m->where("id>2")->select();
	var_dump($arr);
	$m->getSql();
```
## 常用的连惯操作 - 重点
注意:最后放select()或find()方法.下面一些存放顺序可随意.
### 1.where - 条件
```php
			$m = M('User');
			//上节大量进行where查询,所以不多写
			$arr = $m ->where('id > 2')->select();//SELECT * FROM `th_user` WHERE ( id > 2 )
			var_dump($arr);
			$this->show('<hr>test show()');//如果不想创建模板文件调用display(),也可以用show()方法输出HTML代码.
```
### 2.order - 排序
```php
		public function test(){
			$m = M('User');
			//$arr = $m ->where('id > 2')->select();//SELECT * FROM `th_user` WHERE ( id > 2 )
			
			//$arr = $m->order('id desc')->where('id > 2')->select();//SELECT * FROM `th_user` WHERE ( id > 2 ) ORDER BY id desc
			//$arr = $m->where('id > 2')->order('id desc')->select();//SELECT * FROM `th_user` WHERE ( id > 2 ) ORDER BY id desc
			$arr = $m->where('id > 2')->order(array('id'=>'desc','sex'=>'asc'))->select();//数组方式:SELECT * FROM `th_user` WHERE ( id > 2 ) ORDER BY `id` desc,`sex` asc
			
			var_dump($arr);
			$this->show('<hr>test show()');//如果不想创建模板文件调用display(),也可以用show()方法输出HTML代码.
		}
```

### 3.limit - 限制结果
```php
		public function test(){
			$m = M('User');
			//限制结果
			//$arr = $m->limit('3')->select();//SELECT * FROM `th_user` LIMIT 3	//也相当于limit(0,3)//取最前面三条
			//$arr = $m->limit('2,3')->select();//SELECT * FROM `th_user` LIMIT 2,3
			$arr = $m->limit(2,3)->select();//SELECT * FROM `th_user` LIMIT 2,3
			var_dump($arr);
			$this->show('<hr>test show()');//如果不想创建模板文件调用display(),也可以用show()方法输出HTML代码.
		}
```

### 4.field - 取部分列(设置字段)
```php
		public function test(){
			$m = M('User');
			//设置字段
			//字符串
			//$arr = $m->field("username")->select();//SELECT `username` FROM `th_user`
			//$arr = $m->field("username,sex")->select();//SELECT `username`,`sex` FROM `th_user`
			//$arr = $m->field("username 姓名,sex 性别")->select();//SELECT username 姓名,sex 性别 FROM `th_user`
			//$arr = $m->field("username 姓名,sex 性别")->select();//SELECT username 姓名,sex 性别 FROM `th_user`
			//数组
			//$arr = $m->field(array('username','sex'))->select();//SELECT `username`,`sex` FROM `th_user`
			//$arr = $m->field(array('username'=>'姓名','sex'=>'性别'))->select();//SELECT `username` AS `姓名`,`sex` AS `性别` FROM `th_user` 
			//取id外的所有字段
			$arr = $m->field('id',true)->select();//SELECT `username`,`sex` FROM `th_user`
			
			var_dump($arr);
			$this->show('<hr>test show()');//如果不想创建模板文件调用display(),也可以用show()方法输出HTML代码.
		}
```
### 5.table - 设置表名
```php
//详见手册:模型-连惯操作
```

### 6.group - 分组
```php
//详见手册:模型-连惯操作
```
### 7.having - 分组后过滤
```php
//详见手册:模型-连惯操作
```

## 补充
alias 用于给当前数据表定义别名 字符串 
page 用于查询分页（内部会转换成limit） 字符串和数字 
join* 用于对查询的join支持 字符串和数组 
union* 用于对查询的union支持 字符串、数组和对象 
distinct 用于查询的distinct支持 布尔值 
lock 用于数据库的锁机制 布尔值 
cache 用于查询缓存 支持多个参数（以后在缓存部分再详细描述） 
relation 用于关联查询（需要关联模型扩展支持） 字符串 
validate 用于数据自动验证 数组 
auto 用于数据自动完成 数组 
filter 用于数据过滤 字符串 
scope* 用于命名范围 字符串、数组 

补充部分会在以后在详细探讨