# ThinkPHP_CURD实例,操作数据
[TOC]
## 1.创建User模块
```php
    //在Home下创建User类:[\Home\Lib\Action\UserAction.php]
    <?php
	class UserAction extends Action{
		public function index(){
			echo 'hello userclass.index打开了';//不管乱码,我们一般不这么做,常用display()输出一个模板
		}
	}
?>
```
## 2.要求在打开此模块时显示所有用户信息
```table
ID |	姓名 |	性别|	操作
1 |	huodeming	 |	1 |	修改 删除
2 |	huodeming2 |	1 |	修改 删除
3 |	huodeming3 |	 0 |	修改 删除
5 |	huodeming5 |	 0 |	修改 删除
```
`ThinkPHP3.1.2/Home/Lib/Action/UserAction.class.php`文件内容
```php
	class UserAction extends Action{
		public function index(){
			//echo 'hello userclass.index打开了';//不管乱码,我们一般不这么做,常用display()输出一个模板
			//$this->display();//如果不存在模板则报错.模板不存在[./Home/Tpl/User/index.html],按要求去创建模板,创建好OK
			
			//我们的需求,需要这个方法加载,显示所有用户信息,并带修改,删除功能
			$m = new Model("User");
			$arr = $m->select();
			//var_dump($arr);
			$this->assign('data',$arr);//把读取的用户表数组传给模板
			$this->display();//显示模板
		}
	}
```
`ThinkPHP3.1.2/Home/Tpl/User/index.html`文件内容
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>用户操作主页</title>
	</head>
	<body>
		<table border="1px" cellspacing="1px" cellpadding="0px" width="500px">
			<tr>
				<th>ID</th>
				<th>姓名</th>
				<th>性别</th>
				<th>操作</th>
			</tr>
				<volist name='data' id='vo'>
					<tr>
						<td><{$vo.id}></td>
						<td><{$vo.name}></td>
						<td><{$vo.sex}></td>
						<td>修改 删除</td>
					</tr>
				</volist>
		</table>
	</body>
</html>
```
## 3.要求点击删除按扭,进行相应用户删除
```php
	class UserAction extends Action{
		public function index(){
			//echo 'hello userclass.index打开了';//不管乱码,我们一般不这么做,常用display()输出一个模板
			//$this->display();//如果不存在模板则报错.模板不存在[./Home/Tpl/User/index.html],按要求去创建模板,创建好OK
			
			//我们的需求,需要这个方法加载,显示所有用户信息,并带修改,删除功能
			$m = new Model("User");
			$arr = $m->select();
			//var_dump($arr);
			$this->assign('data',$arr);//把读取的用户表数组传给模板
			$this->display();//显示模板
		}
		public function del(){
			$m = M('User');
			$id = $_GET['id'];
			$count = $m->delete($id);
			if($count > 0){
				$this->success('删除成功!');//后面讲解此方法
			}else{
				$this->error('删除失败!');//后面讲解此方法
			}
			
		}
	}
```
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>用户操作主页</title>
	</head>
	<body>
		<table border="1px" cellspacing="1px" cellpadding="0px" width="500px">
			<tr>
				<th>ID</th>
				<th>姓名</th>
				<th>性别</th>
				<th>操作</th>
			</tr>
				<volist name='data' id='vo'>
					<tr>
						<td><{$vo.id}></td>
						<td><{$vo.name}></td>
						<td><{$vo.sex}></td>
						<td>修改 <a href='../User/del/id/<{$vo.id}>'>删除</a></td>
					</tr>
				</volist>
		</table>
	</body>
</html>
```
## 4.点击修改按扭,要求显示修改页面.
```php
	class UserAction extends Action{
		public function index(){
			//echo 'hello userclass.index打开了';//不管乱码,我们一般不这么做,常用display()输出一个模板
			//$this->display();//如果不存在模板则报错.模板不存在[./Home/Tpl/User/index.html],按要求去创建模板,创建好OK
			
			//我们的需求,需要这个方法加载,显示所有用户信息,并带修改,删除功能
			$m = new Model("User");
			$arr = $m->select();
			//var_dump($arr);
			$this->assign('data',$arr);//把读取的用户表数组传给模板
			$this->display();//显示模板
		}
		/**
		 * 删除相应用户
		 */
		public function del(){
			$m = M('User');
			$id = $_GET['id'];
			$count = $m->delete($id);
			if($count > 0){
				$this->success('删除成功!');//后面讲解此方法
			}else{
				$this->error('删除失败!');//后面讲解此方法
			}
			
		}
		/**
		 * 修改相应用户
		 */
		public function update(){
			$m = M('User');
			$id = $_GET['id'];
			$data = $m->find($id);//用find方法读取单条数据
			$this->assign('data',$data);
			$this->display();
		}
	}
```
`ThinkPHP3.1.2/Home/Tpl/User/index.html`增加修改链接
```html
<a href="../User/update/id/<{$vo.id}>">修改</a>
```
`ThinkPHP3.1.2/Home/Tpl/User/update.html`创建此文件,作为修改页面.
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>修改用户</title>
		<script type="text/javascript">
			//用window.onload方法获取传过来的性别,以显示下表单中的性别.
			window.onload = function(){
			var sex = <{$data.sex}>;
			if(sex == 0){
				document.getElementsByName('sex')[0].checked = 'checked';
			}else{
				document.getElementsByName('sex')[1].checked = 'checked';
			}
			}
		</script>
	</head>
	<body>
		<form action="" method="post">
			I D: <{$data.id}> <br />
			<input type="hidden" name="id"value="<{$data.id}>" />
			姓名:<input type="text" name="name" value="<{$data.name}>" /><br />
			性别:<input type="radio" name="sex" value="0" />女 <input type="radio" name="sex" value="1" />男
			<input type="submit" value="确认修改"/>
		</form>
	</body>
</html>
```
## 5.要求在修改页面点击确认修改后进行数据库更改
```php
		public function updateOk(){
			//var_dump($_POST);
			$m = M('User');
			$count = $m->save($_POST);
			if($count > 0){
				$this->success('修改成功!','index');//加第二个参数,表示返回到主方法页
			}else{
				$this->error('修改失败!');//后面讲解此方法
			}
		}
```
```html
		<form action="../../updateOk" method="post">
			I D: <{$data.id}> <br />
			<input type="hidden" name="id"value="<{$data.id}>" />
			姓名:<input type="text" name="name" value="<{$data.name}>" /><br />
			性别:<input type="radio" name="sex" value="0" />女 <input type="radio" name="sex" value="1" />男
			<input type="submit" value="确认修改"/>
		</form>
```
## 6.增加用户实现
点击增加按扭-->显示增加页面-->确认增加发送表单到addOk()方法完成增加用户.
```php
		public function add(){
			//echo 'add...';
			$this->display();
		}
```
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>增加用户</title>
	</head>
	<body>
		<form action="./addOk" method="post">
			姓名:<input type="text" name="name" value="" /><br />
			性别:<input type="radio" name="sex" value="0" />女  <input type="radio" name="sex" value="1" />男<br />
			<input type="submit" value="确认增加"/>
		</form>
	</body>
</html>
```
```php
		public function addOk(){
			$m = M('User');
			//var_dump($_POST);
			$m->name = $_POST['name'];
			$m->sex = $_POST['sex'];
			$retId = $m->add();//调用add方法返回插入的ID值.不是行数
			if($retId > 0){
				$this->success('增加成功!','index');//后面讲解此方法
			}else{
				$this->error('增加失败!');//后面讲解此方法
			}
		}
```
在练习后对CURD应有个比较深刻的印象.






