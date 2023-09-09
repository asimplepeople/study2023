# 网络概述

One of the reasons Docker containers and services are so powerful is that you can connect them together, or connect them to non-Docker workloads. Docker containers and services do not even need to be aware that they are deployed on Docker, or whether their peers are also Docker workloads or not. Whether your Docker hosts run Linux, Windows, or a mix of the two, you can use Docker to manage them in a platform-agnostic way.

This topic defines some basic Docker networking concepts and prepares you to design and deploy your applications to take full advantage of these capabilities.

## 本主题的范围

## 网络驱动

Docker’s networking subsystem is pluggable, using drivers. Several drivers exist by default, and provide core networking functionality:
Docker的网络子系统是可插拔的， 使用驱动程序。默认存在几个驱动程序，并提供核心的网络功能：

* bridge(桥接模式)： 这是默认的网络驱动，如果你不指定一个驱动的话，这就是你正在创建的网络类型。