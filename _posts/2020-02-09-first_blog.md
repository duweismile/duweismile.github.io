---
layout: post
title: python2.7 使用rabbitMQ 时的一些坑
categories: [python, rabbitMQ]
description: python 的 rabbitMQ 客户端使用时的一些小坑
keywords: python, 消息队列
---

### 背景
在使用 `pip` 安装 `pika` 包时，默认安装的是较高的版本，高版本的 `pika` 需要 `SSLContext` 这个依赖，但是这个包是在 `python2.7.9` 之后才添加的，所以此时用高版本的 `pika` 在 `python2.7.9` 以下版本环境连接 `rabbitMQ` 时会报出如下错误

```
'module' object has no attribute 'SSLContext' python2.7
```

因此在 python2.7.4 环境中使用 `pika` 客户端连接 `rabbitMQ` 时，需要安装低版本的 `pika` 包才可运行，或者升级到更高的 python 版本
### 解决办法
更新 pika 版本，将 pika 升级到 0.11.0, 在终端执行以下命令，更换 `pika` 版本
```
pip install pika==0.11.0
```

此时还会有一些问题，如果按照网上大部分的教程的话，会报以下错误
```
exchange_declare() got an unexpected keyword argument 'type'
```
这是由于 `pika` 低版本的 API 和高版本的稍微有点差别，需要改一下调用中的参数名
```
# 高版本的 API
exchange_declare(callback=None,
                 exchange=None,
                 type='direct',
                 passive=False,
                 durable=False,
                 auto_delete=False,
                 internal=False,
                 nowait=False,
                 arguments=None)
```
```
# 低版本版本的API
exchange_declare(callback=None,
                 exchange=None,
                 exchange_type='direct',
                 passive=False,
                 durable=False,
                 auto_delete=False,
                 internal=False,
                 nowait=False,
                 arguments=None)
```
在调用该接口时，需要将交换机的类型 `type` 改为低版本的 `exchange_type` ,若其他接口业报类似错误，一般是关键词参数名与老版本不一致，需要查看原接口的定义关键词，修改一致即可

我的报错的代码如下
```
self.channel.exchange_declare(exchange=self._exchange,
                              durable=True,
                              type='topic'
                             )
```
修改后的代码
```
self.channel.exchange_declare(exchange=self._exchange,
                              durable=True,
                              exchange_type='topic'
                              )
```
