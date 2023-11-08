---
layout: post
title:  Nginx负载均衡策略
date:   2023-05-29 14:20 UTC+8
categories: nginx
tags:
    - nginx
    - load balance
permalink: /nginx-load-balance-strategy
---

[官方文档](https://nginx.org/en/docs/http/load_balancing.html)

## 轮询策略

```
upstream myapp1 {
    server srv1.example.com;
    server srv2.example.com;
    server srv3.example.com;
}
```

Nginx将请求平均分发至每个正常的下游服务器，同时该策略具有基本的容灾功能，当下游服务器C发生故障，Nginx中的`Health checks`机制将会把C标记为failed，并在`fail_timeout`时间内避免将请求转发至下游服务器C，经过`fail_timeout`后，Nginx将通过客户端请求探测下游服务器C是否正常，如果正常，则C将被重新标记为live。

> Nginx是一个多进程的应用，由master管理worker，当worker数量不为1时，[进程间需要通过shm来确保当前的轮询状态是互通的](https://stackoverflow.com/questions/55257595/nginx-round-robin-load-balancing-is-not-as-expected)
>
> 


## 带权策略

```
upstream myapp1 {
    server srv1.example.com weight=3;
    server srv2.example.com;
    server srv3.example.com;
}
```

实际上轮询策略是一种特殊的带权策略，轮询策略中，每个下游服务器的权值都为1，在一般的带权策略中，Nginx将会将请求按权值大小进行分配。例如，在当前情况下，每5个请求就有3个分发至srv1，1个分发至srv2，1个分发至srv3。

## 最少连接策略

```
upstream myapp1 {
    least_conn;
    server srv1.example.com;
    server srv2.example.com;
    server srv3.example.com;
}
```

使用最少连接的负载平衡，nginx 将尽量不让繁忙的应用程序服务器因过多的请求而过载，而是将新请求分配给不太繁忙的服务器。

## IP_HASH策略

```
upstream myapp1 {
    ip_hash;
    server srv1.example.com;
    server srv2.example.com;
    server srv3.example.com;
}
```

通过对客户端的IP进行哈希，并通过[哈希值mod的方式](https://github.com/nginx/nginx/blob/a64190933e06758d50eea926e6a55974645096fd/src/http/modules/ngx_http_upstream_ip_hash_module.c#L184-L192)来决定将请求转发至具体的哪台下游服务器。

## 哈希策略

```
upstream myapp1 {
    hash $cookie_jsessionid;
    server srv1.example.com;
    server srv2.example.com;
    server srv3.example.com;
}
```

通过对客户端的`Cookie`中的`jsessionid`进行哈希，并通过[哈希值mod的方式](https://github.com/nginx/nginx/blob/a64190933e06758d50eea926e6a55974645096fd/src/http/modules/ngx_http_upstream_hash_module.c#L214-L222)来决定将请求转发至具体的哪台下游服务器。

> $cookie_{key}为访问http请求头中cookie的某个键的方式
>
> $http_{key}为访问http请求头headers中某个键的方式

## fair策略（第三方）

```
upstream myapp1 {
    fair;
    server srv1.example.com;
    server srv2.example.com;
    server srv3.example.com;
}
```

根据下游服务器的响应时间来决定请求转发，源码在[这里](https://github.com/gnosek/nginx-upstream-fair/tree/master)

## 一致性哈希策略（第三方）

```
upstream myapp1 {
    consistent_hash $cookie_jsessionid;
    server srv1.example.com;
    server srv2.example.com;
    server srv3.example.com;
}
```

通过对客户端的`Cookie`中的`jsessionid`进行哈希，并通过[哈希环](https://en.wikipedia.org/wiki/Consistent_hashing)的方式来决定将请求转发至具体的哪台下游服务器。

推荐阅读:

- [什么是一致性哈希？](https://xiaolincoding.com/os/8_network_system/hash.html)
- [nginx 负载均衡之一致性hash，普通hash](https://www.jianshu.com/p/89838a43ebf2)
