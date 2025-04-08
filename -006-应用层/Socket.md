---
title: Socket
---



# 概述

- 在计算机通信领域，socket 被翻译为“套接字”，它是计算机之间进行通信的一种约定或一种方式
- 说白了就是将运输层协议的书写方法进行一个封装，使开发者不用在写项目时，不用每一个项目都要写一个用于生产完整运输层协议单元的工具。socket就是这个生成运输层协议单元的工具，开发者只要在程序中将应用层数据丢进去，让socket工具帮你封装成运输层协议单元

# 创建（C语言）

- ```
  int socket(int domain,int type,int protocol);
  ```

- | domain    | 描述               |
  | --------- | ------------------ |
  | PF_INET   | IPv4互联网的协议族 |
  | PF_INET6  | IPv6互联网的协议族 |
  | PF_LOCAL  | 本地通信的协议族   |
  | PF_PACKET | 内核底层的协议族   |
  | PF_IPX    | IPX Novell的协议族 |

- | type        | 描述                                            |
  | ----------- | ----------------------------------------------- |
  | SOCK_STREAM | 面向连接的socket（可靠服务，参照运输层协议TCP） |
  | SOCK_DGRAM  | 无连接的socket（不可靠服务，参照运输层协议UDP） |

- | protocol    | 描述            |
  | ----------- | --------------- |
  | IPPROTO_TCP | 搭配SOCK_STREAM |
  | IPPROTO_UDP | 搭配SOCK_DGRAM  |
  | 0           | 可填            |

- 成功返回一个有效的socket，失败返回-1

- 创建数量有限制

