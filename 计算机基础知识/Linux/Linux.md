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

## 五、开机、重启和用户登录注销

### 1、关机和重启命令

- shutdown

  - shutdown -h now：表示立即关机
  - shutdown -h 1：表示1分钟后关机
  - shutdown -r now：立即重启

- halt

  直接使用，效果等价于关机

- reboot

  重启系统

- sync

  把内存的数据同步到磁盘上

**注意细节：**当我们关机或者重启时，都应该先执行以下指令，把内存的数据写入磁盘，防止数据丢失。

### 2、用户登录和注销

基本介绍：

- 登陆时尽量少用root账号登录，因为它是系统管理员，最大的权限，避免操作失误。可以利用普通用户登录，登陆后再用“su- 用户名”命令来切换成系统管理员身份。
- 在提示符下输入logout即可注销用户。

## 六、用户管理

### 1、基本介绍

![用户管理.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/%E7%94%A8%E6%88%B7%E7%AE%A1%E7%90%86.png?raw=true)

说明：

- Linux系统是一个多用户多任务的操作系统，任何一个要使用系统资源的用户，都必须首先向系统管理员申请一个账号，然后以这个账号的身份进入系统。
- Linux的用户至少要属于一个组。

### 2、添加用户

- useradd [选项] 用户名

  cd表示 change directory，切换目录

细节说明：

- 当创建用户成功后，会自动创建和用户同名的家目录
- 也可以通过useradd -d指定目录  新的用户名，给新创建的用户指定家目录

### 3、指定密码

- passwd 用户名

### 4、删除用户

- userdel 用户名        删除用户，但是保留家目录
- userdel -r 用户名       删除用户以及用户家目录

在删除用户时，我们一般不会将家目录删除。

### 5、查询用户信息/切换用户

- id 用户名

![查询用户.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/%E6%9F%A5%E8%AF%A2%E7%94%A8%E6%88%B7.png?raw=true)

- su - 切换用户名

![切换用户.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/%E5%88%87%E6%8D%A2%E7%94%A8%E6%88%B7.png?raw=true)

细节说明:

- 从权限高的用户切换到权限低的用户，不需要输入密码，反之需要。
- 当需要返回到原来用户时，使用exit指令

### 6、查看当前用户/登录用户

- whoami / who am I

### 7、用户组

介绍：类似于角色，系统可以对有共性的多个用户进行统一的管理。

- 添加组：groupadd 组名
- 删除组：groupdel 组名
- 增加用户时直接加上组：useradd -g 用户组 用户名

![查询用户.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/%E6%9F%A5%E8%AF%A2%E7%94%A8%E6%88%B7.png?raw=true)

- 修改用户组：usermod -g 用户组 用户名

### 8、用户和组的相关文件

- /etc/passwd文件

  用户(user)的配置文件，记录用户的各种信息

  每行的含义：用户名：口令：用户标识号：组标识号：注释行描述：主目录：登录Shell

- /etc/shadow文件

  口令的配置文件

  每行的含义：登录名：加密口令：最后一次修改时间：最小时间间隔：最大时间间隔：警告时间：不活动时间：失效时间：标志

- /etc/group文件

  组(group)的配置文件，记录Linux包含的组的信息

  每行含义：组名：口令：组标识号：组内用户列表

## 七、实操篇 实用指令

### 1、指定运行级别

![Linux系统运行级别.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/Linux%E7%B3%BB%E7%BB%9F%E8%BF%90%E8%A1%8C%E7%BA%A7%E5%88%AB.png?raw=true)

### 2、切换到指定运行级别的指令

基本语法：

- init [012356]

![查看运行级别.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/%E6%9F%A5%E7%9C%8B%E8%BF%90%E8%A1%8C%E7%BA%A7%E5%88%AB.png?raw=true)

**面试题：**如何找回root密码，如果我们不小心忘记了root密码，怎么找回？

思路:进入到单用户模式，然后修改root密码。因为进入单用户模式，root不需要密码就可以登陆。

步骤：

- 开机->在引导时输入 回车键->看到一个界面输入 e ->看到一个新的界面，选中第二行(编辑内核) 在输入 e->在这行最后输入 1，再输入 回车键->再次输入b，这时就会进入到单用户模式
- 这时我们就进入到单用户模式，使用passwd指令来修改root密码

### 3、帮助指令

当我们对某个指令不熟悉时，我们可以使用Linux提供的帮助指令来了解这个指令的使用方法。

- man 获取帮助信息
  - 基本语法：man [命令或配置文件] (功能描述：获得帮助信息)
- help 指令
  - 基本语法：help 命令(功能描述：获得shell内置命令的帮助信息)

当我们对一个指令不熟悉，使用上述两个命令或者使用百度搜索。

### 4、文件目录类指令

- pwd指令
  - 基本语法：pwd (功能描述：显示当前工作目录的绝对路径)
  
- ls指令
  - 基本语法：ls [选项] [目录或者文件]
  - 常用选项：
    - -a：显示当前目录所有的文件和目录，包括隐藏的
    - -l：以列表的方式显示信息
  
- cd指令
  - 基本语法：cd [参数] (功能描述：切换到指定目录)
  - 常用参数
    - 绝对路径和相对路径
    - cd ~ 或者 cd：回到自己的家目录
    - cd..：回到当前目录的上一级目录
  - 思考题：当前工作目录是/root，我们希望进入到/home下
    - 绝对路径：/home，即从根目录开始定位
    - 相对路径：../home，从当前工作目录开始定位到需要的目录去

- mkdir指令

  - 基本语法：mkdir [选项] 要创建的目录
  - 常用选项
    - -p：创建多级目录

- rmdir指令

  - 基本语法：rmdir [选项] 要删除的空目录
  - 使用细节：如果需要删除非空目录，需要使用rm -rf 要删除的目录

  ![删除目录.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/%E5%88%A0%E9%99%A4%E7%9B%AE%E5%BD%95.png?raw=true)

- touch指令 创建空文件

  - 基本语法：touch 文件名称

- cp指令 拷贝文件到指定目录

  - 基本语法：cp [选项] source dest
  - 常用选项：
    - ![强制覆盖cp.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/%E5%BC%BA%E5%88%B6%E8%A6%86%E7%9B%96cp.png?raw=true)

- rm指令 移除文件或目录

  - 基本语法：rm [选项] 要删除的文件或目录
  - 常用选项：
    - -r：递归删除整个文件夹
    - -f：强制删除不提示

- mv指令 移动文件与目录或重命名

  - 基本语法：
    - mv oldNameFile newNameFile (功能描述：重命名)
    - mv /temp/movefile /targetFolder (功能描述：移动文件)

- cat指令 查看文件内容，cat是以只读的方式打开

  - 基本语法：cat [选项] 要查看的文件

  - 常用选项：

    - -n：显示行号

    ![cat分页.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/cat%E5%88%86%E9%A1%B5.png?raw=true)

  - 使用细节：cat只能浏览文件，而不能修改文件，为了浏览方便，一般会带上管道命令 | more

- more指令 是一个基于VI编辑器的文本过滤器，它以全屏幕的方式按页显示文本文件的内容。more指令中内置了若干快捷键。

  - 基本语法：more 要查看的文件

  ![more指令快捷键.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/more%E6%8C%87%E4%BB%A4%E5%BF%AB%E6%8D%B7%E9%94%AE.png?raw=true)

- less指令 用来分屏查看文件内容，它的功能与more指令类似，但是比more指令更加强大，支持各种显示终端。less指令在显示文件内容时，并不是一次将整个文件加载之后才显示，而是根据显示需要加载的内容，对于显示大型文件具有较高的效率。

  - 基本语法：less 要查看的文件

  ![less指令快捷键.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/less%E6%8C%87%E4%BB%A4%E5%BF%AB%E6%8D%B7%E9%94%AE.png?raw=true)

- \>指令 和 \>>指令：\>输出重定向和\>>追加

  - 基本语法：
    - ls -l >文件 (功能描述：列表的内容覆盖写入文件a.txt中)
    - ls -al>>文件 (功能描述：列表的内容追加到文件aa.txt的末尾)
    - cat 文件1>文件2 (功能描述：将文件1的内容覆盖到文件2)
    - echo "内容">>文件 (功能描述：将内容追加到文件末尾)

- echo指令：输出内容到控制台

  - 基本语法：echo [选项] [输出内容]

- head指令：用于显示文件的开头部分内容，默认情况下head指令显示文件的前10行内容

  - 基本语法：
    - head 文件 (功能描述：查看文件头10行内容)
    - head -n 5 文件 (功能描述：查看文件头5行内容，5可以是任意行数)

- tail指令：用于输出文件中尾部的内容，默认情况下tail指令显示文件的后10行内容

  - 基本语法：
    - tail 文件 (功能描述：查看文件后10行内容)
    - tail -n 5 文件 (功能描述：查看文件后5行内容，5可以是任意行数)
    - **tail -f 文件 (功能描述：实时追踪该文档的所有更新)**
  
- ln指令：软链接也叫符号链接，类似于windows里的快捷方式，主要存放了链接其他文件的路径。

  - 基本语法：
    - ln -s [原文件或目录] [软链接名] (功能描述：给原文件创建一个软链接)
    - rm -rf [软链接名] (功能描述：删除软链接，注意软链接名后面不要带"/")
  - 细节说明：当我们使用pwd指令查看目录时，仍然看到的是软链接所在目录

- history指令：查看已经执行过的历史命令，也可以执行历史命令

  - 基本语法：history [显示行数] (功能描述：查看已执行的历史指令)

### 5、时间日期类

- date指令：显示当前日期
  - 基本语法：
    - date (功能描述：显示当前时间)
    - date +%Y (功能描述：显示当前年份)
    - date +%m (功能描述：显示当前月份)
    - date +%d (功能描述：显示当前是哪一天)
    - date “+%Y-%m-%d %H:%M:%S” (功能描述：显示年月日时分秒)
- date指令：设置日期
  - 基本语法：date -s 字符串时间
- cal指令：查看日历指令
  - 基本语法：cal[选项] (功能描述：不加选项，显示本月日历)

### 6、搜索查找类

- find指令：find指令将从指定目录向下递归地遍历其各个子目录，将满足条件的文件或者目录显示在终端。

  - 基本语法：find [搜索范围] [选项]

  ![find指令选项.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/find%E6%8C%87%E4%BB%A4%E9%80%89%E9%A1%B9.png?raw=true)

- locate指令：locate指令可以快速定位文件路径。locate指令利用事先建立的系统中所有文件名称及路径的locate数据库实现快速定位给定的文件。

  - 基本语法：locate 搜索文件

  - 特别说明：由于locate指令基于数据库查询，所以第一次运行前，必须使用updatedb指令创建locate数据库。locate查询是模糊查询。

- grep指令和管道符号|：grep过滤查找，管道符“|”，表示将前一个命令的处理结果输出传递给后面的命令处理。
  - 基本语法：grep [选项] 查找内容 源文件
  - 常用选项：
    - -n：显示匹配行及行号
    - -i：忽略字母大小写

### 7、压缩和解压缩类

- gzip/gunzip指令：gzip用于压缩文件，gunzip用于解压

  - 基本语法：
    - gzip文件 (功能描述：压缩文件，只能将文件压缩为*.gz文件)
    - gunzip 文件.gz (功能描述：解压缩文件命令)

- zip/unzip指令：zip用于压缩文件，unzip用于解压文件，在项目打包发布中很有用

  - 基本语法：
    - zip [选项] xxx.zip 将要压缩的内容(功能描述：压缩文件和目录的命令)
    - unzip [选项] xxx.zip (功能描述：解压缩文件)
  - 常用选项：
    - -r ：递归压缩，即压缩目录
    - -d <目录>：指定解压后文件的存放目录

- tar指令：打包指令，最后打包的文件是 .tar.gz的文件

  - 基本语法：tar [选项] XXX.tar.gz 打包的内容 (功能描述：打包目录，压缩后的文件格式 .tar.gz)

    <img src="https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/tar%E6%8C%87%E4%BB%A4%E9%80%89%E9%A1%B9.png?raw=true" alt="tar指令选项.png"  />

## 八、组管理和权限管理

### 1、Linux组的基本介绍

在Linux中的每个用户必须属于一个组，不能独立于组外。在Linux中每个文件有所有者、所在组、其他组的概念。

### 2、文件/目录所有者

一般为文件的创建者，谁创建了该文件，就自然成为该文件的所有者。

- 查看文件的所有者
  - 指令：ls -ahl
- 修改文件的所有者
  - 指令：chown 用户名 文件名

### 3、文件/目录所在组

当某个用户创建了一个文件后，默认这个文件的所在组就是该用户所在的组。

- 查看文件/目录所在组
  - 基本指令：ls -ahl
- 修改文件所在的组
  - 基本指令：chgrp 组名 文件名

### 4、改变用户所在组

在添加用户时，可以指定将该用户添加到哪个组中，同样的用root的管理权限可以改变某个用户所在的组。

- 指令：
  - usermod -g 组名 用户名
  - usermod -d 目录名 用户名 改变该用户登录的初始目录

### 5、权限的基本介绍

![权限介绍.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/%E6%9D%83%E9%99%90%E4%BB%8B%E7%BB%8D.png?raw=true)

- rwx作用到文件
  - [r]代表可读：可以读取、查看
  - [w]代表可写：可以修改，但是不代表可以删除文件，删除一个文件的前提条件是对该文件所在的目录有写权限，才能删除该文件
  - [x]代表可执行：可以被执行
- rwx作用到目录
  - [r]代表可读：可以读取，ls查看目录内容
  - [w]代表可写：可以修改，目录创建+删除+重命名目录
  - [x]代表可执行：可以进入该目录

### 6、修改权限 -chmod

- 第一种方式：+、-、=变更权限
  - u：所有者 g：所有组 o：其他人 a：所有人(u、g、o的总和)
  - chmod u=rwx,g=rx,o=x 文件目录名
  - chmod o+w 文件目录名
  - chmod a-x 文件目录名
- 第二种方式：通过数字变更权限
  - 规则：r=4 w=2 x=1      rwx=4+2+1=7
  - chmod u=rwx,g=rx,o=x 文件目录名    相当于     chmod 751

### 7、修改文件所有者 -chown

- 基本介绍
  - chown newowner file 改变文件的所有者
  - chown newowner:newgroup file 改变用户的所有者和所有组
  - -R 如果是目录 则使其下所有子文件或目录递归生效

### 8、修改文件所在组 -chgrp

- 基本介绍
  - chgrp newgroup file 改变文件的所有组

## 九、crond  任务调度

crontab 定时任务设置

任务调度：是指系统在某个时间执行的特定的命令或程序

任务调度分类：

- 系统工作：有些重要的工作必须周而复始地执行。如病毒扫描等
- 个别用户工作：个别用户可能希望执行某些程序，比如对mysql数据库的备份

基本语法：

- crontab [选项]

![crontab指令常用选项.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/crontab%E6%8C%87%E4%BB%A4%E5%B8%B8%E7%94%A8%E9%80%89%E9%A1%B9.png?raw=true)



![任务调度参数说明.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/%E4%BB%BB%E5%8A%A1%E8%B0%83%E5%BA%A6%E5%8F%82%E6%95%B0%E8%AF%B4%E6%98%8E.png?raw=true)



![特殊符号细节说明.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/%E7%89%B9%E6%AE%8A%E7%AC%A6%E5%8F%B7%E7%BB%86%E8%8A%82%E8%AF%B4%E6%98%8E.png?raw=true)

## 十、Linux磁盘分区、挂载

### 1、分区基础知识

- mbr分区
  - 最多支持四个主分区
  - 系统只能安装在主分区
  - 扩展分区要占一个主分区
  - MBR最大只支持2TB，但拥有最好的兼容性
- gtp分区
  - 支持无限多个主分区(但操作系统可能限制，比如windows下最多128个分区)
  - 最大支持18EB的大容量(EB=1024PB，PB=1024TB)
  - windows7 64位后支持gtp

### 2、Linux分区

原理介绍：

- Linux无论有几个分区，分给哪个目录使用，它归根结底就只有一个根目录，一个独立且唯一的文件结构，Linux中每个分区都是用来组成整个文件系统的一部分。
- Linux采用了一种叫“载入”的处理方法，它的整个文件系统中包含了一整套的文件和目录，且将一个分区和一个目录联系起来。这时要载入的一个分区将使它的存储空间在一个目录下获得。

![Linux分区原理示意图.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/Linux%E5%88%86%E5%8C%BA%E5%8E%9F%E7%90%86%E7%A4%BA%E6%84%8F%E5%9B%BE.png?raw=true)

硬盘说明：

- Linux硬盘分IDE硬盘和SCSI硬盘，目前基本上是SCSI硬盘
- 对于IDE硬盘，驱动器标识符为“hdx~”，其中“hd”表明分区所在设备的类型，这里是指IDE硬盘。“x”为盘号(a为基本盘，b为基本从属盘，c为辅助主盘，d为辅助从属盘)。“ ~”代表分区，前四个分区用数字1到4表示，它们是主分区或扩展分区，从5开始就是逻辑分区。
- 对于SCSI硬盘则标识为“sdx~”，SCSI硬盘是用“sd”来表示分区所在设备的类型的，其余则和IDE硬盘的表示方法一样。

### 3、使用lsblk指令查看当前系统的分区情况

![查看系统分区和挂载情况.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/%E6%9F%A5%E7%9C%8B%E7%B3%BB%E7%BB%9F%E5%88%86%E5%8C%BA%E5%92%8C%E6%8C%82%E8%BD%BD%E6%83%85%E5%86%B5.png?raw=true)

### 4、如何增加一块硬盘

- 在【虚拟机】菜单中，选择【设置】，然后设备列表里添加硬盘，然后一路【下一步】，中间只有选择磁盘大小的地方需要修改，至到完成。然后重启系统（才能识别）！

- 分区命令  fdisk   /dev/sdb

  开始对/sdb 分区

  •      m  显示命令列表

  •      p                     显示磁盘分区 同 fdisk –l

  •      n  新增分区

  •       d 删除分区

  •      w  写入并退出

  说明： 开始分区后输入 n，新增分区，然后选择 p ，分区类型为主分区。两次回车默认剩余全部空间。最后输入 w 写入分区并退出，若不保存退出输入 q。

- 格式化磁盘

  分区命令:mkfs -t  ext4   /dev/sdb1

  其中 ext4 是分区类型

- 挂载：将一个分区与一个目录联系起来

  •           mount     设备名称  挂载目录

  •                    例如：  mount    /dev/sdb1   /newdisk

  •           umount          设备名称  或者 挂载目录

  •          例如：   umount                 /dev/sdb1 或 者  umount /newdisk

- 永久挂载

  永久挂载: 通过修改/etc/fstab 实现挂载添加完成后  执行 mount –a 即刻生效

### 5、磁盘情况查询

- 查询系统整体磁盘使用情况

  - 基本语法：df -h

- 查询指定目录的磁盘占用情况

  - 基本语法：du -h /目录

    -s 指定目录占用大小汇总

    -h 带计量单位

    -a 含文件

    --max-depth=1 子目录深度

## 十一、网络配置

### 1、Linux网络配置原理图(含虚拟机)

![Linux网络环境.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/Linux%E7%BD%91%E7%BB%9C%E7%8E%AF%E5%A2%83.png?raw=true)



### 2、Linux网络环境配置

- 第一种：自动获取

![Linux网络自动配置.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/Linux%E7%BD%91%E7%BB%9C%E8%87%AA%E5%8A%A8%E9%85%8D%E7%BD%AE.png?raw=true)

缺点: linux 启动后会自动获取 IP,缺点是每次自动获取的 ip 地址可能不一样。这个不适用于做服务器，因为我们的服务器的 ip 需要是固定的。

- 第二种：指定固定的ip

  直接修改配置文件来指定IP，并可以连接到外网(程序员推荐)，编辑vi /etc/sysconfig/network-scripts/ifcfg-eth0

  ![Linux网络手动配置.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/Linux%E7%BD%91%E7%BB%9C%E6%89%8B%E5%8A%A8%E9%85%8D%E7%BD%AE.png?raw=true)

修改后一定要重启服务

1) service network restart

2) reboot

## 十二、进程管理

### 1、进程的基本介绍

- 在Linux中，每个执行的程序(代码)都称为一个进程。每一个进程都分配一个ID号
- 每一个进程，都会对应一个父进程，而这个父进程可以复制多个子进程。例如www服务器。
- 每个进程都可能以两种方式存在。前台与后台，所谓前台进程就是用户目前的屏幕上可以进行操作的。后台进程则是实际在操作，但由于屏幕上无法看到的进程，通常使用后台方式执行。
- 一般系统的服务都是以后台进程的方式存在，而且都会常驻在系统中。直到关机才结束。

### 2、显示系统执行的进程

ps命令用来查看目前系统中，有哪些正在执行，以及它们执行的状况。可以不加任何参数

- ps -a：显示当前终端所有进程信息
- ps -u：以用户的格式显示进程信息
- ps -x：显示后台进程运行的参数

![Linux进程信息](D:\JavaHub\学习相关\Java笔记\pictures\Linux进程信息.png)

### 3、ps指令详解

- ps -aux | grep xxx, 比如看看有没有sshd服务

![ps指令说明](D:\JavaHub\学习相关\Java笔记\pictures\ps指令说明.png)



- ps -ef | grep xxx 以全格式显示当前所有进程，查看进程的父进程

  ![ps-ef指令详解](D:\JavaHub\学习相关\Java笔记\pictures\ps-ef指令详解.png)

### 4、终止进程kill和killall

若是某个进程执行一半需要停止时，或是已消了很大的系统资源时，此时可以考虑停止该进程。使用kill命令来完成此项任务。

基本语法：

- kill [选项] 进程号 (功能描述：通过进程号杀死进程)
- killall 进程名称 (功能描述：通过进程名称杀死进程，也支持通配符，这在系统因负载过大而变得很慢时有用)

常用选项：

-9：表示强迫进程立即停止

### 5、查看进城树pstree

基本语法：

- pstree [选项]，可以更加直观的来看进程信息

常用选项：

-p：显示进程的PID

-u：显示进程的所属用户

### 6、服务(service)管理

服务(service)本质就是进程，但是是运行在后台的，通常都会监听某个端口，等待其他进程的请求，比如(mysql,sshd防火墙等)，因此我们又称为守护进程，是Linux中非常重要的知识点。

service管理指令：

- service 服务名 start | stop | restart | reload | status
- 在CentOS7.0后 不再使用service，而是systemctl

![服务管理原理图](D:\JavaHub\学习相关\Java笔记\pictures\服务管理原理图.png)

查看服务名：

- 使用setup -> 系统服务就可以看到
- /etc/init.d/服务名称

服务的运行级别(runlevel):

- 查看或者修改默认级别： vi/etc/inittab

![Linux运行级别](D:\JavaHub\学习相关\Java笔记\pictures\Linux运行级别.png)

### 7、开机流程说明

![开机流程](D:\JavaHub\学习相关\Java笔记\pictures\开机流程.png)

### 8、chkconfig指令

通过chkconfig命令可以给每个服务的各个运行级别设置自启动/关闭

基本语法：

- 查看服务 chkconfig --list | grep xxx
- chkconfig 服务名 --list
- chkconfig --level 5 服务名 on/off

### 9、动态监控进程

top与ps命令很相似，它们都用来显示正在执行的进程。Top与ps最大的不同之处，在于top在执行一段时间可以更新正在运行的进程。

基本语法：

- top [选项]

![top指令选项说明](D:\JavaHub\学习相关\Java笔记\pictures\top指令选项说明.png)



![top指令交互操作说明](D:\JavaHub\学习相关\Java笔记\pictures\top指令交互操作说明.png)

### 10、监控网络状态

查看系统网络情况netstat

基本语法：

- netstat [选项]

选项说明：

- -an 按一定顺序排列输出
- -p 显示哪个进程在调用