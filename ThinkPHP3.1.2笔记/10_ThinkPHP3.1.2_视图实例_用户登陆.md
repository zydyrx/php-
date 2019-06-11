#[视图实例_用户登陆]
[TOC]
## 1.创建登陆登陆模块
```php
    //ThinkPHP3.1.2/Home/Lib/Action/LoginAction.class.php
	class LoginAction extends Action{
		public function index(){
			$this->display();
		}
	}

```
创建登陆模板:ThinkPHP3.1.2/Home/Tpl/Login/index.html
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>登陆页</title>
		<style type="text/css">
			/*写样式要注意,别把调式功能搞乱了.*/
			#login { border: 1px solid #777777; border-radius: 5px; width: 300px; height: 300px; margin: 0 auto; padding: 20px; text-align: center;}
			#login input{
				margin-top: 10px;
				height: 1.5em;
			}
		</style>
	</head>
	<body>
		<div id="login">
			<form action="" method="post">
			用户名:<input type="text" name="username" value="" /><br />
			密&nbsp;码:<input type="password" name="password" value="" /><br />
			验证码:<input type="text" name="code" value="" /><br />
			<input type="submit" value="登陆"/>
		</form>
		</div>
	</body>
</html>
```

## 2.导入ThinkPHP扩展(里面有验证码类)
1.下载thinkphp的3.1.2扩展包,进行解压后复制到ThinkPHP3.1.2/ThinkPHP/Extend/目录下

## 3.创建共公模块与验证码
参照手册中的:杂项->验证码进行操作.
1.创建公共模块,在里面按要求创建方法
```php
//ThinkPHP3.1.2/Home/Lib/Action/PublicAction.class.php
class PublicAction extends Action{
	public function code(){
		import('ORG.Util.Image');
		Image::buildImageVerify();
	}
}
```

## 4.进行验证,全部代码
```php
//ThinkPHP3.1.2/Home/Lib/Action/LoginAction.class.php
class LoginAction extends Action{
		public function index(){
			$this->display();
		}
		public function do_login(){
			//获取用户与密码,及验证码是否正确.
			//dump($_POST);
			//dump($_SESSION);
			$code = $_POST['code'];
			if(md5($code) != $_SESSION['verify']){
				$this->error("验证码错误!");
				return;
			}/*else{
				$this->success("验证码正确!");
			}*/
			//再验证用户与密码
			$m = M('User');
			$where['username'] = $_POST['username'];
			//由于表未创建密码字段,先略过
			$count = $m->where($where)->count();
			if($count == 1){
				$_SESSION['username'] = $_POST['username'];
				$this->redirect('/');//直接按URL跳转方法.
			}else{
				$this->error('登陆失败,跳转到登陆页!');
			}
		}
	}
```
ThinkPHP3.1.2/Home/Tpl/Login/index.html
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>登陆页</title>
		<style type="text/css">
			/*写样式要注意,别把调式功能搞乱了.*/
			#login { border: 1px solid #777777; border-radius: 5px; width: 300px; height: 300px; margin: 0 auto; padding: 20px; text-align: center;}
			#login input{
				margin-top: 10px;
				height: 1.5em;
				vertical-align:bottom;
			}
		</style>
		<script type="text/javascript">
			function sub(){
				var ou = document.myForm.username;
				var op = document.myForm.password;
				var oc = document.myForm.code;
				if(ou.value == '' || op.value=='' || oc.value==''){
					alert('用户名或密码或验证码不能为空!');
				}else{
					document.myForm.submit();
				}
			}
		</script>
	</head>
	<body>
		<div id="login">
			<form action="__URL__/do_Login" method="post" name = 'myForm'>
			用户名:<input type="text" name="username" value="" /><br />
			密&nbsp;码:<input type="password" name="password" value="" /><br />
			<!--为了点击验证码刷新,我们要需给它写个click事件响应-->
			验证码:<input type="text" name="code" value="" size="5" />
			<img src="__APP__/Public/code" onclick='this.src = this.src+"?"+Math.random()' />
			<br />
			<!--<input type="submit" value="登陆"/>-->
			<input type="button" value="登陆" onclick="sub()" />
		</form>
		</div>
	</body>
</html>
```


