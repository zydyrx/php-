# WXS脚本
[TOC]


## WXS（WeiXin Script）
是小程序的一套脚本语言，功能类似`<script>`标签用于在视图层定义函数(比较少用)。

```xml
<!--wxml-->
<wxs module="foo">
var sum = function(a,b){
  return a+b;
};
// 这里可以导出一个对象，这个对象可以直接在界面上使用 
module.exports.sum = sum;
</wxs>

<view> {{foo.sum}} </view>
```
