# [07_ThinkPHP3.1.2_查询方式实例]
[TOC]
## 编写查询页面.对数据进行条件检索 - 练习
由于与前面的User模块下index模板可重复使用.则不再创建新模板页面,用$this->display('index')加载本模块下的index模板.
thinkPHP_V3.1.2/Home/Lib/Action/UserAction.class.php
```php
	class UserAction extends Action{
		public function index(){
			$m = M('User');
			$arr = $m->select();
			$this->assign('data',$arr);
			$this->display();
		}
		public function search(){
			//var_dump($_POST);
			$m = M('User');
			if( isset($_POST['username']) &&   $_POST['username'] != null)
				$where['username'] = array('like',"%{$_POST['username']}%");
			if($_POST['sex'] != null)
				$where['sex'] = array('eq',$_POST['sex']);
			$arr = $m->where($where)->select();
			$this->assign('data',$arr);
			$this->display('index');
		}
	}
```
thinkPHP_V3.1.2/Home/Tpl/User/index.html
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>用户操作主页</title>
	</head>
	<body>
		<form action="__URL__/search" method="post">
			姓名:<input type="text" name="username" value="" />
			性别:<input type="radio" name="sex" value="0" />女 <input type="radio" name="sex" value="1" />男
			<input type="submit" value="搜 索"/>
		</form>
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
						<td><{$vo.username}></td>
						<td><{$vo.sex}></td>
						<td>修改 删除</td>
					</tr>
				</volist>
		</table>
	</body>
</html>

```