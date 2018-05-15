---
title: angular-testing
date: 2018-05-15 12:03:55
categories:
- 测试
- angular
- 前端
- js
tags:
- testing
- angular testing
---

这是一篇流水账式记录angular测试时遇到的问题和想法。

在这个项目内的`api.service`，我注入了`HttpClient`、`NzNotificationService`、`router`三个服务。
在测试`api.service`时，你测试的是`router`服务的`navigate`方法有没有被调用，而且输入什么参数。这时，你要使用`jasmine`的`spy`监视对`router`下的`navigate`方法。而不是，检查浏览器地址栏是否有发生变化。
难道我们就不要检查浏览器地址栏的变化吗？其实，还是要的。不过，这应该是交给`router`服务本身的测试来做。这才是模块化的思想，各模块能独立加载，有自身的单元测试。  

然后，是对登录页组件的测试。对于`login.spec.ts`这个测试文件的内容，主要还是记录下`jasmine.createSpyObj`这个方法。我用这个方法创建了一个`AuthService`的`spy`。这个`spy`是监视点击登录按钮时，触发的组件内的`submit`函数。`submit`执行时会调用`AuthService`的`login`方法。此时，我们不需要用`httpMock`方式来模拟网络请求数据了，可以直接调用`AuthService.login`(这个服务和方法现在是`jasmine`创建的`spy`)下的`returnValue`来立刻返回结果，这样我们就可以直接测试`authService.login.subscribe`回调函数内的代码。  
那为什么可以不用`httpMock`模拟网络请求呢？这就是上面提到的各自独立加载和有测试单元了。我们已经对`AuthService`这个服务写过单元测试了，保证了他的功能正常。然后，到我们的对组件测试时，我们就没必要再重复这个模拟过程了。


