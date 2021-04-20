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

### 2、安装VM和CentOS

学习Linux需要一个环境，我们需要创建一个虚拟机，然后在虚拟机上安装一个CentOS系统来学习。

- 先安装Virtual Machine VM12
- 再安装Linux(CentOS 6.8)
- 原理示意图

![Windows和VM和CentOS关系图.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/Windows%E5%92%8CVM%E5%92%8CCentOS%E5%85%B3%E7%B3%BB%E5%9B%BE.png?raw=true)

- 难点：虚拟机的网络连接三种形式的说明
  - 桥连接：Linux可以和其它的系统通信。但是可能造成IP冲突。
  - NAT(Network Address Translation)模式：Linux可以访问外网，不会造成IP冲突。
  - 主机模式：Linux是一个独立主机，不能访问外网。

### 3、CentOS的终端使用和联网

- 终端的使用，点击鼠标右键，即可选择打卡终端

- 配置网络，可以上网

  点击上面右侧的两个计算机图标，先择启用eth0，即可成功连接到网络，就可以上网。

### 4、VMTools安装

- 可以直接粘贴命令再windows和CentOS系统之间

- 可以设置windows和CentOS的共享文件夹

  ![vmtools示意图.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/vmtools%E7%A4%BA%E6%84%8F%E5%9B%BE.png?raw=true)

安装vmtools的步骤：

- 进入CentOS
- 点击vm菜单的-》install vmware tools
- CentOS会出现一个vm的安装包
- 点击右键解压，得到一个安装文件
- 进入该vm解压的目录，该文件在/boot/桌面/vmware-tools-distrib/下
- 安装./vmware-install.pl
- 全部使用默认设置即可
- 需要reboot重新启动即可生效

## 二、Linux目录结构

### 1、基本介绍

Linux的文件系统是采用级层式的树状目录结构，在此结构中的最上层是根目录“/”，然后在此目录下再创建其他的目录。**在Linux世界里，一切皆文件。**

![Linux目录结构.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/Linux%E7%9B%AE%E5%BD%95%E7%BB%93%E6%9E%84.png?raw=true)

总结：

- Linux的目录中有且只有一个根目录 /
- Linux的各个目录存放的内容是规划好，不用乱放文件。
- Linux是以文件的形式管理我们的设备，因此Linux系统，一切皆为文件。
- Linux的各个文件目录下存放什么内容，大家必须有一个认识。
- 学习后，你脑海中应该有一颗Linux目录树

## 三、远程登陆Linux系统

### 1、为什么需要远程登陆Linux

- Linux服务器是开发小组共享的
- 正式上线的项目是运行在公网的
- 因此程序员需要远程登录到CentOS进行项目管理或者开发
- 远程登录客户端有Xshell5，Xftp5，我们学习使用Xshell5和Xftp，其它的远程工具大同小异
- 画出简单网络拓扑示意图

![为什么需要远程登陆Linux.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/%E4%B8%BA%E4%BB%80%E4%B9%88%E9%9C%80%E8%A6%81%E8%BF%9C%E7%A8%8B%E7%99%BB%E9%99%86Linux.png?raw=true)

- 如果希望安装好XShell5就可以远程访问Linux系统的话，需要有一个前提，就是Linux启用了sshd服务，该服务会监听22号端口。

### 2、安装Xshell并使用

[官网下载](https://www.netsarang.com/zh/xshell/)，非商业用途免费使用

- 使用Xshell远程连接Linux

![Xshell连接Linux.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/Xshell%E8%BF%9E%E6%8E%A5Linux.png?raw=true)

Xshell远程登陆到Linux后，就可以使用指令来操作Linux系统

### 3、安装Xftp5并使用

[官网下载](https://www.netsarang.com/zh/xshell/)，非商业用途免费使用

- 使用Xftp连接Linux

![Xftp连接Linux.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/Xftp%E8%BF%9E%E6%8E%A5Linux.png?raw=true)

- 连接到Linux界面如下

![Xftp连接到linux界面如下.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/Xftp%E8%BF%9E%E6%8E%A5%E5%88%B0linux%E7%95%8C%E9%9D%A2%E5%A6%82%E4%B8%8B.png?raw=true)

## 四、vi和vim编辑器

### 1、基本介绍

所有Linux系统都会内建vi文本编辑器。Vim具有程序编辑的能力，可以看做是Vi的增强版本，可以主动的以字体颜色辨别语法的正确性，方便程序设计。代码补全、编译及错误跳转等方便编程的功能特别丰富，在程序员中被广泛使用。

### 2、vi和vim的三种常见模式

- 正常模式

  在正常模式下，我们可以使用快捷键

  以vim打开一个档案就直接进入一般模式了(这是默认的模式)。在这个模式中，你可以使用[上下左右]按键来移动光标，你可以使用[删除字符]或[删除整行]来处理档案内容，也可以使用[复制、贴上]来处理你的文件数据。

- 插入/编辑模式

  在此模式下，程序员可以输入内容

  按下i,l,o,O,a,A,r,R等任何一个字母之后才会进入编辑模式，一般来说按i即可

- 命令行模式

  在此模式下，可以提供你相关指令。完成读取、存盘、替换、离开vim、显示行号等的动作。

![vi和vim模式的相互转换.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/vi%E5%92%8Cvim%E6%A8%A1%E5%BC%8F%E7%9A%84%E7%9B%B8%E4%BA%92%E8%BD%AC%E6%8D%A2.png?raw=true)

### 3、vi和vim常用快捷键

- 拷贝当前行 yy，拷贝当前行向下的5行 5yy，并粘贴(p)
- 删除当前行 dd，删除当前行向下的5行 5dd
- 在文件中查找某个单词[命令行下 /关键字， 回车 查找，输入n就是查找下一个]
- 设置文件的行号，取消文件的行号[命令行下 :set nu 和  :set nonu]
- 编辑 /etc/profile 文件，使用快捷到达文档的最末行[G]和最首行[gg]，注意这些都是在正常模式下执行的。
- 在一个文件中输入“hello”，然后又撤销这个动作。在正常模式下输入  u
- 编辑 /etc/profile 文件，并将光标移动到 20行 G
  - 第一步：显示行号 :set nu
  - 第二步：输入20这个数
  - 第三步：输入G