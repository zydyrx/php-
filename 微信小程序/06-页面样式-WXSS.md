# 06-页面样式-WXSS
[TOC]

## WXSS 样式

WXSS(WeiXin Style Sheets)是一套样式语言。

与 CSS 相比，WXSS 扩展以下2个特性：

- 尺寸单位      rpx ( responsive pixel 响应式像素), 规定屏幕宽为750rpx,所以我们开发时都用IPHONE6作为调试参考,方便计算.
- 样式导入      在wxss文件中直接  @import  "样式表相对路径";

第三方样式库
WeUI 一套同微信原生视觉体验一致的基础样式库
iView Weapp 一套高质量微信小程序UI组件库
Vant Weapp 轻量,可靠的小程序UI组件库