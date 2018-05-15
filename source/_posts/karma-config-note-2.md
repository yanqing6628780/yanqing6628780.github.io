---
title: karma配置参数笔记(2)
date: 2017-02-21 16:39:11
categories:
    - 测试
tags:
- karma
- 测试框架
description: 本文是关于karam配置参数笔记.涉及如下参数:files preprocessors reporters
---


```
module.exports = function(config) {
    var files = [];
    files.push('newifi/@(jquery*|newifi).js');
    files.push('index.html');
    files.push('../../test/*.coffee');
    files.push('../../test/*.js');
    var otherFiles = [
        'newifi/**/!(jquery*|newifi|angular*|bootstrap|echarts*).js',
        'newifi/**/*.css',
        'newifi/**/*.json'
    ];
    otherFiles.forEach((file) => {
        files.push({
            pattern: file,
            served: true,
            included: false
        });
    });
    var preprocessors = {
        '**/*.html': ['html2js'],
        '../../**/*.coffee': ['coffee']
    };
    preprocessors[otherFiles[0]] = ['coverage'];
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
        preprocessors: preprocessors,
        proxies: {
            '/newifi': '/base/newifi'
        },

        // test results reporter to use
        // possible values: 'dots', 'progress'
        // available reporters: https://npmjs.org/browse/keyword/karma-reporter
        reporters: ['progress', 'kjhtml', 'coverage'],

        coverageReporter: {
            type: 'html',
            dir: '../../coverage/'
        },

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

这是改进后的配置文件。  
上一篇里面用`glob`获取所有匹配文件，然后一个个加文件的做法其实是多余的。
经过试验，其实`karma`的文件匹配方式是支持`glob`那套的。
<!--more-->

然后，新配置内添加`kjhtml`插件和`coverage`插件。  
`kjhtml`插件的作用是将`karma`服务器的debug页变成测试结果页。而且，在debug页点击其中一个测试结果，该测试都会重新执行一遍。不过，可惜的是，修改测试文件后不会自动刷新。  
`coverage`插件是覆盖测试报告。  
上面配置文件里面的这句:  
```
preprocessors[otherFiles[0]] = ['coverage'];
```
表示那些被测试的文件需要代码覆盖测试报告。
