# [模板中的语法知识]
[TOC]
## 1.导入CSS与JS文件
传统用link标签导入CSS,用script标捡的src导入JS文件.
```html
		<link rel="stylesheet" type="text/css" href="__PUBLIC__/Css/index.css"/>
		<script src="__PUBLIC__/Js/index.js" type="text/javascript" charset="utf-8"></script>
```
在ThinkPHP中有自己的方式导入CSS文件
import:它默认找的是Public目录.
```html
<!--type默认是CSS,在导入css文件时可以省略不写-->
<import type='js' file='Js.Test'/><!--项目入口/Public/js/test.js-->
<import type='css' file='Css.Test'/><!--项目入口/Public/css/test.css-->
```
如果不是在public目录下,则用第三个参数 basepath='./Other/'来指定相应目录.
```html
<import type='css' file='Css.Test' basepath='./Other/' /><!--项目入口/Public/css/test.css-->
```
用load标签导入:会自动检测文件类型.
```html
<load href='__PUBLIC__/Css/Test.css' />
<load href='__PUBLIC__/Js/Test.js' />
```
## 2.分支结构
```php
    $this->assign('sex','男');
    $this->assign('age','18');
    $this->display();
```
### if...else,if...elseif...else
```html
	<if condition='$sex eq "男"'>
		男人
		<else /> 不是男人
	</if>
	<hr />
	<!--if...else....if-->
	<if condition='$age gt 18'>
		  成年人
		<elseif condition='$age eq 18 ' /> 
		  刚好18岁
		<else />
		  未成年人
	</if>
```
条件判断符号
```table
gt | >
lt | <
eq | ==
elt | <=
egt | >=
neq | !=
heq | ===
nheq |!== 
```
### switch case标签
```html
			<switch name='num'><!--注意不要$符号-->
				<case value='1'>一个和尚挑水吃</case>
				<case value='2'>两个和尚抬水吃</case>
				<case value='3'>三个和尚没水吃</case>
				<default /> 这里是default
			</switch>
```



## 3.循环结构
### for标签
```html
			<for start = '0' end = '10' name = 'j' step = '2' comparison = 'lt'>
				<{$j}><br />
			</for>
```
参见手册:模板引擎->for标签
start（必须）：循环变量开始值
end（必须）：循环变量结束值
name（可选）：循环变量名，默认值为i
step（可选）：步进值，默认值为1
comparison（可选）：判断条件，默认为lt
### volist标签
遍历一维数组
```php
    $this->assign('list',array('a1','b2','c3','d4','e5','f6','g7'));
    //$this->assign('list',array('k1'=>'a1','k2'=>'b2','k3'=>'c3','k4'=>'d4','k5'=>'e5','k6'=>'f6','k7'=>'g7'));//对关联数组同样有效
```
```html
			<volist name='list' id='vo'>
				<{$vo}><br />
			</volist>
			<!--可以用offset结合length属性输出数组的某一部分(如下输出d3,d4,e5)-->
			<hr />
			<volist name='list' id='vo' offset='2' length='3'>
				<{$vo}><br />
			</volist>
```
遍历多维数组
```php
	$arr[0] = array('id'=>1,'username'=>'huodeming1','sex'=>'男');
	$arr[1] = array('id'=>2,'username'=>'huodeming2','sex'=>'男');
	$arr[2] = array('id'=>3,'username'=>'huodeming3','sex'=>'男');
	$arr[3] = array('id'=>4,'username'=>'huodeming4','sex'=>'男');
	$this->assign('data',$arr);
```
```html
			<volist name='data' id='vo'>
				<{$vo.id}>----<{$vo.username}>----<{$vo.sex}><br />
			</volist>
			<!--用二级循环也是可以输出的-了解内容-->
			<volist name='data' id='vo'>
				<volist name='vo' id="v" >
					<{$v}>--
				</volist>
				<br />
			</volist>
```
### foreach标签
```html
			<foreach name='list' item='vo' key='k'>
				<{$k}>----<{$vo}><br />
			</foreach>
```
> 实例:用表格输出一个数据库查询结果.前面做过.再做一次或做类似功能.

## 4.特殊标签
### 比较标签 eq,neq,gt,egt,lt elt,heq,nheq
```table
eq或者 equal | 等于 
neq 或者notequal |  不等于 
gt  | 大于 
egt  | 大于等于 
lt |  小于 
elt |  小于等于 
heq  | 恒等于 
nheq  | 不恒等于 
```
用法:
<比较标签 name="变量" value="值">内容</比较标签>
可以与`<else/`>标签结合使用.
eq
```html
<eq name='n' value='1'>传过来的n是1<else />传过来的n不是1</eq>
```
### 范围判断标签 in,between
用法：可与`<else/>`标签结合使用.
可以使用in标签来判断模板变量是否在某个范围内，例如：
```html
<in name="id"value="1,2,3">输出内容1</in>
```
如果判断不再某个范围内，可以使用：
```html
<notin name="id"value="1,2,3">输出内容2</notin>
```
可以把上面两个标签合并成为：
```html
<in name="id"value="1,2,3">输出内容1<else/>输出内容2</in>
```
可以使用between标签来判断变量是否在某个区间范围内，可以使用：
```html
<between name="id"value="1,10">输出内容1</between>
```
可以使用notbetween标签来判断变量不在某个范围内：
```html
<notbetween name="id"value="1,10">输出内容1</notbetween>
```
当使用between标签的时候，value只需要一个区间范围，也就是只支持两个值，后面的值无效，例如
```html
<between name="id"value="1,3,10">输出内容1</between>
```
实际判断的范围区间是1~3，而不是1~10，也可以支持字符串判断，例如：
```html
<between name="id"value="A,Z">输出内容1</between>
```
所有的范围判断标签的value属性都可以使用变量，例如：
```html
<in name="id"value="$var">输出内容1</in>
```
变量的值可以是字符串或者数组，都可以完成范围判断。
也可以直接使用range标签，替换in和notin的用法：
```html
<range name="id"value="1,2,3"type="in">输出内容1</range>
```
其中type属性的值可以用in或者notin。
### present标签,判断标识是否已赋值
```html
<present name='name'>name已赋值</present>
```
### empty标签,判断标识是否是空值
用法：可以`<else/>`标签结合使用
```html
<empty name="name">name为空值</empty>
```
如果判断没有赋值，可以使用：
```html
<notempty name="name">name不为空</notempty>
```
可以把上面两个标签合并成为：
```html
<empty name="name">name为空<else /> name不为空</empty>
```
### defined标签,判断常量是否已定义 - 了解
```html
<defined name="NAME">NAME常量已经定义<else /> NAME常量未定义</defined>
```
### define标签,在模板中定义一个常量 - 了解
```html
<define name="MY_DEFINE_NAME"value="3"/>
```
### assgin标签,在模板中申明标识 - 了解
```
<assign name="var" value="123" />
```

## 5.其他标签使用 - 不建议使用
```html
<php>echo "我是huodeming"</php>
```
我们建议需要使用PHP代码的时候尽量采用php标签，因为原生的PHP语法可能会被配置禁用而导致解析错误。
如果设置了TMPL_DENY_PHP参数为true，就不能在模板中使用原生的PHP代码，但是仍然支持PHP标签输出。
