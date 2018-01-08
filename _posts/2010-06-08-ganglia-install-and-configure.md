---
title: Ganglia安装配置手记
date: 2010-06-08 23:51
author: Ley
category: 说明文
slug: ganglia-install-and-configure
tags: ganglia, linux, 网络
---
[![Ganglia][ganglia-logo]][ganglia-link]首先，为体现本人计算机系学生的特点，特此贡献本文，作为本博好久不见的所谓技术文。

最近因为实验室要用到相关工具，学习了一下Ganglia（关于Ganglia的更多介绍可以参考[这一篇文章][]），这个比较通用的集群监控工具。在多台主机上进行安装后，现将过程中遇到的各种问题记录如下，一为方便各位（不过这真的是冷知识），二兼作笔记；

如果您对本文有兴趣，请[**点击这个链接**][]到博客进行查看，上面的代码排版为更加便于阅读；

本文面向读者对象需要对Linux和计算机网络技术有一定了解，主要是针对可能遇到的问题有一定的简述，笔记较为简单，仅作参考用，具体步骤以[官方安装指南][]为准：

0. 如果您使用的是Ubuntu
-----------------------

Ubuntu用户只需运行：

    :::sh
    sudo apt-get install ganglia-monitor
    sudo apt-get install ganglia-webfrontend

运行以上两步以后可以直接跳转本文最后一部分或者直接无视本文；而如果你的Linux其它发行版，不能直接处理依赖包，需要进行自编译安装的话，下文可能会提及你可能遇到的问题：

1. 典型依赖软件的安装
---------------------

这实际是我在配置过程中遇到问题最多的地方，因为系统环境不同，缺乏的依赖包可能不一样，这里就最有可能遇到的，部分依赖安装包的配置过程记录如下：

### a. 无法找到apr-1-config的问题

apr-1-config是apache
dev工具包中的组件，下载编译安装之则可以，以在CentOS下为例，运行：

    :::sh
    yum install httpd-devel

进行安装即可；

### b. libconfuse的安装问题

因为种种原因，如果直接编译libconfuse，可能在编译ganglia的时候提示如下的错误：

    :::console
    recompile with -fPIC
    /usr/local/lib/libconfuse.a: could not read symbols: Bad value

这就需要在编译libconfuse的时候加入特殊的参数，在网站（http://www.nongnu.org/confuse/）下载最新的版本，通过如下的参数编译安装之：

    :::sh
    ./configure CFLAGS=-fPIC --disable-nls

2. 编译安装Ganglia
------------------

生成配置的文件的代码如下：

    :::sh
    ./configure --with-librrd=/opt/rrdtool-1.4.3 --sysconfdir=/etc/ganglia --with-gmetad

其中**`/opt/rrdtool-1.4.3`**为在你机器中rrdtool的安装位置。

剩余的安装步骤参见[官方Wiki][]；

3. 安装完后的剩余配置
---------------------

主要是因为默认的rrdtool安装位置和实际系统有可能不同，以及系统权限等原因，可能导致安装完成后不能正常使用，现将在对服务进行配置的注意事项整理如下：

### a. 配置文件的填写：

以配置文件目录在**/etc/ganglia**下为例，运行以下命令生成gmond配置文件：

    :::sh
    /usr/sbin/gmond --default_config > gmond.conf

这个文件基本不用编辑即可用，当然，后面还可以对之进行一些针对性的修改。

而对gmetad.conf文件可能要做如下的修改：

运行

    :::sh
    vi /etc/ganglia/gmetad.conf

在`setuid\_username`附近，对文件添加如下的代码：

    :::sh
    setuid_username "root"
    rrd_rootdir "/opt/rrdtool-1.4.3"

不然查看服务状态，可能出现***`gmetad dead but subsys locked`***的错误；

### b. 将程序设置为启动项

在ganglia源代码目录下执行如下的代码，可以添加gmond为系统服务：

    :::sh
    cp ./gmond/gmond.init /etc/rc.d/init.d/gmond
    chkconfig --add gmond
    #chkconfig --list gmond
    #主要是用来查看服务状态，如果成功会显示
    #gmond         0:off   1:off   2:on    3:on    4:on    5:on    6:off
    /etc/rc.d/init.d/gmond start
    #成功会显示：
    #Starting GANGLIA gmond:               [  OK  ]

将以上代码中**`gmond`**更改为**`gmetad`**，执行相同的操作，可以将gmetad设置为启动项。

### c. 网页配置文件的修改

对网页文件下的**conf.php**文件进行如下的编辑修改：
    
    :::php+html
    //大致在22行附近
    // Where gmetad stores the rrd archives.
    $gmetad_root = "/opt/rrdtool-1.4.3";
    $rrds = "$gmetad_root/";
    
    // Leave this alone if rrdtool is installed in $gmetad_root,
    // otherwise, change it if it is installed elsewhere (like /usr/bin)
    define("RRDTOOL", "/opt/rrdtool-1.4.3/bin/rrdtool");

4. 其它一些相关文章推荐
-----------------------

上文提到的只是配置的一小部分，如果要完整实现整个安装配置过程，可能还要参考其它文档，现将几个比较好的文档推荐如下：

​a. 官方的wiki安装指南： [点击查看][官方安装指南]；

​b. 官方的配置指南，有相关配置参数的详细说明，可作参考：[点击查看][]；

​c. IBM
Developer网站上的一篇参考文章，涉及到原理介绍和安装配置等，比较好，**<span style="color: #ff0000;">推荐</span>**：[点击查看][1]；

​d.
这篇文章提到了较多的错误处理和较全的配置细节，不过排版不是很好：[点击查看][2]；

**如果你在安装过程遇到了问题，也欢迎<span style="color: #3366ff;">留言</span>，进行交流；**

  [ganglia-logo]: http://ganglia.sourceforge.net/logo_small.jpg "Ganglia"
  [ganglia-link]: http://ganglia.sourceforge.net/
  [这一篇文章]: http://yaoweibin2008.blog.163.com/blog/static/11031392008763256465/
  [**点击这个链接**]: http://imley.net/2010/06/08/ganglia-install-and-configure/
  [官方安装指南]: http://sourceforge.net/apps/trac/ganglia/wiki/Ganglia%203.1.x%20Installation%20and%20Configuration
  [官方Wiki]: http://sourceforge.net/apps/trac/ganglia/wiki/Ganglia%203.1.x%20Installation%20and%20Configuration#monitoring_core_installation
  [点击查看]: http://sourceforge.net/apps/trac/ganglia/wiki/Gmond%203.1.x%20General%20Configuration
  [1]: http://www.ibm.com/developerworks/cn/linux/l-ganglia-nagios-1/index.html
  [2]: http://spongebobliu.blog.sohu.com/54473377.html
