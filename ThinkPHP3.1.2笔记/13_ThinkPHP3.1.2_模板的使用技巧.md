#[模板的使用技巧]
[TOC]
## 1.模板包含
前面用了导入css与果js文件,用的是
```html
<!--type默认是CSS,在导入css文件时可以省略不写-->
<import type='js' file='Js.Test'/><!--项目入口/Public/js/test.js-->
<import type='css' file='Css.Test'/><!--项目入口/Public/css/test.css-->
<!--或者loand标签-->
<load href='__PUBLIC__/Css/Test.css' />
<load href='__PUBLIC__/Js/Test.js' />
```
如果是模板文件的导入:
```html
<include file='Public:header.html' />
<!--当然还有其他参数,但不常用也不建议用-->
<include file='Public:header' css='Test'/>
<!--在头部文件中导入CSS时可以中括号[]中使用标识-->
<load href='__PUBLIC__/Js/[css].js' />
<!--
在编译后文件中显示
<load href='__PUBLIC__/Js/Test.js' />
-->
```
实例讲解:
ThinkPHP3.1.2/Home/Tpl/Index/index.html
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>网站首页</title>
		<load file = '__PUBLIC__/Css/test.css' />
	</head>
	<body>
		<p>网站首页,欢迎你</p>
		<a href="__URL__/next">点击进入下一页</a>
	</body>
</html>
```
ThinkPHP3.1.2/Home/Tpl/Index/next.html
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>下一页</title>
		<load file = '__PUBLIC__/Css/test.css' />
	</head>
	<body>
		<p>这是下一页</p>
		<a href="__URL__/index">点击跳转到首页</a>
	</body>
</html>
```
假设以上两个页面有部分是相同的,可以拿出来做为共公页面header.html就可以了.
ThinkPHP3.1.2/Home/Tpl/Public/header.html[模板目录/公共目录/头.html文件]
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>[pagename]</title><!--这地方可以用中括号,里面写标识,在include标签中可以替换出来-->
		<load file = '__PUBLIC__/Css/test.css' />
	</head>
```
上面头文件建好后,可以把首页与下一页中的公共部分去掉了.并用include标签导入头模板文件.
```html
<!--首页代码,注意pagename属性-->
	<include file='Public:header' pagename='标题:首页' />
	<body>
		<p>这是网站首页</p>
		<a href="__URL__/next">点击进入下一页</a>
	</body>
</html>
<!--下一页代码,同样注意pagename属性-->
	<include file='Public:header' pagename='标题:下一页' />
	<body>
		<p>这是下一页</p>
		<a href="__URL__/index">点击跳转到首页</a>
	</body>
</html>
```
注:在3.1以上版本,include标签支持导入多文件,用,隔开
```html
<include file='file1,file2' />
```

## 2.模板渲染
> 参见手册:模板引擎->模板布局

1.自动开启模板渲染
a.设置配置文件    'LAYOUT_ON'=>true,
b.创建:Tpl/layout.html 模板,在模板中使用{__NOLAYOUT__}表示当前页面内容，
c.在自动开启后如有的页面不需要用,则在页首加上{__NOCONTENT__}.

2.不开启自动模板渲染使用.
a.创建:Tpl/layout.html 模板,在模板中使用{__NOLAYOUT__}表示当前页面内容
b.在要使用的页面页首加上`<layout>`标签
```hmtl
<layout name = 'layout' />
		<p>这是下一页</p>
		<a href="__URL__/index">点击跳转到首页</a>
```
Tpl/layout.html [渲染模板文件内容]
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>下一页</title>
        <load file = '__PUBLIC__/Css/test.css' />
    </head>
    <body>
        <{__CONTENT__}>
    </body>
</html>
```
在渲染模板中也可以使用include标签来导入公共部分...如头部作为内容.
```html
	<include file='Public:header' pagename='标题:下一页' />
	<body>
		<{__CONTENT__}>
	</body>
</html>
```
## 3.模板的继承


