---
title: docker 挂载目录造成程序无法找到文件
date: 2018-01-11 10:24:03
categories:
    - 运维
tags:
- docker
- docker-compose
---

```
db:
    image: mongo
    volumes:
        - ~/www/db:/data
    container_name: db
    restart: always

www:
    build: .
    ports:
        - "12333:8000"
    volumes:
        - ~/www/logs:/www/logs
        - ~/www/public:/www/public
        - ~/www/config:/www/config
    links:
        - db
    container_name: www_authbox
    restart: always
```

以上是docker-compose配置。
有问题是这里 `- ~/www/config:/www/config`
config目录是用来放置程序配置文件的
如果将config目录挂载后，`npm start`启动程序时，程序查找config目录下文件会提示找不到文件的错误。但是，文件其实已经在构建时拷贝到config目录下了。
这是docker挂载的bug呢？还是写法有问题呢？暂时未找到原因

解决方案如下：
- 不要挂载config目录
- 根据同事建议，迟延`npm start`。如使用sleep。或者在`\bin\www`增加`timeout`代码
