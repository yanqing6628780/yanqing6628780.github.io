---
title: karma配置参数笔记
date: 2017-02-16 16:04:10
categories:
- 测试
tags:
- karma
- 测试框架
description: 本文是关于karam配置参数笔记.涉及如下参数:basePath files proxies
---

```
module.exports = function(config) {
    var globSync = require("glob").sync;
    var files = globSync('newifi/@(jquery*|newifi).js', { cwd: 'web/htdocs' });
    var otherJsFiles = globSync('newifi/**/!(jquery*|newifi|angular*|bootstrap|echarts*).{js,json}', { cwd: 'web/htdocs' });
    var cssFiles = globSync('newifi/**/*.css', { cwd: 'web/htdocs' });
    files.push('index.html');
    files.push('../../test/*.coffee');
    files.push('../../test/*.js');
    otherJsFiles.forEach(function(file){
        files.push({
            pattern: file,
            served: true,
            included: false
        });
    });
    cssFiles.forEach(function(file){
        files.push({
            pattern: file,
            served: true,
            included: false
        });
    });
    config.set({
        // base path that will be used to resolve all patterns (eg. files, exclude)
        basePath: 'web/htdocs/',


        // frameworks to use
        // available frameworks: https://npmjs.org/browse/keyword/karma-adapter
        frameworks: ['jasmine-jquery', 'jasmine', 'jasmine-matchers'],


        // list of files / patterns to load in the browser
        files: files,

        // list of files to exclude
        exclude: [],


        // preprocess matching files before serving them to the browser
        // available preprocessors: https://npmjs.org/browse/keyword/karma-preprocessor
        preprocessors: {
            '**/*.html': ['html2js'],
            '../../**/*.coffee': ['coffee', 'coverage']
        },
        proxies: {
            '/newifi': '/base/newifi'
        },

        // test results reporter to use
        // possible values: 'dots', 'progress'
        // available reporters: https://npmjs.org/browse/keyword/karma-reporter
        reporters: ['progress'],


        // web server port
        port: 9876,


        // enable / disable colors in the output (reporters and logs)
        colors: true,


        // level of logging
        // possible values: config.LOG_DISABLE || config.LOG_ERROR || config.LOG_WARN || config.LOG_INFO || config.LOG_DEBUG
        logLevel: config.LOG_INFO,


        // enable / disable watching file and executing tests whenever any file changes
        autoWatch: true,


        // start these browsers
        // available browser launchers: https://npmjs.org/browse/keyword/karma-launcher
        browsers: ['PhantomJS'],


        // Continuous Integration mode
        // if true, Karma captures browsers, runs the tests and exits
        singleRun: false,

        // Concurrency level
        // how many browser should be started simultaneous
        concurrency: Infinity
    });
};
```
上面是我的配置文件  
<!--more-->
首先，说明下basePath参数。这个参数设置了`web/htdocs/`，这个作用是在`files`和`exclude`两个参数里面不用再写这部分路径的匹配了。  
但是，我这个项目的`test`目录在`web/htdocs/`外,所以还要补上`../../`。否则会匹配不上。

然后，我们再看看`files`参数下面这三个参数`pattern`、`served`和`included`这三个参数.  
`pattern`是要匹配的文件和匹配模式  
`served`用于该文件是否由karma webserver提供  
`included`浏览器是否需要通过`<script>`标签引入该文件  

为什么我要将部分文件设置成`included:false`手动加载呢？  
这是因为部分js文件加载是由`newifi.js`，通过ajax加载进去的。如果，直接`included`进karma webserver就会报错（找不到函数或者变量没定义）。  
但是，这样还是不能正常加载这些js文件的。在执行`newifi.js`时，ajax加载这部分js文件会提示404.
这是因为karma将加载后的文件移到`http:\\localhost:[port]\base`下了。所以，你还要设置`proxies`参数.将ajax加载`/newifi`下的js文件指向`/base/newifi`
