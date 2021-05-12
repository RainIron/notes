# Nginx基础学习(一)

Nginx源码安装与配置。摘自《深入理解Nginx》。

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

### 2.1  通用配置

简单配置示例：

```
user  nobody;
worker_processes  8;
error_log  /var/log/nginx/error.log error;
#pid           logs/nginx.pid;
events {
    use epoll;
    worker_connections  50000;
}
http {
    include       mime.types;
    default_type  application/octet-stream;
    log_format  main  '$remote_addr [$time_local] "$request" '
                      '$status $bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
    access_log  logs/access.log  main buffer=32k;
    …

｝
```

<b>块配置项</b>

块配置项由一个块配置项名和一对大括号组成。

```
events {…


}
http {
    upstream backend {
           server 127.0.0.1:8080;
    }
    gzip on;
    server {
           …


           location /webstatic {
                        gzip off;
           }
    }
}
```

> 块配置项一定会用大括号把一系列所属的配置项全包含进来，表示大括号内的配置项同时生效。所有的事件类配置都要在events块中，http、server等配置也遵循这个规定。

<b>注释</b>

加“#”注释掉这一行配置。

```
# pid        logs/nginx.pid;
```

<b>配置项单位</b>

- 当指定空间大小时，可以使用的单位包括：K或者k千字节(KiloByte，KB)和M或者m兆字节(MegaByte，MB)等。例如：

  ```
  gzip_buffers     4 8k;
  client_max_body_size 64M;
  ```

- 当指定时间时，可以使用的单位包括：ms(毫秒)，s(秒)，m(分钟)，h(小时)，d(天)，w(周，包含7天)，M(月，包含30天)，y(年，包含365天)。例如：

  ```
  expires                    10y;
  proxy_read_timeout         600;
  client_body_timeout        2m;
  ```

<b>在配置中使用变量</b>

有些模块允许在配置项中使用变量，如在日志记录部分，具体示例如下:

```
log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
```

> 其中，remote_addr是一个变量，使用它的时候前面要加上$符号。需要注意的是，这种变量只有少数模块支持，并不是通用的。

### 2.2  Nginx的基本配置

预期功能分成了以下4类：

- 用于调试、定位问题的配置项

- 正常运行的必备配置项

- 优化性能的配置项

- 事件类配置项（有些事件类配置项归纳到优化性能类，这是因为它们虽然也属于events{}块，但作用是优化性能）

<b>用于调试、定位问题的配置项</b>

- 是否以守护进程方式运行Nginx

  ```
  语法： daemon on|off;
  默认： daemon on;
  ```

- 是否以master/worker方式工作

  ```
  语法： master_process on|off;
  默认： master_process on;
  ```

  > 提供master_process配置也是为了方便跟踪调试Nginx。如果用off关闭了master_process方式，就不会fork出worker子进程来处理请求，而是用master进程自身来处理请求。

- error日志的设置

  ```
  语法： error_log /path/file level;
  默认： error_log logs/error.log error;
  ```

  > /path/file参数可以是一个具体的文件，例如，默认情况下是logs/error.log文件，最好将它放到一个磁盘空间足够大的位置；/path/file也可以是/dev/null，这样就不会输出任何日志了，这也是关闭error日志的唯一手段；/path/file也可以是stderr，这样日志会输出到标准错误文件中。
  >
  > level是日志的输出级别，取值范围是debug、info、notice、warn、error、crit、alert、emerg，从左至右级别依次增大。当设定为一个级别时，大于或等于该级别的日志都会被输出到/path/file文件中，小于该级别的日志则不会输出。

- 是否处理几个特殊的调试点

  ```
  语法： debug_points[stop|abort]
  ```

  > 这个配置项也是用来帮助用户跟踪调试Nginx的。它接受两个参数：stop和abort。

- 仅对指定的客户端输出debug级别的日志

  ```
  语法： debug_connection[IP|CIDR]
  ```

  这个配置项实际上属于事件类配置，因此，它必须放在events{...}中才有效。它的值可以是IP地址或CIDR地址，例如：

  ```
  events {
      debug_connection 10.224.66.14; 
      debug_connection 10.224.57.0/24;
  }
  ```

- 限制coredump核心转储文件的大小

  ```
  语法： worker_rlimit_core size;
  ```

  > 在Linux系统中，当进程发生错误或收到信号而终止时，系统会将进程执行时的内存内容（核心映像）写入一个文件（core文件），以作为调试之用，这就是所谓的核心转储（core dumps）。当Nginx进程出现一些非法操作（如内存越界）导致进程直接被操作系统强制结束时，会生成核心转储core文件，可以从core文件获取当时的堆栈、寄存器等信息，从而帮助我们定位问题。但这种core文件中的许多信息不一定是用户需要的，如果不加以限制，那么可能一个core文件会达到几GB，这样随便coredumps几次就会把磁盘占满，引发严重问题。

- 指定coredump文件生成目录

  ```
  语法： working_directory path;
  ```

  > worker进程的工作目录。这个配置项的唯一用途就是设置coredump文件所放置的目录，协助定位问题。因此，需确保worker进程有权限向working_directory指定的目录中写入文件。

<b>正常运行的配置项</b>

- 定义环境变量

  ```
  语法： env VAR|VAR=VALUE
  ```

  > 这个配置项可以让用户直接设置操作系统上的环境变量。

- 嵌入其他配置文件

  ```
  语法： include/path/file;
  ```

  include配置项可以将其他配置文件嵌入到当前的nginx.conf文件中，它的参数既可以是绝对路径，也可以是相对路径（相对于Nginx的配置目录，即nginx.conf所在的目录），例如：

  ```
  include mime.types;
  include vhost/*.conf;
  ```

- pid文件的路径

  ```
  语法： pid path/file;
  默认： pid logs/nginx.pid;
  ```

  >  保存master进程ID的pid文件存放路径。

- Nginx worker进程运行的用户及用户组

  ```
  语法： user username[groupname];
  默认： user nobody nobody;
  ```

  > user用于设置master进程启动后，fork出的worker进程运行在哪个用户和用户组下。当按照“user username;”设置时，用户组名与用户名相同。

- 指定Nginx worker进程可以打开的最大句柄描述符个数

  ```
  语法： worker_rlimit_nofile limit;
  ```

- 限制信号队列

  ```
  语法： worker_rlimit_sigpending limit;
  ```

  > 设置每个用户发往Nginx的信号队列的大小。也就是说，当某个用户的信号队列满了，这个用户再发送的信号量会被丢掉。

<b>优化性能的配置项</b>

- Nginx worker进程个数

  ```
  语法： worker_processes number;
  默认： worker_processes 1;
  ```

  worker进程的数量会直接影响性能。那么，用户配置多少个worker进程才好呢？这实际上与业务需求有关。

  每个worker进程都是单线程的进程，它们会调用各个模块以实现多种多样的功能。如果这些模块确认不会出现阻塞式的调用，那么，有多少CPU内核就应该配置多少个进程；反之，如果有可能出现阻塞式调用，那么需要配置稍多一些的worker进程。

  例如，如果业务方面会致使用户请求大量读取本地磁盘上的静态资源文件，而且服务器上的内存较小，以至于大部分的请求访问静态资源文件时都必须读取磁盘（磁头的寻址是缓慢的），而不是内存中的磁盘缓存，那么磁盘I/O调用可能会阻塞住worker进程少量时间，进而导致服务整体性能下降。

  多worker进程可以充分利用多核系统架构，但若worker进程的数量多于CPU内核数，那么会增大进程间切换带来的消耗（Linux是抢占式内核）。一般情况下，用户要配置与CPU内核数相等的worker进程，并且使用下面的worker_cpu_affinity配置来绑定CPU内核

- 绑定Nginx worker进程到指定的CPU内核

  ```
  语法： worker_cpu_affinity cpumask[cpumask...]
  ```

  为什么要绑定worker进程到指定的CPU内核呢？假定每一个worker进程都是非常繁忙的，如果多个worker进程都在抢同一个CPU，那么这就会出现同步问题。反之，如果每一个worker进程都独享一个CPU，就在内核的调度策略上实现了完全的并发。

  例如，如果有4颗CPU内核，就可以进行如下配置：

  ```
  worker_processes 4;
  worker_cpu_affinity 1000 0100 0010 0001;
  ```

  > worker_cpu_affinity配置仅对Linux操作系统有效。Linux操作系统使用sched_setaffinity()系统调用实现这个功能。

- SSL硬件加速

  ```
  语法： ssl_engine device；
  ```

  如果服务器上有SSL硬件加速设备，那么就可以进行配置以加快SSL协议的处理速度。用户可以使用OpenSSL提供的命令来查看是否有SSL硬件加速设备：

  ```
  openssl engine -t
  ```

- 系统调用gettimeofday的执行频率

  ```
  语法： timer_resolution t;
  ```

- Nginx worker进程优先级设置

  ```
  语法： worker_priority nice;
  默认： worker_priority 0;
  ```

  >  在Linux或其他类UNIX操作系统中，当许多进程都处于可执行状态时，将按照所有进程的优先级来决定本次内核选择哪一个进程执行。进程所分配的CPU时间片大小也与进程优先级相关，优先级越高，进程分配到的时间片也就越大（例如，在默认配置下，最小的时间片只有5ms，最大的时间片则有800ms）。这样，优先级高的进程会占有更多的系统资源。

  > 优先级由静态优先级和内核根据进程执行情况所做的动态调整（目前只有±5的调整）共同决定。nice值是进程的静态优先级，它的取值范围是–20~+19，–20是最高优先级，+19是最低优先级。因此，如果用户希望Nginx占有更多的系统资源，那么可以把nice值配置得更小一些，但不建议比内核进程的nice值（通常为–5）还要小。

<b>事件类配置项</b>

- 是否打开accept锁

  ```
  语法： accept_mutex[on|off]
  默认： accept_mutext on;
  ```

  > accept_mutex是Nginx的负载均衡锁

- lock文件的路径

  ```
  语法： lock_file path/file;
  默认： lock_file logs/nginx.lock;
  ```

  > accept锁可能需要这个lock文件，如果accept锁关闭，lock_file配置完全不生效。如果打开了accept锁，并且由于编译程序、操作系统架构等因素导致Nginx不支持原子锁，这时才会用文件锁实现accept锁，这样lock_file指定的lock文件才会生效。

- 使用accept锁后到真正建立连接之间的延迟时间

  ```
  语法： accept_mutex_delay Nms;
  默认： accept_mutex_delay 500ms;
  ```

  > 在使用accept锁后，同一时间只有一个worker进程能够取到accept锁。这个accept锁不是阻塞锁，如果取不到会立刻返回。如果有一个worker进程试图取accept锁而没有取到，它至少要等accept_mutex_delay定义的时间间隔后才能再次试图取锁。

- 批量建立新连接

  ```
  语法： multi_accept[on|off];
  默认： multi_accept off;
  ```

  > 当事件模型通知有新连接时，尽可能地对本次调度中客户端发起的所有TCP请求都建立连接。

- 选择事件模型

  ```
  语法： use[kqueue|rtsig|epoll|/dev/poll|select|poll|eventport];
  默认： Nginx会自动使用最适合的事件模型。
  ```

- 每个worker的最大连接数

  ```
  语法： worker_connections number;
  ```

  > 定义每个worker进程可以同时处理的最大连接数。

### 2.3  用HTTP核心模块配置一个静态Web服务器

Nginx为配置一个完整的静态Web服务器提供了非常多的功能，下面会把这些配置项分为以下8类进行详述：

- 虚拟主机与请求的分发
- 文件路径的定义
- 内存及磁盘资源的分配
- 网络连接的设置
- MIME类型的设置
- 对客户端请求的限制
- 文件操作的优化
- 对客户端请求的特殊处理

<b>虚拟主机与请求的分发</b>

由于IP地址的数量有限，因此经常存在多个主机域名对应着同一个IP地址的情况，这时在nginx.conf中就可以按照server_name（对应用户请求中的主机域名）并通过server块来定义虚拟主机，每个server块就是一个虚拟主机，它只处理与之相对应的主机域名请求。这样，一台服务器上的Nginx就能以不同的方式处理访问不同主机域名的HTTP请求了。

- 监听端口

  ```
  语法： listen address:port[default(deprecated in 0.8.21)|default_server|[backlog=num|rcvbuf=size|sndbuf=size|accept_filter=filter|deferred|bind|ipv6only=[on|off]|ssl]];
  默认： listen 80;
  配置块： server
  ```

  IPV4示例：

  ```
  listen 127.0.0.1:8000;
  listen 127.0.0.1;        #注意：不加端口时，默认监听80端口
  
  listen 8000;
  listen *:8000;
  listen localhost:8000;
  ```

  IPV6示例：

  ```
  listen [::]:8000; 
  listen [fe80::1];
  listen [:::a8c9:1234]:80;
  ```

  listen可用参数的意义:

  `default`：将所在的server块作为整个Web服务的默认server块。如果没有设置这个参数，那么将会以在nginx.conf中找到的第一个server块作为默认server块。为什么需要默认虚拟主机呢？当一个请求无法匹配配置文件中的所有主机域名时，就会选用默认的虚拟主机（在11.3节介绍默认主机的使用）。

  `default_server`：同上。

  `backlog=num`：表示TCP中backlog队列的大小。默认为–1，表示不予设置。在TCP建立三次握手过程中，进程还没有开始处理监听句柄，这时backlog队列将会放置这些新连接。可如果backlog队列已满，还有新的客户端试图通过三次握手建立TCP连接，这时客户端将会建立连接失败。

  `rcvbuf=size`：设置监听句柄的SO_RCVBUF参数。

  `sndbuf=size`：设置监听句柄的SO_SNDBUF参数。

  `accept_filter`：设置accept过滤器，只对FreeBSD操作系统有用。

  `deferred`：在设置该参数后，若用户发起建立连接请求，并且完成了TCP的三次握手，内核也不会为了这次的连接调度worker进程来处理，只有用户真的发送请求数据时（内核已经在网卡中收到请求数据包），内核才会唤醒worker进程处理这个连接。这个参数适用于大并发的情况下，它减轻了worker进程的负担。当请求数据来临时，worker进程才会开始处理这个连接。只有确认上面所说的应用场景符合自己的业务需求时，才可以使用deferred配置。

  `bind`：绑定当前端口/地址对，如127.0.0.1:8000。只有同时对一个端口监听多个地址时才会生效。

  `ssl`：在当前监听的端口上建立的连接必须基于SSL协议。

- 主机名称

  ```
  语法： server_name name[...];
  默认： server_name"";
  配置块： server
  ```

  在开始处理一个HTTP请求时，Nginx会取出header头中的Host，与每个server中的server_name进行匹配，以此决定到底由哪一个server块来处理这个请求。有可能一个Host与多个server块中的server_name都匹配，这时就会根据匹配优先级来选择实际处理的server块。server_name与Host的匹配优先级如下：

  1）首先选择所有字符串完全匹配的server_name，如[www.testweb.com](http://www.testweb.com/) 。

  2）其次选择通配符在前面的server_name，如*.testweb.com。

  3）再次选择通配符在后面的server_name，如[www.testweb.*](http://www.testweb.*/) 。

  4）最后选择使用正则表达式才匹配的server_name，如~^\.testweb\.com$。

- server_names_hash_bucket_size

  ```
  语法： server_names_hash_bucket_size size;
  默认： server_names_hash_bucket_size 32|64|128;
  配置块： http、server、location
  ```

  > 为了提高快速寻找到相应server name的能力，Nginx使用散列表来存储server name。server_names_hash_bucket_size设置了每个散列桶占用的内存大小。

- server_names_hash_max_size

  ```
  语法： server_names_hash_max_size size;
  默认： server_names_hash_max_size 512;
  配置块： http、server、location
  ```

  > server_names_hash_max_size会影响散列表的冲突率。server_names_hash_max_size越大，消耗的内存就越多，但散列key的冲突率则会降低，检索速度也更快。server_names_hash_max_size越小，消耗的内存就越小，但散列key的冲突率可能增高。

- 重定向主机名称的处理

  ```
  语法： server_name_in_redirect on|off;
  默认： server_name_in_redirect on;
  配置块： http、server或者location
  ```

- location

  ```
  语法： location[=|~|~*|^~|@]/uri/{...}
  配置块： server
  ```

  location会尝试根据用户请求中的URI来匹配上面的/uri表达式，如果可以匹配，就选择location{}块中的配置来处理用户请求。当然，匹配方式是多样的，下面介绍location的匹配规则：

  - `=` 表示把URI作为字符串，以便与参数中的uri做完全匹配。
  - `~` 表示匹配URI时是字母大小写敏感的。
  - `~*` 表示匹配URI时忽略字母大小写问题。
  - `^~` 表示匹配URI时只需要其前半部分与uri参数匹配即可。
  - `@` 表示仅用于Nginx服务内部请求之间的重定向，带有@的location不直接处理用户请求。

<b>文件路径的定义</b>

- 以root方式设置资源路径

  ```
  语法： root path;
  默认： root html;
  配置块： http、server、location、if
  ```

- 以alias方式设置资源路径

  ```
  语法： alias path;
  配置块： location
  ```

- 访问首页

  ```
  语法： index file...;
  默认： index index.html;
  配置块： http、server、location
  ```

  有时，访问站点时的URI是/，这时一般是返回网站的首页，而这与root和alias都不同。这里用ngx_http_index_module模块提供的index配置实现。index后可以跟多个文件参数，Nginx将会按照顺序来访问这些文件，例如：

  ```
  location / {
      root   path;
      index /index.html /html/index.php /index.php;
  }
  ```

  > 接收到请求后，Nginx首先会尝试访问path/index.php文件，如果可以访问，就直接返回文件内容结束请求，否则再试图返回path/html/index.php文件的内容，依此类推。

- 根据HTTP返回码重定向页面

  ```
  语法： error_page code[code...][=|=answer-code]uri|@named_location
  配置块： http、server、location、if
  ```

  当对于某个请求返回错误码时，如果匹配上了error_page中设置的code，则重定向到新的URI中。例如：

  ```
  error_page   404          /404.html;
  error_page   502 503 504  /50x.html;
  error_page   403          http://example.com/forbidden.html;
  error_page   404          = @fetch;
  ```

  虽然重定向了URI，但返回的HTTP错误码还是与原来的相同。用户可以通过“=”来更改返回的错误码，例如：

  ```
  error_page 404 =200 /empty.gif;
  error_page 404 =403 /forbidden.gif;
  ```

  可以不指定确切的返回错误码，而是由重定向后实际处理的真实结果来决定，这时，只要把“=”后面的错误码去掉即可，例如：

  ```
  error_page 404 = /empty.gif;
  ```

  如果不想修改URI，只是想让这样的请求重定向到另一个location中进行处理，那么可以这样设置：

  ```
  location / (
      error_page 404 @fallback;
  )
  location @fallback (
      proxy_pass http://backend;
  )
  ```

- 是否允许递归使用error_page

  ```
  语法： recursive_error_pages[on|off];
  默认： recursive_error_pages off;
  配置块： http、server、location
  ```

- try_files

  ```
  语法： try_files path1[path2]uri;
  配置块： server、location
  ```

  > try_files后要跟若干路径，如path1 path2...，而且最后必须要有uri参数，意义如下：尝试按照顺序访问每一个path，如果可以有效地读取，就直接向用户返回这个path对应的文件结束请求，否则继续向下访问。如果所有的path都找不到有效的文件，就重定向到最后的参数uri上。

<b>内存及磁盘资源的分配</b>

- HTTP包体只存储到磁盘文件中

  ```
  语法： client_body_in_file_only on|clean|off;
  默认： client_body_in_file_only off;
  配置块： http、server、location
  ```

  > 当值为非off时，用户请求中的HTTP包体一律存储到磁盘文件中，即使只有0字节也会存储为文件。当请求结束时，如果配置为on，则这个文件不会被删除（该配置一般用于调试、定位问题），但如果配置为clean，则会删除该文件。

- HTTP包体尽量写入到一个内存buffer中

  ```
  语法： client_body_in_single_buffer on|off;
  默认： client_body_in_single_buffer off;
  配置块： http、server、location
  ```

  > 用户请求中的HTTP包体一律存储到内存buffer中。当然，如果HTTP包体的大小超过了下面client_body_buffer_size设置的值，包体还是会写入到磁盘文件中。

- 存储HTTP头部的内存buffer大小

  ```
  语法： client_header_buffer_size size;
  默认： client_header_buffer_size 1k;
  配置块： http、server
  ```

  > 上面配置项定义了正常情况下Nginx接收用户请求中HTTP header部分（包括HTTP行和HTTP头部）时分配的内存buffer大小。有时，请求中的HTTP header部分可能会超过这个大小，这时large_client_header_buffers定义的buffer将会生效。

- 存储超大HTTP头部的内存buffer大小

  ```
  语法： large_client_header_buffers number size;
  默认： large_client_header_buffers 48k;
  配置块： http、server
  ```

  > large_client_header_buffers定义了Nginx接收一个超大HTTP头部请求的buffer个数和每个buffer的大小。如果HTTP请求行（如GET/index HTTP/1.1）的大小超过上面的单个buffer，则返回"Request URI too large"(414)。请求中一般会有许多header，每一个header的大小也不能超过单个buffer的大小，否则会返回"Bad request"(400)。当然，请求行和请求头部的总和也不可以超过buffer个数*buffer大小。

- 存储HTTP包体的内存buffer大小

  ```
  语法： client_body_buffer_size size;
  默认： client_body_buffer_size 8k/16k;
  配置块： http、server、location
  ```

  > 如果用户请求中含有HTTP头部Content-Length，并且其标识的长度小于定义的buffer大小，那么Nginx会自动降低本次请求所使用的内存buffer，以降低内存消耗。

- HTTP包体的临时存放目录

  ```
  语法： client_body_temp_path dir-path[level1[level2[level3]]]
  默认： client_body_temp_path client_body_temp;
  配置块： http、server、location
  ```

  上面配置项定义HTTP包体存放的临时目录。在接收HTTP包体时，如果包体的大小大于client_body_buffer_size，则会以一个递增的整数命名并存放到client_body_temp_path指定的目录中。后面跟着的level1、level2、level3，是为了防止一个目录下的文件数量太多，从而导致性能下降，因此使用了level参数，这样可以按照临时文件名最多再加三层目录。例如：

  ```
  client_body_temp_path  /opt/nginx/client_temp 1 2;
  ```

- connection_pool_size

  ```
  语法： connection_pool_size size;
  默认： connection_pool_size 256;
  配置块： http、server
  ```

  > Nginx对于每个建立成功的TCP连接会预先分配一个内存池，上面的size配置项将指定这个内存池的初始大小，用于减少内核对于小块内存的分配次数。需慎重设置，因为更大的size会使服务器消耗的内存增多，而更小的size则会引发更多的内存分配次数。

- request_pool_size

  ```
  语法： request_pool_size size;
  默认： request_pool_size 4k;
  配置块： http、server
  ```

  > Nginx开始处理HTTP请求时，将会为每个请求都分配一个内存池，size配置项将指定这个内存池的初始大小，用于减少内核对于小块内存的分配次数。TCP连接关闭时会销毁connection_pool_size指定的连接内存池，HTTP请求结束时会销毁request_pool_size指定的HTTP请求内存池，但它们的创建、销毁时间并不一致，因为一个TCP连接可能被复用于多个HTTP请求。

<b>网络连接的设置</b>

- 读取HTTP头部的超时时间

  ```
  语法： client_header_timeout time（默认单位：秒）;
  默认： client_header_timeout 60;
  配置块： http、server、location
  ```

  > 客户端与服务器建立连接后将开始接收HTTP头部，在这个过程中，如果在一个时间间隔（超时时间）内没有读取到客户端发来的字节，则认为超时，并向客户端返回408("Request timed out")响应。

- 读取HTTP包体的超时时间

  ```
  语法： client_body_timeout time（默认单位：秒）；
  默认： client_body_timeout 60;
  配置块： http、server、location
  ```

- 发送响应的超时时间

  ```
  语法： send_timeout time;
  默认： send_timeout 60;
  配置块： http、server、location
  ```

  > 这个超时时间是发送响应的超时时间，即Nginx服务器向客户端发送了数据包，但客户端一直没有去接收这个数据包。如果某个连接超过send_timeout定义的超时时间，那么Nginx将会关闭这个连接。

- reset_timeout_connection

  ```
  语法： reset_timeout_connection on|off;
  默认： reset_timeout_connection off;
  配置块： http、server、location
  ```

  > 连接超时后将通过向客户端发送RST包来直接重置连接。这个选项打开后，Nginx会在某个连接超时后，不是使用正常情形下的四次握手关闭TCP连接，而是直接向用户发送RST重置包，不再等待用户的应答，直接释放Nginx服务器上关于这个套接字使用的所有缓存（如TCP滑动窗口）。相比正常的关闭方式，它使得服务器避免产生许多处于FIN_WAIT_1、FIN_WAIT_2、TIME_WAIT状态的TCP连接。

- lingering_close

  ```
  语法： lingering_close off|on|always;
  默认： lingering_close on;
  配置块： http、server、location
  ```

  > 该配置控制Nginx关闭用户连接的方式。always表示关闭用户连接前必须无条件地处理连接上所有用户发送的数据。off表示关闭连接时完全不管连接上是否已经有准备就绪的来自用户的数据。on是中间值，一般情况下在关闭连接前都会处理连接上的用户发送的数据，除了有些情况下在业务上认定这之后的数据是不必要的。

- lingering_time

  ```
  语法： lingering_time time;
  默认： lingering_time 30s;
  配置块： http、server、location
  ```

  > lingering_close启用后，这个配置项对于上传大文件很有用。上文讲过，当用户请求的Content-Length大于max_client_body_size配置时，Nginx服务会立刻向用户发送413（Request entity too large）响应。但是，很多客户端可能不管413返回值，仍然持续不断地上传HTTP body，这时，经过了lingering_time设置的时间后，Nginx将不管用户是否仍在上传，都会把连接关闭掉。

- lingering_timeout

  ```
  语法： lingering_timeout time;
  默认： lingering_timeout 5s;
  配置块： http、server、location
  ```

  > lingering_close生效后，在关闭连接前，会检测是否有用户发送的数据到达服务器，如果超过lingering_timeout时间后还没有数据可读，就直接关闭连接；否则，必须在读取完连接缓冲区上的数据并丢弃掉后才会关闭连接。

- 对某些浏览器禁用keepalive功能

  ```
  语法： keepalive_disable[msie6|safari|none]...
  默认： keepalive_disablemsie6 safari
  配置块： http、server、location
  ```

- keepalive超时时间

  ```
  语法： keepalive_timeout time（默认单位：秒）;
  默认： keepalive_timeout 75;
  配置块： http、server、location
  ```

  > 一个keepalive连接在闲置超过一定时间后（默认的是75秒），服务器和浏览器都会去关闭这个连接。当然，keepalive_timeout配置项是用来约束Nginx服务器的，Nginx也会按照规范把这个时间传给浏览器，但每个浏览器对待keepalive的策略有可能是不同的。

- 一个keepalive长连接上允许承载的请求最大数

  ```
  语法： keepalive_requests n;
  默认： keepalive_requests 100;
  配置块： http、server、location
  ```

  > 一个keepalive连接上默认最多只能发送100个请求。

- tcp_nodelay

  ```
  语法： tcp_nodelay on|off;
  默认： tcp_nodelay on;
  配置块： http、server、location
  ```

  > 确定对keepalive连接是否使用TCP_NODELAY选项。

- tcp_nopush

  ```
  语法： tcp_nopush on|off;
  默认： tcp_nopush off;
  配置块： http、server、location
  ```

<b>MIME类型的设置</b>

- MIME type与文件扩展的映射

  ``` 
  语法： type{...};
  配置块： http、server、location
  ```

  定义MIME type到文件扩展名的映射。多个扩展名可以映射到同一个MIME type。例如：

  ```
  types {
          text/html    html;
          text/html    conf;
          image/gif    gif;
          image/jpeg   jpg;
  }
  ```

- 默认MIME type

  ```
  语法： default_type MIME-type;
  默认： default_type text/plain;
  配置块： http、server、location
  ```

  > 当找不到相应的MIME type与文件扩展名之间的映射时，使用默认的MIME type作为HTTP header中的Content-Type。

- types_hash_bucket_size

  ```
  语法： types_hash_bucket_size size;
  默认： types_hash_bucket_size 32|64|128;
  配置块： http、server、location
  ```

  > 为了快速寻找到相应MIME type，Nginx使用散列表来存储MIME type与文件扩展名。types_hash_bucket_size设置了每个散列桶占用的内存大小。

- types_hash_max_size

  ```
  语法： types_hash_max_size size;
  默认： types_hash_max_size 1024;
  配置块： http、server、location
  ```

  > types_hash_max_size影响散列表的冲突率。types_hash_max_size越大，就会消耗更多的内存，但散列key的冲突率会降低，检索速度就更快。types_hash_max_size越小，消耗的内存就越小，但散列key的冲突率可能上升。

<b>对客户端请求的限制</b>

- 按HTTP方法名限制用户请求

  ```
  语法： limit_except method...{...}
  配置块： location
  ```

  Nginx通过limit_except后面指定的方法名来限制用户请求。方法名可取值包括：GET、HEAD、POST、PUT、DELETE、MKCOL、COPY、MOVE、OPTIONS、PROPFIND、PROPPATCH、LOCK、UNLOCK或者PATCH。例如：

  ```
  limit_except GET {
      allow 192.168.1.0/32;
      deny  all;
  }
  ```

- HTTP请求包体的最大值

  ```
  语法： client_max_body_size size;
  默认： client_max_body_size 1m;
  配置块： http、server、location
  ```

  > 浏览器在发送含有较大HTTP包体的请求时，其头部会有一个Content-Length字段，client_max_body_size是用来限制Content-Length所示值的大小的。因此，这个限制包体的配置非常有用处，因为不用等Nginx接收完所有的HTTP包体——这有可能消耗很长时间——就可以告诉用户请求过大不被接受。例如，用户试图上传一个10GB的文件，Nginx在收完包头后，发现Content-Length超过client_max_body_size定义的值，就直接发送413("Request Entity Too Large")响应给客户端。

- 对请求的限速

  ```
  语法： limit_rate speed;
  默认： limit_rate 0;
  配置块： http、server、location、if
  ```

  > 此配置是对客户端请求限制每秒传输的字节数。默认参数为0，表示不限速。

- limit_rate_after

  ```
  语法： limit_rate_after time;
  默认： limit_rate_after 1m;
  配置块： http、server、location、if
  ```

  > 此配置表示Nginx向客户端发送的响应长度超过limit_rate_after后才开始限速。

<b>文件操作的优化</b>

- sendfile系统调用

  ```
  语法： sendfile on|off;
  默认： sendfile off;
  配置块： http、server、location
  ```

  > 可以启用Linux上的sendfile系统调用来发送文件，它减少了内核态与用户态之间的两次内存复制，这样就会从磁盘中读取文件后直接在内核态发送到网卡设备，提高了发送文件的效率。

- AIO系统调用

  ```
  语法： aio on|off;
  默认： aio off;
  配置块： http、server、location
  ```

  > 此配置项表示是否在FreeBSD或Linux系统上启用内核级别的异步文件I/O功能。注意，它与sendfile功能是互斥的。

- directio

  ```
  语法： directio size|off;
  默认： directio off;
  配置块： http、server、location
  ```

  > 此配置项在FreeBSD和Linux系统上使用O_DIRECT选项去读取文件，缓冲区大小为size，通常对大文件的读取速度有优化作用。注意，它与sendfile功能是互斥的。

- directio_alignment

  ```
  语法： directio_alignment size;
  默认： directio_alignment 512;
  配置块： http、server、location
  ```

  > 它与directio配合使用，指定以directio方式读取文件时的对齐方式。一般情况下，512B已经足够了，但针对一些高性能文件系统，如Linux下的XFS文件系统，可能需要设置到4KB作为对齐方式。

- 打开文件缓存

  ```
  语法： open_file_cache max=N[inactive=time]|off;
  默认： open_file_cache off;
  配置块： http、server、location
  ```

  `max`：表示在内存中存储元素的最大个数。当达到最大限制数量后，将采用LRU（Least Recently Used）算法从缓存中淘汰最近最少使用的元素。

  `inactive`：表示在inactive指定的时间段内没有被访问过的元素将会被淘汰。默认时间为60秒。

  `off`：关闭缓存功能。

- 是否缓存打开文件错误的信息

  ```
  语法： open_file_cache_errors on|off;
  默认： open_file_cache_errors off;
  配置块： http、server、location
  ```

- 不被淘汰的最小访问次数

  ```
  语法： open_file_cache_min_uses number;
  默认： open_file_cache_min_uses 1;
  配置块： http、server、location
  ```

  > 它与open_file_cache中的inactive参数配合使用。如果在inactive指定的时间段内，访问次数超过了open_file_cache_min_uses指定的最小次数，那么将不会被淘汰出缓存。

- 检验缓存中元素有效性的频率

  ```
  语法： open_file_cache_valid time;
  默认： open_file_cache_valid 60s;
  配置块： http、server、location
  ```

  > 默认为每60秒检查一次缓存中的元素是否仍有效。

<b>对客户端请求的特殊处理</b>

- 忽略不合法的HTTP头部

  ```
  语法： ignore_invalid_headers on|off;
  默认： ignore_invalid_headers on;
  配置块： http、server
  ```

  > 如果将其设置为off，那么当出现不合法的HTTP头部时，Nginx会拒绝服务，并直接向用户发送400（Bad Request）错误。如果将其设置为on，则会忽略此HTTP头部。

- HTTP头部是否允许下划线

  ```
  语法： underscores_in_headers on|off;
  默认： underscores_in_headers off;
  配置块： http、server
  ```

  > 默认为off，表示HTTP头部的名称中不允许带“_”（下划线）。

- 对If-Modified-Since头部的处理策略

  ```
  语法： if_modified_since[off|exact|before];
  默认： if_modified_since exact;
  配置块： http、server、location
  ```

  相关参数说明如下:

  `off`：表示忽略用户请求中的If-Modified-Since头部。这时，如果获取一个文件，那么会正常地返回文件内容。HTTP响应码通常是200。

  `exact`：将If-Modified-Since头部包含的时间与将要返回的文件上次修改的时间做精确比较，如果没有匹配上，则返回200和文件的实际内容，如果匹配上，则表示浏览器缓存的文件内容已经是最新的了，没有必要再返回文件从而浪费时间与带宽了，这时会返回304 Not Modified，浏览器收到后会直接读取自己的本地缓存。

  `before`：是比exact更宽松的比较。只要文件的上次修改时间等于或者早于用户请求中的If-Modified-Since头部的时间，就会向客户端返回304 Not Modified。

- 文件未找到时是否记录到error日志

  ```
  语法： log_not_found on|off;
  默认： log_not_found on;
  配置块： http、server、location
  ```

  > 此配置项表示当处理用户请求且需要访问文件时，如果没有找到文件，是否将错误日志记录到error.log文件中。这仅用于定位问题。

- merge_slashes

  ```
  语法： merge_slashes on|off;
  默认： merge_slashes on;
  配置块： http、server、location
  ```

  > 此配置项表示是否合并相邻的“/”，例如，//test///a.txt，在配置为on时，会将其匹配为location/test/a.txt；如果配置为off，则不会匹配，URI将仍然是//test///a.txt。

- DNS解析地址

  ```
  语法： resolver address...;
  配置块： http、server、location
  ```

  设置DNS名字解析服务器的地址，例如：

  ```
  resolver 127.0.0.1 192.0.2.1;
  ```

- DNS解析的超时时间

  ```
  语法： resolver_timeout time;
  默认： resolver_timeout 30s;
  配置块： http、server、location
  ```

- 返回错误页面时是否在Server中注明Nginx版本

  ```
  语法： server_tokens on|off;
  默认： server_tokens on;
  配置块： http、server、location
  ```

  > 表示处理请求出错时是否在响应的Server头部中标明Nginx版本，这是为了方便定位问题。

### 2.4  反向代理服务器

反向代理（reverse proxy）方式是指用代理服务器来接受Internet上的连接请求，然后将请求转发给内部网络中的上游服务器，并将从上游服务器上得到的结果返回给Internet上请求连接的客户端，此时代理服务器对外的表现就是一个Web服务器。

<b>负载均衡的基本配置</b>

作为代理服务器，一般都需要向上游服务器的集群转发请求。这里的负载均衡是指选择一种策略，尽量把请求平均地分布到每一台上游服务器上。

- upstream块

  ```
  语法： upstream name{...}
  配置块： http
  ```

  upstream块定义了一个上游服务器的集群，便于反向代理中的proxy_pass使用。例如：

  ```
  upstream backend {
    server backend1.example.com;
    server backend2.example.com;
      server backend3.example.com;
  }
  server {
    location / {
      proxy_pass  http://backend;
    }
  }
  ```

  

- server

  ```
  语法： server name[parameters];
  配置块： upstream
  ```

  server配置项指定了一台上游服务器的名字，这个名字可以是域名、IP地址端口、UNIX句柄等，在其后还可以跟下列参数:

  `weight=number`：设置向这台上游服务器转发的权重，默认为1。

  `max_fails=number`：该选项与fail_timeout配合使用，指在fail_timeout时间段内，如果向当前的上游服务器转发失败次数超过number，则认为在当前的fail_timeout时间段内这台上游服务器不可用。max_fails默认为1，如果设置为0，则表示不检查失败次数。

  `fail_timeout=time`：fail_timeout表示该时间段内转发失败多少次后就认为上游服务器暂时不可用，用于优化反向代理功能。它与向上游服务器建立连接的超时时间、读取上游服务器的响应超时时间等完全无关。fail_timeout默认为10秒。

  `down`：表示所在的上游服务器永久下线，只在使用ip_hash配置项时才有用。

  `backup`：在使用ip_hash配置项时它是无效的。它表示所在的上游服务器只是备份服务器，只有在所有的非备份上游服务器都失效后，才会向所在的上游服务器转发请求。

  ```
  upstream  backend  {
    server   backend1.example.com    weight=5;
    server   127.0.0.1:8080          max_fails=3  fail_timeout=30s;
    server   unix:/tmp/backend3;
  }
  ```

- ip_hash

  ```
  语法： ip_hash;
  配置块： upstream
  ```

  在有些场景下，我们可能会希望来自某一个用户的请求始终落到固定的一台上游服务器中。例如，假设上游服务器会缓存一些信息，如果同一个用户的请求任意地转发到集群中的任一台上游服务器中，那么每一台上游服务器都有可能会缓存同一份信息，这既会造成资源的浪费，也会难以有效地管理缓存信息。ip_hash就是用以解决上述问题的，它首先根据客户端的IP地址计算出一个key，将key按照upstream集群里的上游服务器数量进行取模，然后以取模后的结果把请求转发到相应的上游服务器中。这样就确保了同一个客户端的请求只会转发到指定的上游服务器中。

  ip_hash与weight（权重）配置不可同时使用。如果upstream集群中有一台上游服务器暂时不可用，不能直接删除该配置，而是要down参数标识，确保转发策略的一贯性。例如：

  ```
  upstream backend {
    ip_hash;
    server   backend1.example.com;
    server   backend2.example.com;
    server   backend3.example.com  down;
    server   backend4.example.com;
  }
  ```

<b>反向代理的基本配置</b>

- proxy_pass

  ```
  语法： proxy_pass URL;
  配置块： location、if
  ```

  此配置项将当前请求反向代理到URL参数指定的服务器上，URL可以是主机名或IP地址加端口的形式，例如：

  ```
  proxy_pass http://localhost:8000/uri/;
  ```

  UNIX句柄：

  ```
  proxy_pass http://unix:/path/to/backend.socket:/uri/;
  ```

  使用upstream块:

  ```
  upstream backend {
    …
  }
  server {
    location / {
      proxy_pass  http://backend;
    }
  }
  ```

  把HTTP转换成更安全的HTTPS:

  ```
  proxy_pass https://192.168.0.1;
  ```

  默认情况下反向代理是不会转发请求中的Host头部的。如果需要转发，那么必须加上配置：

  ```
  proxy_set_header Host $host;
  ```

- proxy_method

  ```
  语法： proxy_method method;
  配置块： http、server、location
  ```

  例如：

  ```
  proxy_method POST;
  ```

- proxy_hide_header

  ```
  语法： proxy_hide_header the_header;
  配置块： http、server、location
  ```

  Nginx会将上游服务器的响应转发给客户端，但默认不会转发以下HTTP头部字段：Date、Server、X-Pad和X-Accel-*。使用proxy_hide_header后可以任意地指定哪些HTTP头部字段不能被转发。例如：

  ```
  proxy_hide_header Cache-Control;
  proxy_hide_header MicrosoftOfficeWebServer;
  ```

- proxy_pass_header

  ```
  语法： proxy_pass_header the_header;
  配置块： http、server、location
  ```

  > 与proxy_hide_header功能相反，proxy_pass_header会将原来禁止转发的header设置为允许转发。

- proxy_pass_request_body

  ```
  语法： proxy_pass_request_body on|off;
  默认： proxy_pass_request_body on;
  配置块： http、server、location
  ```

- proxy_pass_request_headers

  ```
  语法： proxy_pass_request_headers on|off;
  默认： proxy_pass_request_headers on;
  配置块： http、server、location
  ```

- proxy_redirect

  ```
  语法： proxy_redirect[default|off|redirect replacement];
  默认： proxy_redirect default;
  配置块： http、server、location
  ```

  当上游服务器返回的响应是重定向或刷新请求（如HTTP响应码是301或者302）时，proxy_redirect可以重设HTTP头部的location或refresh字段。例如:

  ```
  location /one/ {
    proxy_pass       http://upstream:port/two/;
    proxy_redirect   default;
  }
  location /one/ {
    proxy_pass       http://upstream:port/two/;
    proxy_redirect   http://upstream:port/two//one/;
  }
  ```

- proxy_next_upstream

  ```
  语法： proxy_next_upstream[error|timeout|invalid_header|http_500|http_502|http_503|http_504|http_404|off];
  默认： proxy_next_upstream error timeout;
  配置块： http、server、location
  ```

  此配置项表示当向一台上游服务器转发请求出现错误时，继续换一台上游服务器处理这个请求。proxy_next_upstream的参数如下：

  `error`：当向上游服务器发起连接、发送请求、读取响应时出错。

  `timeout`：发送请求或读取响应时发生超时。

  `invalid_header`：上游服务器发送的响应是不合法的。

  `http_500`：上游服务器返回的HTTP响应码是500。

  `http_502`：上游服务器返回的HTTP响应码是502。

  `http_503`：上游服务器返回的HTTP响应码是503。

  `http_504`：上游服务器返回的HTTP响应码是504。

  `http_404`：上游服务器返回的HTTP响应码是404。

  `off`：关闭proxy_next_upstream功能—出错就选择另一台上游服务器再次转发。

