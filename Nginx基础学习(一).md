# Nginx基础学习(一)

Nginx源码安装与配置。

[TOC]

## 1. 走进Nginx

Nginx是一个跨平台的Web服务器，可运行在Linux、FreeBSD、Solaris、AIX、Mac OS、Windows等操作系统上，并且它还可以使用当前操作系统特有的一些高效API来提高自己的性能。

<b>特点</b>

- 更快

- 高扩展性

- 高可靠性

- 低内存消耗

  一般情况下，10000个非活跃的HTTP Keep-Alive连接在Nginx中仅消耗2.5M的内存，这是Nginx支持高并发连接的基础。

- 单机支持10万以上的并发连接

- 热部署

- 最自由的BSD许可协议

### 1.1  环境准备

- Linux操作系统(centos7为例)

  需要内核为Linux 2.6及以上版本的操作系统，因为Linux 2.6及以上内核才支持epoll。查看命令如下：

  ```
  # uname -a
  ```

- GCC编译器

  安装GCC命令如下：

  ```
  # yum install -y gcc
  ```

  安装G++命令如下：

  ```
  # yum install -y gcc-c++
  ```

- PCRE库

  PCRE（Perl Compatible Regular Expressions，Perl兼容正则表达式）是由Philip Hazel开发的函数库，目前为很多软件所使用，该库支持正则表达式。它由RegEx演化而来，实际上，Perl正则表达式也是源自于Henry Spencer写的RegEx。

  如果我们在配置文件nginx.conf里使用了正则表达式，那么在编译Nginx时就必须把PCRE库编译进Nginx，因为Nginx的HTTP模块要靠它来解析正则表达式。

  ```
  # yum install -y pcre pcre-devel
  ```

- zlib库

  zlib库用于对HTTP包的内容做gzip格式的压缩，如果我们在nginx.conf里配置了gzip on，并指定对于某些类型（content-type）的HTTP响应使用gzip来进行压缩以减少网络传输量，那么，在编译时就必须把zlib编译进Nginx。其yum安装方式如下：

  ```
  # yum install -y zlib zlib-devel
  ```

- OpenSSL开发库

  如果我们的服务器不只是要支持HTTP，还需要在更安全的SSL协议上传输HTTP，那么就需要拥有OpenSSL了。另外，如果我们想使用MD5、SHA1等散列函数，那么也需要安装它。其yum安装方式如下：

  ```
  # yum install -y openssl openssl-devel
  ```

- 磁盘目录

  - 设置Nginx源代码存放目录

    该目录用于放置从官网上下载的Nginx源码文件，以及第三方或我们自己所写的模块源代码文件。

  - 设置Nginx编译阶段产生的中间文件存放目录

    该目录用于放置在configure命令执行后所生成的源文件及目录，以及make命令执行后生成的目标文件和最终连接成功的二进制文件。默认情况下，configure命令会将该目录命名为objs，并放在Nginx源代码目录下。

  - 部署目录

    该目录存放实际Nginx服务运行期间所需要的二进制文件、配置文件等。默认情况下，该目录为/usr/local/nginx。

  - 日志文件存放目录

    日志文件通常会比较大，当研究Nginx的底层架构时，需要打开debug级别的日志，这个级别的日志非常详细，会导致日志文件的大小增长得极快，需要预先分配一个拥有更大磁盘空间的目录。

- Linux内核参数的优化

  针对通用的、使Nginx支持更多并发请求的TCP网络参数做简单的内核参数调整。修改`/etc/sysctl.conf` 来更改内核参数，常用配置如下：

  ```
  fs.file-max = 999999  
  net.ipv4.tcp_tw_reuse = 1
  net.ipv4.tcp_keepalive_time = 600
  net.ipv4.tcp_fin_timeout = 30
  net.ipv4.tcp_max_tw_buckets = 5000
  net.ipv4.ip_local_port_range = 1024    61000
  net.ipv4.tcp_rmem = 4096 32768 262142
  net.ipv4.tcp_wmem = 4096 32768 262142
  net.core.netdev_max_backlog = 8096
  net.core.rmem_default = 262144
  net.core.wmem_default = 262144
  net.core.rmem_max = 2097152
  net.core.wmem_max = 2097152
  net.ipv4.tcp_syncookies = 1
  net.ipv4.tcp_max_syn.backlog=1024
  ```

  修改完后执行以下命令使其生效：

  ```
  # sysctl -p
  ```

  上面参数意义解释如下：

  `.file-max` : 这个参数表示进程（比如一个worker进程）可以同时打开的最大句柄数，这个参数直接限制最大并发连接数，需根据实际情况配置。

  `.tcp_tw_reuse` : 这个参数设置为1，表示允许将TIME-WAIT状态的socket重新用于新的TCP连接，这对于服务器来说很有意义，因为服务器上总会有大量TIME-WAIT状态的连接。

  `.tcp_keepalive_time` : 这个参数表示当keepalive启用时，TCP发送keepalive消息的频度。默认是2小时，若将其设置得小一些，可以更快地清理无效的连接。

  `.tcp_fin_timeout` : 这个参数表示当服务器主动关闭连接时，socket保持在FIN-WAIT-2状态的最大时间。

  `.tcp_max_tw_buckets` : 这个参数表示操作系统允许TIME_WAIT套接字数量的最大值，如果超过这个数字，TIME_WAIT套接字将立刻被清除并打印警告信息。该参数默认为180000，过多的TIME_WAIT套接字会使Web服务器变慢。

  `.tcp_max_syn_backlog` : 这个参数表示TCP三次握手建立阶段接收SYN请求队列的最大长度，默认为1024，将其设置得大一些可以使出现Nginx繁忙来不及accept新连接的情况时，Linux不至于丢失客户端发起的连接请求。

  `.ip_local_port_range` : 这个参数定义了在UDP和TCP连接中本地（不包括连接的远端）端口的取值范围。

  `.net.ipv4.tcp_rmem` : 这个参数定义了TCP接收缓存（用于TCP接收滑动窗口）的最小值、默认值、最大值。

  `.net.ipv4.tcp_wmem` : 这个参数定义了TCP发送缓存（用于TCP发送滑动窗口）的最小值、默认值、最大值。

  `.netdev_max_backlog` : 当网卡接收数据包的速度大于内核处理的速度时，会有一个队列保存这些数据包。这个参数表示该队列的最大值。

  `.rmem_default` : 这个参数表示内核套接字接收缓存区默认的大小。

  `.wmem_default` : 这个参数表示内核套接字发送缓存区默认的大小。

  `.rmem_max` : 这个参数表示内核套接字接收缓存区的最大大小。

  `.wmem_max` : 这个参数表示内核套接字发送缓存区的最大大小。

  > 注意 滑动窗口的大小与套接字缓存区会在一定程度上影响并发连接的数目。每个TCP连接都会为维护TCP滑动窗口而消耗内存，这个窗口会根据服务器的处理速度收缩或扩张。
  >
  > 参数wmem_max的设置，需要平衡物理内存的总大小、Nginx并发处理的最大连接数量（由nginx.conf中的worker_processes和worker_connections参数决定）而确定。当然，如果仅仅为了提高并发量使服务器不出现Out Of Memory问题而去降低滑动窗口大小，那么并不合适，因为滑动窗口过小会影响大数据量的传输速度。rmem_default、wmem_default、rmem_max、wmem_max这4个参数的设置需要根据我们的业务特性以及实际的硬件成本来综合考虑。
  >
  > ·tcp_syncookies：该参数与性能无关，用于解决TCP的SYN攻击。

### 1.2  编译安装

- 源码下载

  地址： `http://nginx.org/en/download.html`

  命令行下载：

  ```
  # wget http://nginx.org/download/nginx-1.18.0.tar.gz
  ```

- 解压到nginx目录

  ```
  # tar -zxvf nginx-1.18.0.tar.gz -C nginx
  ```

- 编译安装

  ```
  # ./configure
  # make
  # make install
  ```

  > configure命令做了大量的“幕后”工作，包括检测操作系统内核和已经安装的软件，参数的解析，中间目录的生成以及根据各种参数生成一些C源码文件、Makefile文件等。
  >
  > make命令根据configure命令生成的Makefile文件编译Nginx工程，并生成目标文件、最终的二进制文件。
  >
  > make install命令根据configure执行时的参数将Nginx部署到指定的安装目录，包括相关目录的建立和二进制文件、配置文件的复制。

### 1.3  configure详解

<b>与Nginx在编译期、运行期中相关的路径参数</b>

| 参数名称                          | 意义                                                         | 默认值                                                       |
| --------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| --prefix=PATH                     | Nginx安装部署后的根目录                                      | 默认为/usr/local/nginx目录。注意：这个目标的设置会影响其他参数中的相对目录。例如，如果设置了--sbin-path=sbin/nginx，那么实际上可执行文件会被放到/usr/local/nginx/sbin/nginx |
| --sbin-path=PATH                  | 可执行文件的放置路径                                         | <prefix>/sbin/nginx                                          |
| --conf-path=PATH                  | 配置文件的放置路径                                           | <prefix>/conf/nginx.conf                                     |
| --error-log-path=PATH             | error日志文件放置路径。error日志用于定位问题，可输出多种级别的日志。它的配置非常灵活，可以在nginx.conf里配置为不同请求的日志并输出到不同的log文件中。这里是默认的Nginx核心日志路径 | <prefix>/logs/error.log                                      |
| --pid-path=PATH                   | pid文件的存放路径。这个文件里仅以ASCII码存放着Nginx master的进程ID，有了这个进程ID，在使用命令行(例如nginx -s reload)通过读取master进程发送信号时，才能对运行中的Nginx服务产生作用 | <prefix>/logs/nginx.pid                                      |
| --lock-path=PATH                  | lock文件的放置路径                                           | <prefix>/logs/nginx.lock                                     |
| --builddir=DIR                    | configure执行时与编译期间产生的临时文件放置的目录，包括产生的Makefile、C源文件、目标文件、可执行文件等 | <prefix>/objs                                                |
| --with-perl_modules_path=PATH     | perl module放置的路径。只有使用了第三方的perl module，才需要配置这个路径 | 无                                                           |
| --with-perl=PATH                  | perl binary放置的路径。如果配置的Nginx会执行perl脚本，那么就必须要设置此路径 | 无                                                           |
| --http-log-path=PATH              | access日志放置的位置。每一个HTTP请求在结束时都会记录的访问日志。 | <prefix>/logs/access.log                                     |
| --http-client-body-temp-path=PATH | 处理HTTP请求时如果请求的包需要暂时存放到临时磁盘文件中，则把这样的临时文件放置到该路径下 | <prefix>/client_body_temp                                    |
| --http-proxy-temp-path=PATH       | Nignx作为HTTP反向代理服务器时，上游服务器产生的HTTP包体在需要临时存放到磁盘文件时，这样的临时文件将放到该路径下 | <prefix>/proxy_temp                                          |
| --http-fastcgi-temp-path=PATH     | Fastcgi所使用临时文件的放置目录                              | <prefix>/fastcgi_temp                                        |
| --http-uwsgi-temp-path=PATH       | uWSGI所使用临时文件的放置目录                                | <prefix>/uwsgi_temp                                          |
| --http-scgi-temp-path=PATH        | SCGI所使用临时文件的放置目录                                 | <prefix>/scgi_temp                                           |

<b>编译Nginx时与编译器相关参数</b>

| 编译参数              | 意义                                                         |
| --------------------- | ------------------------------------------------------------ |
| --with-cc=PATH        | C编译器的路径                                                |
| --with-cpp=PATH       | C预编译器的路径                                              |
| --with-cc-opt=OPTIONS | 如果希望在Nginx编译期间指定加入一些编译选项，如指定宏或者使用-I加入某些需要包含的目录，这时可以使用该参数达成目的 |
| --with-ld-opt=OPTIONS | 最终的二进制可执行文件是由编译后生成的目标文件与一些第三方库链接生成的，在执行链接操作时可能会需要指定链接参数，--with-ld-opt就是用于加入链接时的参数。例如，如果我们希望将某个库链接到Nginx程序中，需要在这里加入--with-ld-opt=llibraryName -LlibraryPath，其中libraryName是目标库的名称，libraryPath则是目标库所在的路径 |
| --with-cpu-opt=CPU    | 指定CPU处理器架构，只能从以下取值中选择：pentium, pentiumpro, pentium3, pentium4, athlon, opteron, sparc32, sparc64, ppc64 |

 <b>configure生成的文件</b>

默认情况会在源码目录下生成objs目录，其结构如下：

```
|---ngx_auto_headers.h
|---autoconf.err
|---ngx_auto_config.h
|---ngx_modules.c
|---src
|    |---core
|    |---event
|    |    |---modules
|    |---os
|    |    |---unix
|    |    |---win32
|    |---http
|    |    |---modules
|    |    |      |---perl
|    |---mail
|    |---misc
|---Makefile
```

> 1）src目录用于存放编译时产生的目标文件。
>
> 2）Makefile文件用于编译Nginx工程以及在加入install参数后安装Nginx。
>
> 3）autoconf.err保存configure执行过程中产生的结果。
>
> 4）ngx_auto_headers.h和ngx_auto_config.h保存了一些宏，这两个头文件会被src/core/ngx_config.h及src/os/unix/ngx_linux_config.h文件（可将“linux”替换为其他UNIX操作系统）引用。
>
> 5）ngx_modules.c是一个关键文件，用来定义ngx_modules数组的。它指明了每个模块在Nginx中的优先级，当一个请求同时符合多个模块的处理规则时，将按照它们在ngx_modules数组中的顺序选择最靠前的模块优先处理。对于HTTP过滤模块而言则是相反的，因为HTTP框架在初始化时，会在ngx_modules数组中将过滤模块按先后顺序向过滤链表中添加，但每次都是添加到链表的表头，因此，对HTTP过滤模块而言，在ngx_modules数组中越是靠后的模块反而会首先处理HTTP响应

### 1.4  Nginx的命令行控制

在Linux中，需要使用命令行来控制Nginx服务器的启动与停止、重载配置文件、回滚日志文件、平滑升级等行为。默认情况下，Nginx被安装在目录/usr/local/nginx/中，其二进制文件路径为/usr/local/nginc/sbin/nginx，配置文件路径为/usr/local/nginx/conf/nginx.conf。

- 默认方式启动

  直接执行Nginx二进制程序：

  ```
  # /usr/local/nginx/sbin/nginx
  ```

  > 会读取默认路径下的配置文件：/usr/local/nginx/conf/nginx.conf。
  >
  > 实际上，在没有显式指定nginx.conf配置文件路径时，将打开在configure命令执行时使用--conf-path=PATH指定的nginx.conf文件

- 另行指定配置文件的启动方式

  使用`-c` 参数指定配置文件：

  ```
  # /usr/local/nginx/sbin/nginx -c /tmp/nginx.conf
  ```

- 另行指定安装目录的启动方式

  使用`-p` 参数指定Nginx的安装目录：

  ```
  # /usr/local/nginx/sbin/nginx -p /usr/local/nginx/
  ```

- 另行指定全局配置项的启动方式

  通过`-g` 参数临时指定一些全局配置项，以使新的配置项生效：

  ```
  # /usr/local/nginx/sbin/nginx -g "pid /var/nginx/test.pid;"
  ```

  > -g参数的约束条件是指定的配置项不能与默认路径下的nginx.conf中的配置项相冲突，否则无法启动。

  停止执行：

  ```
  # /usr/local/nginx/sbin/nginx -g "pid /var/nginx/test.pid;" -s stop
  ```

- 测试配置信息是否有错误

  ```
  # /usr/local/nginx/sbin/nginx -t
  ```

  测试配置选项时，使用 `-q` 参数可以不把error级别以下的信息输出到屏幕:

  ```
  # /usr/local/nginx/sbin/nginx -t -q
  ```

- 显示版本信息

  ```
  # /usr/local/nginx/sbin/nginx -v
  ```

- 显示编译阶段的参数

  ```
  # /usr/local/nginx/sbin/nginx -V
  ```

- 快速地停止服务

  使用-s stop可以强制停止Nginx服务。-s参数其实是告诉Nginx程序向正在运行的Nginx服务发送信号量，Nginx程序通过nginx.pid文件中得到master进程的进程ID，再向运行中的master进程发送TERM信号来快速地关闭Nginx服务。

  ```
  # /usr/local/nginx/sbin/nginx -s stop
  ```

- 优雅地停止服务

  如果希望Nginx服务可以正常地处理完当前所有请求再停止服务，那么可以使用-s quit参数来停止服务。

  ```
  # /usr/local/nginx/sbin/nginx -s quit
  ```

- 使运行中的Nginx重读配置项并生效

  使用 `-s reload` 参数可以使运行中的Nginx服务重新加载nginx.conf文件。

  ```
  # /usr/local/nginx/sbin/nginx -s reload
  ```

- 日志文件回滚

  使用 `-s reopen` 参数可以重新打开日志文件，这样可以先把当前日志文件改名或转移到其他目录中进行备份，再重新打开时就会生成新的日志文件。这个功能使得日志文件不至于过大。

  ```
  # /usr/local/nginx/sbin/nginx -s reopen
  ```

## 2. 配置Nginx





