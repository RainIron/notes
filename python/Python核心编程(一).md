# Python核心编程(一)

Python核心编程通用主题部分学习记录。

[TOC]

## 1. 正则表达式

正则表达式为高级的文本模式匹配、抽取、与/或文本形式的搜索和替换功能提供了基础。简单地说，正则表达式（简称为regex）是一些由字符和特殊符号组成的字符串，它们描述了模式的重复或者表述多个字符，于是正则表达式能按照某种模式匹配一系列有相似特征的字符串。

Python通过标准库中的re模块来支持正则表达式。

### 1.1  特殊符号与字符

<b>常见正则表达式符号</b>

| 表示法(符号)   | 描述                                                       | 正则表达式示例 |
| -------------- | ---------------------------------------------------------- | -------------- |
| literal        | 匹配文本字符串的字面值literal                              | foo            |
| re1\|re2       | 匹配正则表达式re1或者re2                                   | foo\|bar       |
| .              | 匹配任何字符（除了\n之外）                                 | b.b            |
| ^              | 匹配字符起始部分                                           | ^Dear          |
| $              | 匹配字符终止部分                                           | /bin/*sh$      |
| *              | 匹配0次或者多次前面出现的正则表达式                        | [A-Za-z0-9]*   |
| +              | 匹配1次或者多次前面出现的正则表达式                        | [a-z]+\.com    |
| ?              | 匹配0次或者1次前面出现的正则表达式                         | goo?           |
| {N}            | 匹配N次前面出现的正则表达式                                | [0-9]{3}       |
| {M,N}          | 匹配M~N次前面出现的正则表达式                              | [0-9]{5,9}     |
| []             | 匹配来自字符集的任意单一字符                               | [aciou]        |
| [x-y]          | 匹配x~y范围中的任意单一字符                                | [0-9], [a-z]   |
| [^...]         | 不匹配此字符集中出现的任何一个字符，包括某一范围的字符     | [^aeiou]       |
| (*\|+\|?\|{})? | 用于匹配上面频繁出现/重复出现符号的非贪婪版本(*, +, ?, {}) | _*?[a-z]       |
| ()             | 匹配封闭的正则表达式，然后另存为子组                       | ([0-9]{3})?    |

<b>常见正则表达式特殊字符</b>

| 表示法(特殊字符) | 描述                                                         | 正则表达式示例 |
| ---------------- | ------------------------------------------------------------ | -------------- |
| \d               | 匹配任何十进制数字，与[0-9]一致(\D和\d相反，不匹配任何非数值型的数字) | data\d+.txt    |
| \w               | 匹配任何字母数字字符，与[A-Za-z0-9]相同(\w与之相反)          | [A-Za-z_]\w+   |
| \s               | 匹配任何空格字符，与[\n\t\v\f]相同(\S与之相反)               | of\sthe        |
| \b               | 匹配任何单词边界(\B与之相反)                                 | \bThe\b        |
| \N               | 匹配已保存的子组N                                            | price:\16      |
| \c               | 逐字匹配任何特殊字符c                                        | \\*            |

<b>常见正则表达式Python扩展</b>

| 表示法           | 描述                                                         | 正则表达式示例    |
| ---------------- | ------------------------------------------------------------ | ----------------- |
| (?iLmsux)        | 在正则表达式中嵌入一个或者多个特殊“标记”参数                 | (?x)              |
| (?...)           | 表示一个匹配不用保存的分组                                   | (?:\w+\\.)*       |
| (?P<name>...)    | 像一个仅由name标识而不是数字ID标识的正则分组匹配             | (?P<data>)        |
| (?P=name)        | 在同一字符串中匹配由(?P<name)分组的之前文本                  | (?P=data)         |
| (?#...)          | 表示注释，所有内容被忽略                                     | (?#comment)       |
| (?=...)          | 匹配条件是如果...出现之后的位置，而不使用输入字符串：称作正向前视断言 | (?=.com)          |
| (?!...)          | 匹配条件是如果...不出现之后的位置，而不使用输入字符串：称作正向后视断言 | (?!.net)          |
| (?<=...)         | 匹配条件是如果...出现之前的位置，而不使用输入字符串：称作正向后视断言 | (?<=800-)         |
| (?<!...)         | 匹配条件是如果...不出现之后的位置，而不使用输入字符串：称作负向后视断言 | (?<!192\\.168\\.) |
| (?(id/name)Y\|N) | 如果分组所提供的id或者name存在，就返回正则表达式的条件匹配Y, 如果不存在，就返回N; \|N是可选项 |                   |

<b>扩展表示法释义</b>

| 正则表达式模式    | 匹配的字符串                                                 |
| ----------------- | ------------------------------------------------------------ |
| (?:\w+\.)*        | 以句点作为结尾的字符串，例如"google.", "twitter.", "facebook."，但是这些匹配不会保存下来供后续的使用和数据检索 |
| (?#comment)       | 此处并不做匹配，只是作为注释                                 |
| (?=.com)          | 如果一个字符串后面跟着".com"才做匹配操作，并不使用任何目标字符串 |
| (?!.net)          | 如果一个字符串后面不是跟着".net"才做匹配操作                 |
| (?<=800-)         | 如果字符串之前为"800-"才做匹配，假定为电话号码，同样，并不使用任何输入字符串 |
| (?<!192\\.168\\.) | 如果一个字符串之前不是"192.168."才做匹配操作，假定用于过滤掉一组C类IP地址 |
| (?(L)y\|x)        | 如果一个匹配组L存在，就与y匹配；否则，就与x匹配              |

练习：大写字母 小写字母 数字 特殊字符（四种里至少三种）

第一种方案：

```
pattern = r'((^(?=.*[a-z])(?=.*[A-Z])(?=.*\W)[0-9a-zA-Z\w]{8,16}$)|(^(?=.*\d)(?=.*[a-z])(?=.*[A-Z])[0-9A-Za-z\W]{8,16}$)|(^(?=.*\d)(?=.*[a-z])(?=.*\W)[0-9A-Za-z\W]{8,16}$))|(^(?=.*\d)(?=.*\W)(?=.*[A-Z])[0-9A-Za-z\W]{8,16}$)'
```

第二种方案：

```
pattern = r'^(?![a-zA-Z]+$)(?![A-Z0-9]+$)(?![A-Z\W]+$)(?![a-z0-9]+$)(?![a-z\W]+$)(?![0-9\W]+$)[a-zA-Z0-9\W]{8,16}$'
```

### 1.2  正则表达式与Python语言

#### re模块

<b>常见的正则表达式属性</b>

| 函数/方法                           | 描述                                                         |
| ----------------------------------- | ------------------------------------------------------------ |
| compile(pattern, flags=0)           | 使用任何可选的标记来编译正则表达式的模式，然后返回一个正则表达式对象 |
| match(pattern, string, flags=0)     | 尝试使用带有可选的标记的正则表达式的模式来匹配字符串，如果匹配成功，就返回匹配对象；如果失败，就返回None |
| search(pattern, string, flags=0)    | 使用可选标记搜索字符串中第一次出现的正则表达式模式。如果匹配成功，则返回匹配对象；如果失败，则返回None |
| findall(pattern, string[, flags])   | 查找字符串中所有(非重复)出现的正则表达式。并返回一个匹配列表 |
| finditer(pattern, string[, flags])  | 与findall()函数相同，但返回的不是一个列表，而是一个迭代器，对于每一次匹配，迭代器都返回一个匹配对象 |
| split(pattern, string, max=0)       | 根据正则表达式的模式分隔符，split函数将字符串分割为列表，然后返回成功匹配的列表，分割最多操作max次（默认分割所有匹配成功的位置） |
| sub(pattern, repl, string, count=0) | 使用repl替换所有正则表达式的模式在字符串中出现的位置，除非定义count，否则就将替换所有出现的位置。 |
| purge()                             | 清除隐式编译的正则表达式模式                                 |

<b>常见的匹配对象方法</b>

| 方法                    | 描述                                                         |
| ----------------------- | ------------------------------------------------------------ |
| group(num=0)            | 返回整个匹配对象，或者编号为num的特定子组                    |
| groups(default=None)    | 返回一个包含所有匹配子组的元组(如果没有成功匹配，则返回一个空元组) |
| groupdict(default=None) | 返回一个包含所有匹配的命名子组的字典，所有的子组名称作为字典的键(如果没有成功匹配，则返回一个空字符串) |

<b>常用的模块属性</b>

| 属性                | 描述                                                         |
| ------------------- | ------------------------------------------------------------ |
| re.I, re.IGNORECASE | 不区分大小写匹配                                             |
| re.L, re.LOCALE     | 根据所使用的本地语言环境通过\w, \W, \b, \B, \s, \S实现匹配   |
| re.M, re.MULTILIINE | ^和$分别匹配目标字符串中行的起始和结尾，而不是严格匹配整个字符串的起始和结尾 |
| re.S, re.DOTALL     | "."(点号)通常匹配除了\n之外的所有单个字符；该标记表示"."(点号)能够匹配全部字符串 |
| re.X, re.VERBOSE    | 通过反斜线转义，否则所有空格加上#都被忽略，除非在一个字符类中或者允许注释并提高可读性 |

正则表达式——在模式匹配发生之前，正则表达式模式必须编译成正则表达式对象。由于正则表达式在执行过程中将进行多次比较操作，因此强烈建议使用预编译。而且，既然正则表达式的编译是必需的，那么使用预编译来提升执行性能无疑是明智之举。re.compile()能够提供此功能。

其实模块函数会对已编译的对象进行缓存，所以不是所有使用相同正则表达式模式的 search()和 match()都需要编译。即使这样，你也节省了缓存查询时间，并且不必对于相同的字符串反复进行函数调用。在不同的 Python 版本中，缓存中已编译过的正则表达式对象的数目可能不同，而且没有文档记录。purge()函数能够用于清除这些缓存。

#### match()

match()是将要介绍的第一个re模块函数和正则表达式对象（regex object）方法。match()函数试图从字符串的起始部分对模式进行匹配。如果匹配成功，就返回一个匹配对象；如果匹配失败，就返回 None，匹配对象的 group()方法能够用于显示那个成功的匹配。

示例如下：

```
>>> m = re.match('foo', 'foo')　 # 模式匹配字符串

>>> if m is not None:　　　　　# 如果匹配成功，就输出匹配内容

...　　 m.group()

...

'foo'
```

> 直接使用re.match('foo', 'food on the table').group()，如果匹配失败，将会抛出AttributeError异常。

#### search()

search()的工作方式与match()完全一致，不同之处在于search()会用它的字符串参数，在任意位置对给定正则表达式模式搜索第一次出现的匹配情况。如果搜索到成功的匹配，就会返回一个匹配对象；否则，返回None。

示例如下：

```
>>> m = re.search('foo', 'seafood') # 使用 search() 代替

>>> if m is not None: m.group()

...

'foo'　　　　　　　　　 # 搜索成功
```

#### findall()

findall()查询字符串中某个正则表达式模式全部的非重复出现情况。这与 search()在执行字符串搜索时类似，但与match()和search()的不同之处在于，findall()总是返回一个列表。如果 findall()没有找到匹配的部分，就返回一个空列表，但如果匹配成功，列表将包含所有成功的匹配部分（从左向右按出现顺序排列）。

```
>>> re.findall('car', 'carry the barcardi to the car')

['car', 'car', 'car']
```

#### finditer()

，这是一个与findall()函数类似但是更节省内存的变体。两者之间以及和其他变体函数之间的差异（很明显不同于返回的是一个迭代器还是列表）在于，和返回的匹配字符串相比，finditer()在匹配对象中迭代。

```
>>> [g.groups() for g in re.finditer(r'(th\w+) and (th\w+)', s, re.I)]

[('This', 'that')]
```

#### sub()

用于实现搜索和替换功能, 将某字符串中所有匹配正则表达式的部分进行某种形式的替换。用来替换的部分通常是一个字符串，但它也可能是一个函数，该函数返回一个用来替换的字符串。

```
>>> re.sub('X', 'Mr.Smith', 'attn: X\n\nDear X,\n')

'attn: Mr.Smith\012\012Dear Mr.Smith,\012'
```

#### subn()

subn()和 sub()一样，但 subn()还返回一个表示替换的总数，替换后的字符串和表示替换总数的数字一起作为一个拥有两个元素的元组返回。

```
>>> re.subn('X', 'Mr.Smith', 'attn: X\n\nDear X,\n')

('attn: Mr.Smith\012\012Dear Mr.Smith,\012', 2)
```

#### split()

re 模块和正则表达式的对象方法 split()对于相对应字符串的工作方式是类似的，但是与分割一个固定字符串相比，它们基于正则表达式的模式分隔字符串，为字符串分隔功能添加一些额外的威力。如果你不想为每次模式的出现都分割字符串，就可以通过为max参数设定一个值（非零）来指定最大分割数。

```
>>> re.split(':', 'str1:str2:str3')

['str1', 'str2', 'str3']
```

## 2. 网络编程

### 2.1  套接字：通信端点

套接字是计算机网络数据结构，它体现了上节中所描述的“通信端点”的概念。在任何类型的通信开始之前，网络应用程序必须创建套接字。可以将它们比作电话插孔，没有它将无法进行通信。

套接字最初是为同一主机上的应用程序所创建，使得主机上运行的一个程序（又名一个进程）与另一个运行的程序进行通信。这就是所谓的进程间通信（Inter Process Communication， IPC）。有两种类型的套接字：基于文件的和面向网络的。

UNIX套接字是我们所讲的套接字的第一个家族，并且拥有一个“家族名字”AF_UNIX （又名AF_LOCAL，在POSIX1.g标准中指定），它代表地址家族（address family）：UNIX。包括Python在内的大多数受欢迎的平台都使用术语地址家族及其缩写AF；其他比较旧的系统可能会将地址家族表示成域（domain）或协议家族（protocol family），并使用其缩写PF而非AF。类似地，AF_LOCAL（在2000～2001年标准化）将代替AF_UNIX。然而，考虑到后向兼容性，很多系统都同时使用二者，只是对同一个常数使用不同的别名。Python本身仍然在使用AF_UNIX。

第二种类型的套接字是基于网络的，它也有自己的家族名字AF_INET，或者地址家族：因特网。另一个地址家族AF_INET6用于第6版因特网协议（IPv6）寻址。

Python 2.5中引入了对特殊类型的Linux套接字的支持。套接字的AF_NETLINK家族（无连接[见2.3.3节]）允许使用标准的BSD套接字接口进行用户级别和内核级别代码之间的IPC。

Linux的另一种特性（Python 2.6中新增）就是支持透明的进程间通信（TIPC）协议。TIPC允许计算机集群之中的机器相互通信，而无须使用基于IP的寻址方式。

<b>Python只支持AF_UNIX、AF_NETLINK、AF_TIPC和AF_INET家族。</b>

#### 套接字地址：主机-端口对

一个网络地址由主机名和端口号对组成，而这是网络通信所需要的。有效的端口号范围为0～65535（尽管小于1024的端口号预留给了系统）。如果你正在使用POSIX兼容系统（如Linux、Mac OS X等），那么可以在/etc/services文件中找到预留端口号的列表（以及服务器/协议和套接字类型）。常见端口号查询地址：`http://www.iana.org/assignments/port-numbers`

#### 面向连接套接字和无连接套接字

<b>面向连接套接字</b>

第一种是面向连接的，这意味着在进行通信之前必须先建立一个连接，例如，使用电话系统给一个朋友打电话。这种类型的通信也称为虚拟电路或流套接字。

面向连接的通信提供序列化的、可靠的和不重复的数据交付，而没有记录边界。这基本上意味着每条消息可以拆分成多个片段，并且每一条消息片段都确保能够到达目的地，然后将它们按顺序组合在一起，最后将完整消息传递给正在等待的应用程序。

实现这种连接类型的主要协议是传输控制协议（更为人熟知的是它的缩写 TCP）。为 了创建 TCP 套接字，必须使用 SOCK_STREAM 作为套接字类型。TCP 套接字的名字SOCK_STREAM 基于流套接字的其中一种表示。因为这些套接字（AF_INET）的网络版本使用因特网协议（IP）来搜寻网络中的主机，所以整个系统通常结合这两种协议（TCP和IP）来进行（当然，也可以使用TCP和本地[非网络的AF_LOCAL/AF_UNIX]套接字，但是很明显此时并没有使用IP）。

<b>无连接的套接字</b>

与虚拟电路形成鲜明对比的是数据报类型的套接字，它是一种无连接的套接字。这意味着，在通信开始之前并不需要建立连接。此时，在数据传输过程中并无法保证它的顺序性、可靠性或重复性。实现这种连接类型的主要协议是用户数据报协议（更为人熟知的是其缩写 UDP）。为 了创建UDP套接字，必须使用SOCK_DGRAM作为套接字类型。所以这个系统也有一个更加普通的名字，即这两种协议（UDP和IP）的组合名字，或UDP/IP。

### 2.2  Python中的网络编程

#### socket()函数（socket模块）

要创建套接字，必须使用socket.socket()函数，它一般的语法如下：

socket(socket_family, socket_type, protocol=0)

其中，socket_family是AF_UNIX或AF_INET（如前所述），socket_type是SOCK_STREAM或SOCK_DGRAM（也如前所述）。protocol通常省略，默认为0。

所以，为了创建TCP/IP套接字，可以用下面的方式调用socket.socket()。

tcpSock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

同样，为了创建UDP/IP套接字，需要执行以下语句。

udpSock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

#### 套接字对象方法

<b>常见服务器套接字方法</b>

| 名称       | 描述                                                |
| ---------- | --------------------------------------------------- |
| s.bind()   | 将地址绑定到套接字上                                |
| s.listen() | 设置并启动TCP监听器                                 |
| s.accept() | 被动接受TCP客户端连接，一直等待直到连接到达（阻塞） |

<b>常见客户端套接字方法</b>

| 名称           | 描述                                                         |
| -------------- | ------------------------------------------------------------ |
| s.connect()    | 主动发起TCP服务器连接                                        |
| s.connect_ex() | connect()的扩展版本，此时会以错误码的形式返回问题，而不是抛出一个异常 |

<b>常见普通套接字方法</b>

| 名称              | 描述                                                 |
| ----------------- | ---------------------------------------------------- |
| s.recv()          | 接收TCP消息                                          |
| s.recv_into()     | 接收TCP消息到指定缓冲区                              |
| s.send()          | 发送TCP消息                                          |
| s.sendall()       | 完整地发送TCP消息                                    |
| s.recvfrom()      | 接收UDP消息                                          |
| s.recvfrom_into() | 接收UDP消息到指定的缓冲区                            |
| s.sendto()        | 发送UDP消息                                          |
| s.getpeemame()    | 连接到套接字（TCP）的远程地址                        |
| s.getsockname()   | 当前套接字的地址                                     |
| s.getsockopt()    | 返回给定套接字选项的值                               |
| s.setsockopt()    | 设置给定套接字选项的值                               |
| s.shutdown()      | 关闭连接                                             |
| s.close()         | 关闭套接字                                           |
| s.detach()        | 在未关闭文件描述符的情况下关闭套接字，返回文件描述符 |
| s.ioctl()         | 控制套接字的模式（仅支持Windows）                    |

<b>常见面向阻塞的套接字方法</b>

| 方法            | 描述                         |
| --------------- | ---------------------------- |
| s.setblocking() | 设置套接字的阻塞或非阻塞模式 |
| s.settimeout()  | 设置阻塞套接字操作的超时时间 |
| s.gettimeout()  | 获取阻塞套接字操作的超时时间 |

<b>常见面向文件的套接字方法</b>

| 方法         | 描述                       |
| ------------ | -------------------------- |
| s.fileno()   | 套接字文件描述符           |
| s.makefile() | 创建与套接字关联的文件对象 |

<b>常见数据属性</b>

| 属性     | 描述       |
| -------- | ---------- |
| s.family | 套接字家族 |
| s.type   | 套接字类型 |
| s.proto  | 套接字协议 |

### 2.3  Python实现简单的TCP服务器客户端

<b>服务端实现</b>

```
"""
TCP时间戳服务器
"""
from socket import *
from time import ctime


HOST = ''
PORT = 8080
BUFSIZE = 1024
ADDR = (HOST, PORT)

tcpSerSock = socket(AF_INET, SOCK_STREAM)
tcpSerSock.bind(ADDR)
tcpSerSock.listen(5)

while True:
    print("waiting for connection...")
    tcpCliSock, addr = tcpSerSock.accept()
    print(f'...connection from: {addr}')

    while True:
        data = tcpCliSock.recv(BUFSIZE)
        if not data:
            break
        ret_data = bytes(f'{ctime()} {data}', encoding='utf-8')
        tcpCliSock.send(ret_data)

    tcpCliSock.close()

tcpSerSock.close()
```

<b>客户端实现</b>

```
"""
TCP时间戳客户端
"""
from socket import *


HOST = '127.0.0.1'
PORT = 8080
BUFSIZE = 1024
ADDR = (HOST, PORT)

tcpCliSock = socket(AF_INET, SOCK_STREAM)
tcpCliSock.connect(ADDR)

while True:
    data = input('> ')
    if not data:
        break
    tcpCliSock.send(bytes(data, encoding='utf-8'))
    data = tcpCliSock.recv(BUFSIZE)
    if not data:
        break
    print(data.decode('utf-8'))

tcpCliSock.close()
```

### 2.4  Python实现简单的UDP服务器客户端

<b>服务端实现</b>

```
"""
UDP时间戳服务器
"""
from socket import *
from time import ctime


HOST = ''
PORT = 21567
BUFSIZE = 1024
ADDR = (HOST, PORT)

udpSerSock = socket(AF_INET, SOCK_DGRAM)
udpSerSock.bind(ADDR)

while True:
    print("waiting for message...")
    data, addr = udpSerSock.recvfrom(BUFSIZE)
    ret_data = bytes(f'{ctime()} {data}', encoding='utf-8')
    udpSerSock.sendto(ret_data, addr)

    print(f'...received from and returned to: {addr}')

udpSerSock.close()
```

<b>客户端实现</b>

```
"""
UDP时间戳客户端
"""
from socket import *


HOST = '127.0.0.1'
PORT = 21567
BUFSIZE = 1024
ADDR = (HOST, PORT)

udpCliSock = socket(AF_INET, SOCK_DGRAM)

while True:
    data = input('> ')
    if not data:
        break
    udpCliSock.sendto(bytes(data, encoding='utf-8'), ADDR)
    data, ADDR = udpCliSock.recvfrom(BUFSIZE)
    if not data:
        break
    print(data.decode('utf-8'))

updCliSock.close()
```

### 2.5  socketserver模块

socketserver是标准库中的一个高级模块，它的目标是简化很多样板代码，它们是创建网络客户端和服务器所必需的代码。

<b>socketserver模块类</b>

| 类                                          | 描述                                                         |
| ------------------------------------------- | ------------------------------------------------------------ |
| BaseServer                                  | 包含核心服务器和mix-in类的钩子；仅用于推导，这样不会创建这个类的实例；可以用TCPServer或UDPServer创建类的实例 |
| TCPServer/UDPServer                         | 基础的网络同步TCP/UDP服务器                                  |
| UnixStreamServer/UnixDatagramServer         | 基于文件的基础同步TCP/UDP服务器                              |
| ForkingMixIn/ThreadingMixIn                 | 核心派出或线程功能；只能作mix-in类与一个服务器类配合实现一些异步性；不能直接实例化这个类 |
| ForkingTCPServer/ForkingUDPServer           | ForkingMixinIn和TCPServer/UDPServer的组合                    |
| ThreadingTCPServer/ThreadingUDPServer       | ThreadingMixIn和TCPServer/UDPServer的组合                    |
| BaseRequestHandler                          | 包含处理服务请求的核心功能；仅仅用于推导，这样无法创建这个类的实例；可以使用StreamRequestHandler或DatagramRequestHandler创建类的实例 |
| StreamRequestHandler/DatagramRequestHandler | 实现TCP/UDP服务器的服务处理                                  |

<b>TCP服务端实现</b>

```
"""
通过socketserver实现时间戳TCP服务器
"""
from socketserver import (TCPServer as TCP, StreamRequestHandler as SRH)
from time import ctime


HOST = ''
PORT = 8080
ADDR = (HOST, PORT)


class MyRequestHandler(SRH):

    def handler(self):
        print(1)
        print(f'...connected from: {self.client_address}')
        ret_data = bytes(f'{ctime()} {self.rfile.readline()}', encoding='utf-8')
        self.wfile.write(ret_data)


tcpServ = TCP(ADDR, MyRequestHandler)

print('waiting for connection...')
tcpServ.serve_forever()
```

>程序MyRequestHandler，作为 SocketServer中StreamRequestHandler的一个子类，并重写了它的handle()方法，该方法在基类Request中默认情况下没有任何行为。
>
>def handle(self):
>
>​	pass
>
>当接收到一个来自客户端的消息时，它就会调用handle()方法。而StreamRequestHandler类将输入和输出套接字看作类似文件的对象，因此我们将使用readline()来获取客户端消息，并利用write()将字符串发送回客户端。

<b>TCP客户端实现</b>

```
"""
通过socketserver实现TCP时间戳客户端
"""
from socket import *


HOST = '127.0.0.1'
PORT = 8080
BUFSIZE = 1024
ADDR = (HOST, PORT)

while True:
    tcpCliSock = socket(AF_INET, SOCK_STREAM)
    tcpCliSock.connect(ADDR)
    data = input('> ')
    if not data:
        break
    tcpCliSock.send(bytes(data, encoding='utf-8'))
    data = tcpCliSock.recv(BUFSIZE)
    if not data:
        break
    print(data.decode('utf-8'))
    tcpCliSock.close()
```

> SocketServer 请求处理程序的默认行为是接受连接、获取请求，然后关闭连接。由于这个原因，我们不能在应用程序整个执行过程中都保持连接，因此每次向服务器发送消息时，都需要创建一个新的套接字。

### 2.6  Twisted框架

