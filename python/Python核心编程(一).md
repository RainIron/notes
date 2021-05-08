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

## 3. 因特网客户端编程

### 3.1  文件传输

因特网中最常见的事情就是传输文件。文件传输每时每刻都在发生。有很多协议可以用于在因特网上传输文件。最流行的包括文件传输协议（FTP）、UNIX 到 UNIX 复制协议（UUCP）、用于Web的超文本传输协议（HTTP）。另外，还有（UNIX下的）远程文件复制命令rcp（以及更安全、更灵活的scp和rsync）。

FTP要求输入用户名和密码才能访问远程FTP服务器，但也允许没有账号的用户匿名登录。不过管理员要先设置FTP服务器以允许匿名用户登录。这时，匿名用户的用户名是“anonymous”，密码一般是用户的电子邮件地址。与向特定的登录用户传输文件不同，这相当于公开某些目录让大家访问。

FTP看作客户端/服务器编程中的特殊情况。因为这里的客户端和服务器都使用两个套接字来通信：一个是控制和命令端口（21号端口），另一个是数据端口（有时是20号端口）。

#### Python与FTP

<b>ftplib.FTP类的方法</b>

| 方法                                        | 描述                                                         |
| ------------------------------------------- | ------------------------------------------------------------ |
| login(user='anonymous', passwd='',acct='' ) | 登录FTP服务器，所有参数都是可选的                            |
| pwd()                                       | 获得当前工作目录                                             |
| cwd(path)                                   | 把当前工作目录设置为path所示的路径                           |
| dir([path[,...[,cd]]])                      | 显示path目录里的内容，可选的参数cd是一个回调函数，会传递给retrlines()方法 |
| nlst([path[,...]])                          | 与dir()类似，但返回一个文件名列表，而不是显示这些文件名      |
| retrlines(cmd[,cd])                         | 给定FTP命令，用于下载文本文件。可选的回调函数cd用于处理文件的每一行 |
| retrbinary(cmd,cd[,bs=8192[,ra]])           | 与retrlines()类似，只是这个指令处理二进制文件。回调函数cd用于处理每一块（块大小默认为8KB）下载的数据 |
| storlines(cmd, f)                           | 给定FTP类似，用来上传文本文件。要给定一个文件对象f           |
| storbinary(cmd, f[f,bs=8192])               | 与storlines()类似，只是这个指令处理二进制文件。要给定一个文件对象f，上传块大小bs默认为8KB |
| rename(old, new)                            | 把远程文件old重命名为new                                     |
| delete(path)                                | 删除位于path的远程文件                                       |
| mkd(directory)                              | 创建远程目录                                                 |
| rmd(directory)                              | 删除远程目录                                                 |
| quit()                                      | 关闭连接并退出                                               |

交互示例：

```
>>> from ftplib import FTP

>>> f = FTP('ftp.python.org')

>>> f.login('anonymous', 'guido@python.org')

'230 Guest login ok, access restrictions apply.'

>>> f.dir()

total 38

drwxrwxr-x 10　1075　　 4127　　　　512 May 17 2000 .

drwxrwxr-x 10　1075　　 4127　　　　512 May 17 2000 ..

drwxr-xr-x 3　 root　　 wheel　　　 512 May 19 1998 bin

drwxr-sr-x 3　 root　　 1400　　　　512 Jun　9 1997 dev

drwxr-xr-x 3　 root　　 wheel　　　 512 May 19 1998 etc

lrwxrwxrwx 1　 root　　 bin　　　　　 7 Jun 29 1999 lib -> usr/lib

-r--r--r-- 1　 guido　　4127　　　　 52 Mar 24 2000 motd

drwxrwsr-x 8　 1122　　 4127　　　　512 May 17 2000 pub

drwxr-xr-x 5　 root　　 wheel　　　 512 May 19 1998 usr

>>> f.retrlines('RETR motd')

Sun Microsystems Inc.　　SunOS 5.6　　　 Generic August 1997

'226 Transfer complete.

>>> f.quit()

'221 Goodbye.'
```

### 3.2  网络新闻传输协议

用户使用网络新闻传输协议（NNTP）在新闻组中下载或发表帖子。该协议由Brain Kantor （加州大学圣地亚哥分校）和Phil Lapsley（加州大学伯克利分校）创建并记录在RFC 977中，于1986年2月公布。其后在2000年10月公布的RFC 2980中对该协议进行了更新。

作为客户端/服务器架构的另一个例子，NNTP 与 FTP 的操作方式相似，但更简单。在FTP中，登录、传输数据和控制需要使用不同的端口，而NNTP只使用一个标准端口119来通信。

#### Python和NNTP

<b>常见nntplib.NNTP类的方法</b>

| 方法                    | 描述                                                         |
| ----------------------- | ------------------------------------------------------------ |
| group(name)             | 选择一个组的名字，返回一个元组(rsp,ct,fst,lst,group)，分别表示服务器响应信息、文章数量、第一个和最后一个文章的编号、组名，所有数据都是字符串。 |
| xhdr(hdr,artrg[,ofile]) | 返回文章范围artrg(“头-尾”的格式)内文章hdr头的列表，或把数据输出到文件ofile中 |
| body(id[,ofile])        | 根据Id获取文章正文，id可以是消息的ID，也可以是文章编号，返回一个元组(rsp,anum,mid,data)，分别表示服务器响应信息、文章编号、消息ID、文章所有行的列表，或把数据输出到文件ofile中 |
| head(id)                | 与body()类似，返回相同的元组，只是返回的行列表中只包括文章标题 |
| article(id)             | 同样与body类似，返回相同的元组，只是返回的行列表中同时包括文章标题与正文 |
| stat(id)                | 让文章的指针指向id(即前面的消息ID或文章编号)。返回一个与body()相同的元组(rsp, anum, mid), 但不包含文章的数据 |
| next()                  | 用法和stat()类似，把文章指针移到下一篇文章，返回与stat()相似的元组 |
| last()                  | 用法和stat()类似，把文章指针移到最后一篇，返回与stat()相似的元组 |
| post(ufile)             | 上传ufile文件对象里的内容(使用ufile.readline())，并发布到当前新闻组中 |
| quit()                  | 关闭连接并退出                                               |

NNTP交互示例：

```
>>> from nntplib import NNTP

>>> n = NNTP('your.nntp.server')

>>> rsp, ct, fst, lst, grp = n.group('comp.lang.python')

>>> rsp, anum, mid, data = n.article('110457')

>>> for eachLine in data:

...　　 print eachLine

From: "Alex Martelli" <alex@...>

Subject: Re: Rounding Question

Date: Wed, 21 Feb 2001 17:05:36 +0100

"Remco Gerlich" <remco@...> wrote:

> Jacob Kaplan-Moss <jacob@...> wrote in comp.lang.python:

>> So I've got a number between 40 and 130 that I want to round up to

>> the nearest 10.That is:

>>

>>　　 40 --> 40, 41 --> 50, ..., 49 --> 50, 50 --> 50, 51 --> 60

> Rounding like this is the same as adding 5 to the number and then

> rounding down.Rounding down is substracting the remainder if you were

> to divide by 10, for which we use the % operator in Python.

This will work if you use +9 in each case rather than +5 (note that he

doesn't really want rounding -- he wants 41 to 'round' to 50, for ex).

Alex

>>> n.quit()

'205 closing connection - goodbye!'

>>>
```

## 4. 多线程编程

### 4.1  线程和进程

<b>进程</b>

计算机程序只是存储在磁盘上的可执行二进制（或其他类型）文件。只有把它们加载到内存中并被操作系统调用，才拥有其生命期。进程（有时称为重量级进程）则是一个执行中的程序。每个进程都拥有自己的地址空间、内存、数据栈以及其他用于跟踪执行的辅助数据。操作系统管理其上所有进程的执行，并为这些进程合理地分配时间。进程也可以通过派生（fork或spawn）新的进程来执行其他任务，不过因为每个新进程也都拥有自己的内存和数据栈等，所以只能采用进程间通信（IPC）的方式共享信息。

<b>线程</b>

线程（有时候称为轻量级进程）与进程类似，不过它们是在同一个进程下执行的，并共享相同的上下文。可以将它们认为是在一个主进程或“主线程”中并行运行的一些“迷你进程”。

线程包括开始、执行顺序和结束三部分。它有一个指令指针，用于记录当前运行的上下文。当其他线程运行时，它可以被抢占（中断）和临时挂起（也称为睡眠）——这种做法叫做让步（yielding）。

一个进程中的各个线程与主线程共享同一片数据空间，因此相比于独立的进程而言，线程间的信息共享和通信更加容易。线程一般是以并发方式执行的，正是由于这种并行和数据共享机制，使得多任务间的协作成为可能。当然，在单核CPU系统中，因为真正的并发是不可能的，所以线程的执行实际上是这样规划的：每个线程运行一小会儿，然后让步给其他线程（再次排队等待更多的CPU时间）。在整个进程的执行过程中，每个线程执行它自己特定的任务，在必要时和其他线程进行结果通信。

当然，这种共享并不是没有风险的。如果两个或多个线程访问同一片数据，由于数据访问顺序不同，可能导致结果不一致。这种情况通常称为竞态条件（race condition）。幸运的是，大多数线程库都有一些同步原语，以允许线程管理器控制执行和访问。

另一个需要注意的问题是，线程无法给予公平的执行时间。这是因为一些函数会在完成前保持阻塞状态，如果没有专门为多线程情况进行修改，会导致CPU的时间分配向这些贪婪的函数倾斜。

### 4.2  Python和线程

#### 全局解释器锁(GIL)

Python虚拟机的访问是由全局解释器锁（GIL）控制的。这个锁就是用来保证同时只能有一个线程运行的。在多线程环境中，Python虚拟机将按照下面所述的方式执行。

1.设置GIL。

2.切换进一个线程去运行。

3.执行下面操作之一。

a.指定数量的字节码指令。

b.线程主动让出控制权（可以调用time.sleep(0)来完成）。

4.把线程设置回睡眠状态（切换出线程）。

5.解锁GIL。

6.重复上述步骤。

当调用外部代码（即，任意C/C++扩展的内置函数）时，GIL会保持锁定，直至函数执行结束（因为在这期间没有Python字节码计数）。编写扩展函数的程序员有能力解锁GIL，然而，作为Python开发者，你并不需要担心Python代码会在这些情况下被锁住。

例如，对于任意面向 I/O 的 Python 例程（调用了内置的操作系统 C 代码的那种）， GIL会在I/O调用前被释放，以允许其他线程在I/O执行的时候运行。而对于那些没有太多 I/O 操作的代码而言，更倾向于在该线程整个时间片内始终占有处理器（和 GIL）。换句话说就是，I/O 密集型的 Python 程序要比计算密集型的代码能够更好地利用多线程环境。

#### `_thread` 模块

除了派生线程外，`_thread` 模块还提供了基本的同步数据结构，称为锁对象（lock object，也叫原语锁、简单锁、互斥锁、互斥和二进制信号量）。

<b>常见`_thread` 模块的函数</b>

| 函数/方法                                     | 描述                                                         |
| --------------------------------------------- | ------------------------------------------------------------ |
| start_new_thread(function, args, kwargs=None) | 派生一个新的线程，使用给定的args和可选的kwargs来执行function |
| allocate_lock()                               | 分配LockType锁对象                                           |
| exit()                                        | 给线程退出指令                                               |

<b>常见LockType锁对象的方法</b>

| 函数/方法          | 描述                                       |
| ------------------ | ------------------------------------------ |
| acquire(wait=None) | 尝试获取锁对象                             |
| locked()           | 如果获取了锁对象则返回True,否则，返回False |
| release()          | 释放锁                                     |

使用示例(无锁):

```
"""
_thread模块使用（无锁）
"""
import _thread as thread
from time import sleep, ctime


def loop0():
    print(f'start loop 0 at: {ctime()}')
    sleep(4)
    print(f'loop 0 done at: {ctime()}')


def loop1():
    print(f'start loop 1 at: {ctime()}')
    sleep(2)
    print(f'loop 1 done at: {ctime()}')


def main():
    print(f'starting at: {ctime()}')
    thread.start_new_thread(loop0, ())
    thread.start_new_thread(loop1, ())

    sleep(6)
    print(f'all DONE at: {ctime()}')


if __name__ == '__main__':
    main()
```

> start_new_thread()必须包含开始的两个参数，于是即使要执行的函数不需要参数，也需要传递一个空元组。
>
> 程序中剩下的一个主要区别是增加了一个 sleep(6)调用。为什么必须要这样做呢？这是因为如果我们没有阻止主线程继续执行，它将会继续执行下一条语句，显示“all done”然后退出，而loop0()和loop1()这两个线程将直接终止。

使用示例(有锁)：

```
"""
_thread模块使用（有锁）
"""
import _thread as thread
from time import sleep, ctime


loops = [4, 2]


def loop(nloop, nsec, lock):
    print(f'start lool {nloop} at: {ctime()}')
    sleep(nsec)
    print(f'loop {nloop} done at: {ctime()}')


def main():
    print(f'starting at: ctime()')
    locks = []
    nloops = range(len(loops))

    for i in nloops:
        lock = thread.allocate_lock()
        lock.acquire()
        locks.append(lock)
    
    for i in nloops:
        thread.start_new_thread(loop, (i, loops[i], locks[i]))
    
    for i in nloops:
        while locks[i].locked():
            pass


if __name__ == '__main__':
    main()
```

#### threading模块

避免使用thread模块的另一个原因是该模块不支持守护线程这个概念。当主线程退出时，所有子线程都将终止，不管它们是否仍在工作。如果你不希望发生这种行为，就要引入守护线程的概念了。

threading 模块支持守护线程，其工作方式是：守护线程一般是一个等待客户端请求服务的服务器。如果没有客户端请求，守护线程就是空闲的。如果把一个线程设置为守护线程，就表示这个线程是不重要的，进程退出时不需要等待这个线程执行完成。

<b>`threading` 模块的对象</b>

| 对象             | 描述                                                         |
| ---------------- | ------------------------------------------------------------ |
| Thread           | 表示一个执行线程对象                                         |
| Lock             | 锁原语对象(和`_thread` 模块中的锁一样)                       |
| RLock            | 可重入锁对象，使单一线程可以(再次)获得已持有的锁(递归锁)     |
| Condition        | 条件变量对象，使得一个线程等待另一个线程满足特定的条件，比如改变状态或某个数据值 |
| Event            | 条件变量的通用版本，任意数量的线程等待某个事件的发生，在该事件发生后所有线程将被激活 |
| Semaphore        | 为线程间共享的有限资源提供一个计数器，如果没有可用资源时会被阻塞 |
| BoundedSemaphore | 与Semaphore相似，不过它不允许超过初始值                      |
| Timer            | 与Thread相似，不过它要在运行前等待一段时间                   |
| Barrier          | 创建一个“障碍”, 必须达到指定数量的线程后才可以继续           |

<b>常见Thread对象的属性</b>

| 属性   | 描述                                 |
| ------ | ------------------------------------ |
| name   | 线程名                               |
| ident  | 线程标识符                           |
| daemon | 布尔标志，表示这个线程是否是守护线程 |

<b>常见Thread对象的方法</b>

| 方法                                                         | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `__init__(group=None, tatget=None, name=None, args=(), kwargs={}, verbose=None, daemon=None)` | 实例化一个线程对象，需要有一个可调用的target，以及其参数args或kwargs。还可以传递name或group参数，不过后者还未实现。此外，verbose标志也是可接受的。而daemon的值将会设定thread.daemon属性/标识 |
| start()                                                      | 开始执行该线程                                               |
| run()                                                        | 定义线程功能的方法                                           |
| join(timeout=None)                                           | 直至启动的线程终止之前一直挂起；除非给出了timeout(单位：秒)，否则会一直阻塞 |
| getName()                                                    | 返回线程名                                                   |
| setName(name)                                                | 设定线程名                                                   |
| isAlivel/is_alive()                                          | 布尔标志，表示这个线程是否还存活                             |
| isDaemon()                                                   | 如果是守护线程，则返回True; 否则，返回False                  |
| setDaemon(daemonic)                                          | 把线程的守护标志设定为布尔值daemonic(必须在线程start()之前调用) |

<b>使用Thread创建线程的三种方式</b>

创建Thread实例，传给它一个函数：

```
"""
通过threading模块的Thread创建线程方式一：
创建Thread实例，传给它一个函数
"""
import threading
from time import sleep, ctime


loops = [4, 2]


def loop(nloop, nsec):
    print(f'start loop {nloop} at: {ctime()}')
    sleep(nsec)
    print(f'loop {nloop} done at: {ctime()}')


def main():
    print(f'starting at: {ctime()}')
    threads = []
    nloops = range(len(loops))

    for i in nloops:
        t = threading.Thread(target=loop, args=(i, loops[i]))
        threads.append(t)
    
    for i in nloops:            # start threads
        threads[i].start()
    
    for i in nloops:            # wait for all
        threads[i].join()       # threads to finish
    
    print(f'all DONE at: {ctime()}')


if __name__ == '__main__':
    main()
```

创建Thread实例，传给它一个可调用的类实例:

```
"""
通过threading模块的Thread创建线程方式二：
创建Thread实例，传给它一个可调用的类实例
"""
import threading
from time import sleep, ctime


loops = [4, 2]


class ThreadFunc:
    def __init__(self, func, args, name=''):
        self.name = name
        self.func = func
        self.args = args
    
    def __call__(self):
        self.func(*self.args)


def loop(nloop, nsec):
    print(f'start loop {nloop} at: {ctime()}')
    sleep(nsec)
    print(f'loop {nloop} done at: {ctime()}')


def main():
    print(f'starting at: {ctime()}')
    threads = []
    nloops = range(len(loops))

    for i in nloops:
        t = threading.Thread(target=ThreadFunc(loop, (i, loops[i]), loop.__name__))
        threads.append(t)
    
    for i in nloops:            # start threads
        threads[i].start()
    
    for i in nloops:            # wait for all
        threads[i].join()       # threads to finish
    
    print(f'all DONE at: {ctime()}')


if __name__ == '__main__':
    main()
```

派生Thread的子类，并创建子类的实例：

```
"""
通过threading模块的Thread创建线程方式三：
派生Thread的子类，并创建子类的实例
"""
import threading
from time import sleep, ctime


loops = [4, 2]


class MyThread(threading.Thread):
    def __init__(self, func, args, name=''):
        threading.Thread.__init__(self)
        self.name = name
        self.func = func
        self.args = args
    
    def run(self):
        self.func(*self.args)


def loop(nloop, nsec):
    print(f'start loop {nloop} at: {ctime()}')
    sleep(nsec)
    print(f'loop {nloop} done at: {ctime()}')


def main():
    print(f'starting at: {ctime()}')
    threads = []
    nloops = range(len(loops))

    for i in nloops:
        t = MyThread(loop, (i, loops[i]), loop.__name__)
        threads.append(t)
    
    for i in nloops:            # start threads
        threads[i].start()
    
    for i in nloops:            # wait for all
        threads[i].join()       # threads to finish
    
    print(f'all DONE at: {ctime()}')


if __name__ == '__main__':
    main()
```

> 1）MyThread子类的构造函数必须先调用其基类的构造函数（第9行）；
>
> 2）之前的特殊方法__call__()在这个子类中必须要写为run()

#### 同步原语

在多线程代码中，总会有一些特定的函数或代码块不希望（或不应该）被多个线程同时执行，通常包括修改数据库、更新文件或其他会产生竞态条件的类似情况。回顾前面的部分，如果两个线程运行的顺序发生变化，就有可能造成代码的执行轨迹或行为不相同，或者产生不一致的数据，这就是需要使用同步的情况。当任意数量的线程可以访问临界区的代码，但在给定的时刻只有一个线程可以通过时，就是使用同步的时候了。程序员选择适合的同步原语，或者线程控制机制来执行同步。

## 5. 数据库编程

Python和数据库

wiki.python.org/moin/DatabaseProgramming

wiki.python.org/moin/DatabaseInterfaces

数据库格式、结构及开发模式

en.wikipedia.org/wiki/DSN

www.martinfowler.com/eaaCatalog/dataMapper.html

en.wikipedia.org/wiki/Active_record_pattern

blog.mongodb.org/post/114440717/bson

非关系数据库

en.wikipedia.org/wiki/Nosql

nosql-database.org/

www.mongodb.org/display/DOCS/MongoDB,+CouchDB,+MySQL+Compare+Grid