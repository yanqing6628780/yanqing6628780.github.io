---
title: 开发angular上的formControl组件
tags:
  - angular
  - formControl
  - component
categories:
  - angular
date: 2018-05-15 12:02:59
---


一个简单的密码显示隐藏组件，是对ant desigin of angular的nz-input组件的二次封装。

文件源代码：https://github.com/BoxSystem/StoreBox-ng2/blob/master/src/app/components/password-eye/index.ts

看文件的提交历史。其实，一开始是没考虑formControl的支持的。不使用formControl的情况，只要使用EventEmitter触发事件进行组件间交互就好。
当你需要在页面组件使用FormBuilder时，你就需要在页面元素上绑定formControlName属性。
但是，当你需要将html元素二次封装成组件时，你的组件必须`implements``ControlValueAccessor`这个接口。并且，实现接口下面的三个方法`writeValue` `registerOnChange` `registerOnTouched`。实现了这个三方法后，上面EventEmitter触发的交互事件已经不需要了。

