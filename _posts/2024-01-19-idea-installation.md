---
layout: post
title:  IntelliJ IDEA 安装
date:   2024-01-19 21:02 UTC+8
categories: java
tags: ["java", "JetBrains"]
permalink: /java/idea-installation
---

[IDEA官方网址](https://www.jetbrains.com/idea/)

## 下载并安装IDEA

1. 在官方网址选择对应操作系统和芯片架构的IDEA安装包，下载

2. 点击安装包进行安装，自定义安装位置


## 自定义IDEA的配置和缓存路径（可选）

我们约定，IDEA的安装路径为/path/to/IntelliJ IDEA 20xx.x.x（例如：D:\IntelliJ IDEA 20xx.x.x或/home/name/IntelliJ IDEA 20xx.x.x）

以文本编辑软件（例如：VS Code）打开/path/to/IntelliJ IDEA 20xx.x.x/bin/idea.properties文件，修改`idea.config.path`，`idea.system.path`，`idea.plugins.path`，`idea.log.path`配置为自定义路径，并取消注释，四个配置项的含义分别为：

- `idea.config.path`: 配置路径
- `idea.system.path`: 缓存路径
- `idea.plugins.path`: 用户下载的插件存储路径
- `idea.log.path`: 日志路径

```
#---------------------------------------------------------------------
# Uncomment this option if you want to customize a path to the settings directory.
#---------------------------------------------------------------------
# idea.config.path=${user.home}/.IntelliJIdea/config
idea.config.path=/path/to/IntelliJ IDEA 20xx.x.x/config

#---------------------------------------------------------------------
# Uncomment this option if you want to customize a path to the caches directory.
#---------------------------------------------------------------------
# idea.system.path=${user.home}/.IntelliJIdea/system
idea.system.path=/path/to/IntelliJ IDEA 20xx.x.x/system

#---------------------------------------------------------------------
# Uncomment this option if you want to customize a path to the user-installed plugins directory.
#---------------------------------------------------------------------
# idea.plugins.path=${idea.config.path}/plugins
idea.plugins.path=${idea.config.path}/plugins

#---------------------------------------------------------------------
# Uncomment this option if you want to customize a path to the logs directory.
#---------------------------------------------------------------------
# idea.log.path=${idea.system.path}/log
idea.log.path=${idea.system.path}/log
```

## 修改Maven的配置路径和三方库安装路径



参考文献:

- [IntelliJ IDEA2020-自定义配置和缓存位置以及数据迁移](https://blog.csdn.net/qq_15769939/article/details/112649686)
- [Jetbrains 缓存清理与安装优化](https://youwu.today/blog/cleanup-jetbrains-ide-cache/#%E7%BC%93%E5%AD%98%E7%9B%AE%E5%BD%95)
