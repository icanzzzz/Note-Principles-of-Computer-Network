---
title: 网际控制报文协议ICMP
---



# Remind

- 为了更有效地转发IP数据报和提高交付成功的机会，在网际层使用了网际控制报文协议ICMP（Internet Control Message Protocol）
- 主机或路由器使用ICMP来发送==差错报告报文==和==询问报文==
- ==ICMP报文被封装在IP数据报==中发送

# 五种==ICMP差错报告报文==

## 终点不可达

- 当路由器或主机不能交付数据报时，就向源点发送终点不可达报文。具体可再根据ICMP的代码字段细分为目的网络不可达、目的主机不可达、目的协议不可达、目的端口不可达、目的网络未知、目的主机未知等13种错误
- ![image-20250306201920268](./resource/image-20250306201920268.png)

## 源点抑制

- 当路由器或主机由于拥塞而丢弃数据报时，就向源点发送源点抑制报文，使源点知道应当把数据报的发送速率放慢

## 时间超过

- 当路由器收到一个目的IP地址不是自己的IP数据报，会将其生存时间TTL字段的值减1
  - 若结果不为0，则将该IP数据报转发出去
  - 若结果为0，则除丢弃该IP数据报外，还要向源点发送时间超过报文
- 当终点在预先规定的时间内不能收到一个数据报的全部数据报片时，就把已收到的数据报片都丢弃，也会向源点发送时间超过报文

## 参数问题

- 当路由器或目的主机收到IP数据报后，根据其首部中的检验和字段发现首部在传输过程中出现了误码，就丢弃该数据报，并向源点发送参数问题报文

## 改变路由（重定向）

- 路由器把改变路由报文发送给主机，让主机知道下次应将数据报发送给另外的路由器（可通过更好的路由）

## Tip

- ==不应发送ICMP差错报告报文==的情况
  - 对ICMP差错报告报文不再发送ICMP差错报告报文
  - 对第一个分片的数据报片的所有后续数据报片都不发送ICMP差错报告报文
  - 对具有多播地址的数据报都不发送ICMP差错报告报文
  - 对具有特殊地址（如127.0.0.0或0.0.0.0）的数据报不发送ICMP差错报告报文

# 常用的==ICMP询问报文==

## ==回送请求和回答==

- ICMP回送请求报文是由主机或路由器向一个特定的目的主机发出的询问
- 收到此报文的主机必须给源主机或路由器发送ICMP回送回答报文
- 这种询问报文用来==测试目的站是否可达==及了解其有关状态

## ==时间戳请求和回答==

- ICMP时间戳请求报文是请某个主机或路由器回答当前的日期和时间
- 在ICMP时间戳回答报文中有一个32位的字段，其中写入的整数代表从1900年1月1日起到当前时刻一共有多少秒
- 这种询问报文用来==进行时钟同步和测量时间==

# ICMP应用举例

## 分组网间探测PING（Packet InterNet Groper）

- 用来测试主机或路由器间的连通性
- 应用层直接使用网际层的ICMP（没有通过运输层的TCP或UDP）
- 使用ICMP回送请求和回答报文

## 跟踪路由traceroute

- 测试IP数据报从源主机到达目的主机要经过哪些路由器
- Windows版本
  - tracert命令
  - 应用层直接使用网际层ICMP
  - 使用了ICMP回送请求和回答报文以及差错报告报文
- Unix版本
  - traceroute命令
  - 在运输层使用UDP协议
  - 仅使用ICMP差错报告报文
- 原理
  1. 开始发送TTL=1的ICMP回送请求
  2. 第一个ICMP回送请求到达第一个路由器后TTL=0，第一个路由丢弃IP数据报并给源主机发送ICMP差错报告（时间超过）
  3. 源主机知道了到达目的主机路径中的第一个路由器
  4. 接着依次发送TTL=2、3、···、n的ICMP回送请求，获取到达目的主机路径中的第2、3、···、n个路由器，最后成功到达目的主机，目的主机发送ICMP回送请求的回答报文给源主机

