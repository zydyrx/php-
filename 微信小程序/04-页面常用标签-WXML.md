# 04-页面结构-WXML
[TOC]

## 微信小程序组件(标签)
组件文档：https://mp.weixin.qq.com/debug/wxadoc/dev/component/

## 常用布局标签

```xml
<view></view>				相当于    <div></div>
<text></text>				相当于    <span></span>
<image></image>				相当于    <img />
<navigator></navigator>		相当于    <a></a>
<block></block>				区块标签，不会渲染到页面
```

**注意：image组件默认宽度300px、高度225px，很多时候我们都不需要这个默认宽高，记得手动设置宽高**

## 常用表单标签

```xml
<button></button>
<input type="text" />				
<checkbox />
<radio/>
```

