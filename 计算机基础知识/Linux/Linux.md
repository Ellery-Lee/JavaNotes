# Linux [b站Linux](https://www.bilibili.com/video/BV1dW411M7xL)

## 一、基础篇

### 1、Linux介绍

- Linux怎么读：li ni ke si、li niu ke si、li na ke si

- Linux是一款操作系统，免费，开源，安全，高效，稳定，处理高并发非常强悍，现在很多的企业级的项目都部署到Linux/Unix服务器运行。

- Linux创始人：林纳斯

- Linux的吉祥物：企鹅 Tux

- Linux主要的发行版本

  ![Linux发行版.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/Linux%E5%8F%91%E8%A1%8C%E7%89%88.png?raw=true)

- 目前主要的操作系统有哪些

  windows、Android、车载系统、linux等。

## 2、安装VM和CentOS

学习Linux需要一个环境，我们需要创建一个虚拟机，然后在虚拟机上安装一个CentOS系统来学习。

- 先安装Virtual Machine VM12
- 再安装Linux(CentOS 6.8)
- 原理示意图

![Windows和VM和CentOS关系图.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/Windows%E5%92%8CVM%E5%92%8CCentOS%E5%85%B3%E7%B3%BB%E5%9B%BE.png?raw=true)

- 难点：虚拟机的网络连接三种形式的说明
  - 桥连接：Linux可以和其它的系统通信。但是可能造成IP冲突。
  - NAT(Network Address Translation)模式：Linux可以访问外网，不会造成IP冲突。
  - 主机模式：Linux是一个独立主机，不能访问外网。