---
title: docker踩坑
date: 2022-08-14 12:23:22
tags: [docker, docker-compose]
---
1. docker-compose 中service下配置的`expose`并不是真正会暴露出的端口，只是说明有可能有这些端口会暴露出去，真正管理哪些端口暴露的还是在`ports`配置项中的
2. docker内部绑定到`127.0.0.1`下的端口配置了端口映射到外部，在外部访问时却报`connect reset by peer`
应改为绑定到`0.0.0.0`,原理是容器外部访问容器内部服务时使用的网段是172.17.\*.\*，所以访问不到