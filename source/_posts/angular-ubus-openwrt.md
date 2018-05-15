---
title: 应用在openwrt固件上的angular项目
date: 2018-01-11 14:00:00
categories:
    - angular
tags:
- angular
- openwrt
- ubus
---

项目地址：https://github.com/yanqing6628780/angular2_ubus_openwrt

该项目是在公司的`pandorabox`固件的`ubus`接口下，使用`angular`进行开发尝试。
所以，要运行该项目：
- 你需要一个路由器
- 该路由器需要刷入`pandorabox`或者`openwrt`固件
- 固件内需要有以下`ubus`命令：
    - session
    - uci

如果你路由器不是`192.168.1.1`的ip，你还需要修改`proxy.conf.json`文件。

该项目是从angular的hero项目clone下来后直接修改的。只app内的文件，其他配置基本没有修改。
用到的知识点如下：
- 路由拆分
- http拦截器
- 路由守卫(鉴权)
- 延迟加载(这个没看出效果)
- 使用[ng-zorro-antd](https://ng.ant.design)
- 组件二次封装(难点1)

其实，按照hero项目的教程看下来，你基本能掌握大部分技能。但是，要真正学习到知识还是要用实际项目练手。就好像难点1那里，这里的封装需了解
- 如何对数据进行双向绑定
- 如何从组件输入后输出
现在`angular`不像`angular1`的`Directive`可以直接使用`scope`进行双向绑定
你还要使用各种装饰器`@Input` `@Output`还要使用`EventEmitter`进行事件触发。
详细请看`password_eye`目录下.ts文件

据说，这样做的目的是为了减少像`angular1`那样的全局`脏检查`，性能会更好。但是，开发就麻烦和复杂了。

### 总结
angular开发还是要配合`vscode`进行开发，不然`import`库的时候真的好麻烦
还有现在引入的组件概念。其实，你把这个组件当成`angular1`的控制器，就比较容易理解。
但是，`angular1`的`Directive`现在也是归到`@Component`的。你在`angular`里面使用`@Directive`是没法引入`template`和`templateUrl`这些参数的。所以，`angular`的`@Directive`又是另外一个概念了
而且，`angular`现在也解决了`angular1`时的少问题。例如：双view。而且，`route`比以前强大，有守卫、延迟加载、还有延迟预加载。
不过，`angular`有个比较麻烦的‘缺点’。例如：父组件的模板上有个`tilte`变量，这个变量是用在head标签内的title标签。如果，想要子组件内赋值，你只能使用`service`这种形式，然后这个变量要用Rxjx的`Observable`。子组件要注入这个`service`，父组件对其进行`订阅`。
暂时，使用`angular`的感受就是这样了。如果项目能继续深入应该会有发现更多‘问题’，解决更多问题，这样对`angular`有更多和更深入的了解。
