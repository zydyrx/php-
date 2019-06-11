# [视图]
[TOC]
## 1.模板的使用 - 重点
1.模板访问规则
 模板文件夹[TPL]下/[分组文件夹] [模板主题文件夹]/和模块名同名的文件[如Index模块]夹/和方法名同名的文件[index.html]或.tpl文件
如要用别的后缀名,修改配置文件,如下,修改后以后模板文件都要以.tpl作为后缀
```php
    'TMPL_TEMPLATE_SUFFIX'=>'.tpl', //修改模板文件的后缀名,不写默认是html
```
2.修改模板文件目录层次 - 一般不改
修改后直接访问[如Index模块下index方法调用display()时,调用的是TPL目录下的Index_index.html]
```php
'TMPL_FILE_DEPR'=>'_',	//更改模板文件的目录层次,都放在根目录下,用[模块名_方法名.html]访问.
```
3.模板主题
默认thinkphp是没有主题的,但我们可以自己创建,如下改配置文件.
```php
'DEFAULT_THEME'=>'my',	//以后调用模板时会去Tpl目示下找my文件夹下找相应模板:[./Home/Tpl/my/User/index.html]
```
动态更改模板主题:
1.用权限改config.php文件.一般三个配置项一起用[在Tpl目录下不直接放模块,而是放各种主题文件夹]
2.在配置文件中自动巾侦测模板主题
```php
	//自动侦测模板主题//做了以下这两项后,可以在URL中用?t=模板主题名
	//如我们访问主页:index.php/User/index?t=my 常写成 index.php/User/index/t/my就可以了
	'DEFAULT_THEME'=>'my',	//以后调用模板时会去Tpl目示下找my文件夹下找相应模板:[./Home/Tpl/my/User
	'TMPL_DETECT_THEME'=>'true',//打开自动侦测模板主题
	'THEME_LIST'=>'your,my',//支持的模板主题列表,要在Tpl目示下创建your,my两个目录,里面再放模块文件夹
```
## 2.输出模板内容 - 重点
1.display方法
 1.1display()方法中没有参数
```php
    $this->display();//直接去Tpl目录下找模块名目录下找方法名.html
```
1.2display()方法带参数
```php
    //1.同目录下:如果我们在index方法中想调用index2.html模板,则此时则需要传参
     $this->display('index2');//直接去Tpl目录下找模块名目录下找index2.html    
     //2.不同目录下(与模块关系不大,但模块也是一个目录,所以可以用.但注意是找目录)//如比错误页,各模块公共访问
     $this->display('Public:index2');//就是去模块名下找Public目录下的index2.html//中间是冒号,可以是多级目录  
     //3.如果是其他主题下的 文件夹下的 模板文件,如my主题则需开启主题
    $this->display('my:index:index');//主题目录:模块目录:文件名[无后缀]     
    //4.在display()方法中用路径调用模板文件  比如整个项根目录下的public目录下的errar.html
    $this->display('./Public/error.html');//当前目录表示整个项目的入口文件index.html文件所在的目录.
    //5.display()方法的第二个参数,指定页面字符编码
    $this->display('./Public/error.html','utf-8');//可改变模板文件的编码格式:utf-8   gbk等
    //6.display()方法的第三个参数,指定文件格式  如text/xml 等.
    $this->display('./Public/error.html','utf-8','text/xml');//以XML格式显示,但注意要关闭调试功能的页面trace才能正常显示XML格式.
```
2.show()方法与fetch()方法
display方法必须创建一个模板文件,有时我们不想创建,则用show方法输出HTML内容也一样.
有了show方法,我们可以把HTML代码存在数据库中都OK.也可以读取模板文件为字符串再加载
```php
    $content = $this->fetch("public:error");//读取public目录下的error.html文件为一个字符串
    //好处,可以处理字符串
    $content = str_replace('h1','i',$content);//把页面中所有的h1替换成i
    $this->show($content);//显示出来
```

## 3.模板中的赋值 - 重点
```php
$nane = 'huodeming';
$this->assign('data',$name);//用assign方法传,参数1:模板页标识,参数2:要传递的值或变量
$this->display;
```
//但有第二种方法也可以给模板赋值设成成员属性,$this->属性 = 值.
```php
$this->data = '霍德明';
$this->display();
```

## 4.模板替换 - 重点
//比如一个模板文件,想调用外面的CSS文件.
//所有常量:参见手册:附录->常量参考
//模板替换常量:参见手册:视图->模板替换
下面常量随环境变化而变化.
../Public ： 会被替换成当前项目的公共模板目录 通常是 / 项目目录 /Tpl/default/Public/
__PUBLIC__ ：会被替换成当前网站的公共目录 通常是 /Public/
__TMPL__ ： 会替换成项目的模板目录 通常是 / 项目目录 /Tpl/default/
__ROOT__ ： 会替换成当前网站的地址（不含域名）
__APP__ ： 会替换成当前项目的 URL 地址 （不含域名）
__URL__ ： 会替换成当前模块的 URL 地址（不含域名）
__ACTION__ ：会替换成当前操作的 URL 地址 （不含域名）
__SELF__ ： 会替换成当前的页面 URL

//如果在模板文件中上面上内容,则是显示如下

/thinkPHP_V3.1.2/Home/Tpl/Public ： 会被替换成当前项目的公共模板目录 通常是 / 项目目录 /Tpl/default/Public/ 
/thinkPHP_V3.1.2/Public ：会被替换成当前网站的公共目录 通常是 /Public/
/thinkPHP_V3.1.2/Home/Tpl/ ： 会替换成项目的模板目录 通常是 / 项目目录 /Tpl/default/
/thinkPHP_V3.1.2 ： 会替换成当前网站的地址（不含域名）
/thinkPHP_V3.1.2/index.php ： 会替换成当前项目的 URL 地址 （不含域名）
/thinkPHP_V3.1.2/index.php/User ： 会替换成当前模块的 URL 地址（不含域名）
/thinkPHP_V3.1.2/index.php/User/index ：会替换成当前操作的 URL 地址 （不含域名）
/thinkPHP_V3.1.2/index.php/User/index ： 会替换成当前的页面 URL

我们经常在模板中用常量调用CSS或JS文件.我们可以自定义常量
```php
    'TMPL_PARSE_STRING'=>array(
    '__CSS__'=>__ROOT__.'/public/css',
    '__JS__'=>__ROOT__.'/public/js',
    ),//增加自己的模板变量规则,注意路径..小心使用
    
```
在配置文件中配置好之后,我们可以在模板文件中调用
```html
__CSS__:自定义的CSS目录<br />
__JS__:自定义的JS目录<br />
```
会输出如下
/thinkPHP_V3.1.2/public/css:自定义的CSS目录
/thinkPHP_V3.1.2/public/js:自定义的JS目录


