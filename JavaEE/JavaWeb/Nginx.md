# Nginx

# 一、Nginx基本概念

## 1、什么是Nginx

*Nginx* (engine x) 是一个高性能的[HTTP](https://baike.baidu.com/item/HTTP)和[反向代理](https://baike.baidu.com/item/反向代理/7793488)web服务器，同时也提供了IMAP/POP3/SMTP服务。

Nginx是一款[轻量级](https://baike.baidu.com/item/轻量级/10002835)的[Web](https://baike.baidu.com/item/Web/150564) 服务器/[反向代理](https://baike.baidu.com/item/反向代理/7793488)服务器及[电子邮件](https://baike.baidu.com/item/电子邮件/111106)（IMAP/POP3）代理服务器，在BSD-like 协议下发行。其特点是占有内存少，[并发](https://baike.baidu.com/item/并发/11024806)能力强，事实上nginx的并发能力在同类型的网页服务器中表现较好。

## 2、反向代理

Nginx不仅可以做反向代理，实现负载均衡。还能用作正向代理来进行上网等功能。

- 正向代理：

  如果把局域网外的Internet想象成一个巨大的资源库，则局域网中的客户端要访问Internet，则需要通过代理服务器来访问，这种代理服务就称为正向代理。

  在客户端(浏览器)配置代理服务器，通过代理服务器进行互联网访问

  ![正向代理](D:\JavaHub\学习相关\Java笔记\pictures\正向代理.png)

- 反向代理：

  其实客户端对代理是无感知的，因为客户端不需要任何配置就可以访问，我们只需要将请求发送到反向代理服务器，由反向代理服务器去选择目标服务器获取数据后，再返回给客户端，此时反向代理服务器和目标服务器对外就是一个服务器，暴露的是代理服务器地址，隐藏了真实服务器IP地址。

  ![反向代理](D:\JavaHub\学习相关\Java笔记\pictures\反向代理.png)

## 3、负载均衡

单个服务器解决不了高并发问题，增加服务器数量，然后将请求分发到各个服务器上，将原先请求集中到单个服务器上的情况改为将请求分发到多个服务器上，将负载分发到不同的服务器，也就是我们所说的负载均衡。

![负载均衡](D:\JavaHub\学习相关\Java笔记\pictures\负载均衡.png)

## 4、动静分离

为了加快网站的解析速度，可以把动态页面和静态页面由不同的服务器来解析，加快解析速度。降低原来单个服务器的压力。

![动静分离](D:\JavaHub\学习相关\Java笔记\pictures\动静分离.png)

# 二、Nginx操作的常用命令

使用nginx操作命令的前提条件是进入到nginx的目录中去 `/usr/local/nginx/sbin`

- 查看nginx的版本号

  `./nginx -v`

- 启动nginx

  `./nginx`

- 关闭nginx

  `./nginx -s stop`

- 重新加载nginx，将配置文件重新加载

  `./nginx -s reload`

# 三、nginx配置文件

## 1、配置文件位置

`/usr/local/nginx/conf`

## 2、配置文件组成

nginx配置文件由三部分组成

- 第一部分：全局块

  从配置文件开始到events块之间的内容，主要会设置一些影响nginx服务器整体运行的配置指令，主要包括配置运行Nginx服务器的用户(组)、允许生成的worker process数，进程PID存放路径、日志存放路径和类型以及配置文件的引入等。

  - `worker_processes  1;`

    这是Nginx服务器并发处理服务的关键配置，worker_processes值越大，可以支持的并发处理量也越多，但是会受到硬件、软件等设备的制约。

- 第二部分：events块

  events块涉及的指令主要影响Nginx服务器与用户的网络连接，常用的设置包括是否开启对多worker_process下的网络连接进行序列化，是否允许同时接收多个网络连接，选取哪种事件驱动模型来处理连接请求，每个worker_process可以同时支持的最大连接数等。

  这部分的配置对Nginx的性能影响较大，在实际中应该灵活配置。

- 第三部分：http块

  这算是Nginx服务器配置中最频繁的部分，代理、缓存和日志定义等绝大多数功能和第三方模块的配置都在这里。需要注意的是：http块也可以包括http全局块、server块。

  - http全局块

    http全局块配置的指令包括文件引入、MIME-TYPE定义、日志自定义、连接超时时间、单连接请求数上限等。

  - server块

    这块和虚拟主机有密切关系，虚拟主机从用户角度看，和一台独立的硬件主机是完全一样的，该技术的产生是为了节省互联网服务器硬件成本。

    每个http块可以包括多个server块，而每个server块就相当于一个虚拟主机。

    而每个server块也分为全局server块，以及可以同时包含多个location块。

    - 全局server块

      最常见的配置是本虚拟机主机的监听配置和本虚拟主机的名称或IP配置。

    - location块

      一个server块可以配置多个location块。

      这块的主要作用是基于Nginx服务器接收到的请求字符串(例如 server_name/uri-string)，对虚拟主机名称(也可以是IP别名)之外的字符串(例如 前面的 /uri-string)进行匹配，对特定的请求进行处理。地址定向、数据缓存和应答控制等功能，还有许多第三方模块的配置也在这里进行。

      

# 四、Nginx配置实例-反向代理1

## 1、实现效果

打开浏览器，在浏览器地址栏输入地址 www.123.com ,跳转linux系统tomcat主页面中。

## 2、准备工作

- 在linux系统中安装tomcat，使用默认端口8080

  tomcat安装文件放到linux系统中，解压

  进入tomcat的bin目录中，./startup.sh启动tomcat服务器

- 对外开放访问的端口

  firewall-cmd --add-port=8080/tcp --permanent

  firewall-cmd -reload

- 在windows系统中通过浏览器访问tomcat服务器

## 3、分析访问过程

![Nginx访问过程](D:\JavaHub\学习相关\Java笔记\pictures\Nginx访问过程.png)

## 4、具体配置

第一步：在windows系统的host文件进行域名和ip对应关系的配置

`C:\Windows\System32\drivers\etc`

添加内容在host文件中

第二步：Nginx请求转发配置

![nginx请求转发配置](D:\JavaHub\学习相关\Java笔记\pictures\nginx请求转发配置.png)

# 五、Nginx配置实例-反向代理2

## 1、实现效果

使用Nginx反向代理，根据访问的路径跳转到不同端口的服务中。Nginx监听端口为9001

## 2、准备工作

- 准备两个tomcat服务器，一个8080端口，一个8081端口
- 创建文件夹和测试页面

## 3、具体配置

- 找到Nginx配置文件，进行反向代理

  ![Nginx端口转发实例2配置](D:\JavaHub\学习相关\Java笔记\pictures\Nginx端口转发实例2配置.png)

- 开放对外访问的端口号 9001 8080 8081

## 4、最终测试

## 5、location指令说明

- =：用于不含正则表达式的uri前，要求请求字符串与uri严格匹配，如果匹配成功，就停止继续向下搜索并立即处理该请求。
- ~：用于表示uri包含正则表达式，并且区分大小写。
- ~*：用于表示uri包含正则表达式，并且不区分大小写
- ~~：用于不含正则表达式的uri前，要求Nginx服务器找到标识uri和请求字符串匹配度最高的location后，立即使用此location处理请求，而不再使用location块中的正则uri和请求字符串做匹配
- 注意：如果uri包含正则表达式，则必须要有\~或者\~标识
