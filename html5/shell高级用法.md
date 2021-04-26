# Shell高级用法

记录shell的高级用法。

[TOC]

## 1. 函数

### 1.1  基本的脚本函数

#### 创建函数

有两种格式可以用来在bash shell脚本中创建函数。第一种格式采用关键字`function` ，后跟分配给该代码块的函数名，格式如下：

```
function name {

    commands

}
```

> `name`属性定义了赋予函数的唯一名称。脚本中定义的每个函数都必须有一个唯一的名称。
>
> `commands`是构成函数的一条或多条bash shell命令。在调用该函数时，bash shell会按命令在函数中出现的顺序依次执行，就像在普通脚本中一样。

第二种格式更接近于其他编程语言中定义函数的方式，其格式如下：

```
name() {

commands

}
```

#### 使用函数

要在脚本中使用函数，只需要像其他shell命令一样，在行中指定函数名就行了。

示例：

```
#!/bin/bash

# using a function in a script

function func1 {

   echo "This is an example of a function"

}

count=1

while [ $count -le 5 ]

do

   func1

   count=$[ $count + 1 ]

done



echo "This is the end of the loop"

func1

echo "Now this is the end of the script"
```

> 脚本试图在`func1` 函数被定义之前使用它。由于`func1` 函数还没有定义，脚本运行函数调用处时，产生了一条错误消息。
>
> 注意，如果你重定义了函数，新定义会覆盖原来函数的定义，这一切不会产生任何错误消息。

#### 返回值

有3种不同的方法来为函数生成退出状态码。

<b>默认退出状态码</b>

默认情况下，函数的退出状态码是函数中最后一条命令返回的退出状态码。在函数执行结束后，可以用标准变量`$?` 来确定函数的退出状态码。

```
#!/bin/bash

# testing the exit status of a function


func1() {

   echo "trying to display a non-existent file"

   ls -l badfile

}

echo "testing the function: "

func1

echo "The exit status is: $?"
```

<b>使用return命令</b>

bash shell使用`return` 命令来退出函数并返回特定的退出状态码。`return` 命令允许指定一个整数值来定义函数的退出状态码，从而提供了一种简单的途径来编程设定函数退出状态码。

```
#!/bin/bash

# using the return command in a functio

function dbl {

   read -p "Enter a value: " value

   echo "doubling the value"

   return $[ $value * 2 ]

}

dbl

echo "The new value is $?"
```

> - 记住，函数一结束就取返回值；
> - 记住，退出状态码必须是0~255。

<b>使用函数输出</b>

正如可以将命令的输出保存到shell变量中一样，你也可以对函数的输出采用同样的处理办法。可以用这种技术来获得任何类型的函数输出，并将其保存到变量中：

```
#!/bin/bash

# using the echo to return a value


function dbl {

   read -p "Enter a value: " value

   echo $[ $value * 2 ]

}

result=$(dbl)

echo "The new value is $result"
```

### 1.2  在函数中使用变量

#### 函数传参

函数可以使用标准的参数环境变量来表示命令行上传给函数的参数。示例如下：

```
#!/bin/bash

# passing parameters to a function


function addem {

   if [ $# -eq 0 ] || [ $# -gt 2 ]

   then

      echo -1

   elif [ $# -eq 1 ]

   then

      echo $[ $1 + $1 ]

   else

      echo $[ $1 + $2 ]

   fi

}


echo -n "Adding 10 and 15: "

value=$(addem 10 15)

echo $value

echo -n "Let's try adding just one number: "

value=$(addem 10)

echo $value

echo -n "Now trying adding no numbers: "

value=$(addem)

echo $value

echo -n "Finally, try adding three numbers: "

value=$(addem 10 15 20)

echo $value
```

#### 函数变量

函数中定义的变量与普通变量的作用域不同。也就是说，对脚本的其他部分而言，它们是隐藏的。

函数使用两种类型的变量：

- 全局变量
- 局部变量

<b>全局变量</b>

默认情况下，你在脚本中定义的任何变量都是全局变量。在函数外定义的变量可在函数内正常访问。示例如下：

```
#!/bin/bash

# using a global variable to pass a value


function dbl {

   value=$[ $value * 2 ]

}


read -p "Enter a value: " value

dbl

echo "The new value is: $value"
```

<b>局部变量</b>

无需在函数中使用全局变量，函数内部使用的任何变量都可以被声明成局部变量。要实现这一点，只要在变量声明的前面加上`local` 关键字就可以了。

```
#!/bin/bash

# demonstrating the local keyword


function func1 {

   local temp=$[ $value + 5 ]

   result=$[ $temp * 2 ]

}

temp=4

value=6

func1

echo "The result is $result"

if [ $temp -gt $value ]

then

   echo "temp is larger"

else

   echo "temp is smaller"

fi
```

### 1.3  数组变量和函数

<b>向函数传入数组参数</b>

如果你试图将该数组变量作为函数参数，函数只会取数组变量的第一个值。

```
#!/bin/bash

# array variable to function test


function testit {

   local newarray

   newarray=(;'echo "$@"')

   echo "The new array value is: ${newarray[*]}"

}



myarray=(1 2 3 4 5)

echo "The original array is ${myarray[*]}"

testit ${myarray[*]}
```

> 该脚本用`$myarray` 变量来保存所有的数组元素，然后将它们都放在函数的命令行上。该函数随后从命令行参数中重建数组变量。在函数内部，数组仍然可以像其他数组一样使用。

<b>从函数中返回数组</b>

从函数里向shell脚本传回数组变量也用类似的方法。函数用`echo` 语句来按正确顺序输出单个数组值，然后脚本再将它们重新放进一个新的数组变量中。示例如下：

```
#!/bin/bash

# returning an array value


function arraydblr {

   local origarray

   local newarray

   local elements

   local i

   origarray=($(echo "$@"))

   newarray=($(echo "$@"))

   elements=$[ $# - 1 ]

   for (( i = 0; i <= $elements; i++ ))

   {

      newarray[$i]=$[ ${origarray[$i]} * 2 ]

   }

   echo ${newarray[*]}

}



myarray=(1 2 3 4 5)

echo "The original array is: ${myarray[*]}"

arg1=$(echo ${myarray[*]})

result=($(arraydblr $arg1))

echo "The new array is: ${result[*]}"
```

### 1.4  函数递归

局部函数变量的一个特性是**自成体系** 。除了从脚本命令行处获得的变量，自成体系的函数不需要使用任何外部资源。

这个特性使得函数可以**递归地** 调用，也就是说，函数可以调用自己来得到结果。阶乘示例如下：

```
function factorial {

   if [ $1 -eq 1 ]

   then

      echo 1

   else

      local temp=$[ $1 - 1 ]

      local result='factorial $temp'

      echo $[ $result * $1 ]

   fi

}
```

### 1.5  创建库

示例如下：

- 创建myfuncs的库文件

```
# my script functions



function addem {

   echo $[ $1 + $2 ]

}



function multem {

   echo $[ $1 * $2 ]

}
```

- 使用库文件内的函数

```
#!/bin/bash

# using functions defined in a library file

. ./myfuncs


value1=10

value2=5

result1=$(addem $value1 $value2)

result2=$(multem $value1 $value2)

echo "The result of adding them is: $result1"

echo "The result of multiplying them is: $result2"
```

> 使用函数库的关键在于`source` 命令。`source` 命令会在当前shell上下文中执行命令，而不是创建一个新shell。可以用`source` 命令来在shell脚本中运行库文件脚本。这样脚本就可以使用库中的函数了。
>
> `source` 命令有个快捷的别名，称作**点操作符** （dot operator）。要在shell脚本中运行myfuncs库文件，只需添加下面这行：
>
> ```
> . ./myfuncs
> ```
>
> 这个例子假定myfuncs库文件和shell脚本位于同一目录。如果不是，你需要使用相应路径访问该文件。

### 1.6  命令行使用函数

因为shell会解释用户输入的命令，所以可以在命令行上直接定义一个函数。有两种方法。一种方法是采用单行方式定义函数示例如下：

```
$ function divem { echo $[ $1 / $2 ];  }

$ divem 100 5

20

$
```

> 当在命令行上定义函数时，你必须记得在每个命令后面加个分号，这样shell就能知道在哪里是命令的起止了。

另一种方法是采用多行方式来定义函数。在定义时，bash shell会使用次提示符来提示输入更多命令。用这种方法，你不用在每条命令的末尾放一个分号，只要按下回车键就行, 示例如下：

```
$ function multem {

> echo $[ $1 * $2 ]

> }

$ multem 2 5

10

$
```

<b>在.bashrc文件中定义函数</b>

在命令行上直接定义shell函数的明显缺点是退出shell时，函数就消失了。对于复杂的函数来说，这可是个麻烦事。

一个非常简单的方法是将函数定义在一个特定的位置，这个位置在每次启动一个新shell的时候，都会由shell重新载入。

最佳地点就是.bashrc文件。bash shell在每次启动时都会在主目录下查找这个文件，不管是交互式shell还是从现有shell中启动的新shell。

### 1.7  实例

函数的应用绝不仅限于创建自己的函数自娱自乐。在开源世界中，共享代码才是关键，而这一点同样适用于脚本函数。你可以下载大量各式各样的函数，并将其用于自己的应用程序中。

本节介绍了如何下载、安装、使用GNU shtool shell脚本函数库。shtool库提供了一些简单的shell脚本函数，可以用来完成日常的shell功能，例如处理临时文件和目录或者格式化输出显示。

#### 下载与安装

首先是将GNU shtool库下载并安装到你的系统中，这样你才能在自己的shell脚本中使用这些库函数。要完成这项工作，可以使用FTP客户端或者图像化桌面中的浏览器。shtool软件包的下载地址是：`ftp://ftp.gnu.org/gnu/shtool/shtool-2.0.8.tar.gz`

#### 构建库

配置工作必须使用标准的`configure` 和`make` 命令，这两个命令常用于C编程环境。要构建库文件，只要输入：

```
$ ./confifgure
$ make
```

`make` 命令测试这个库文件:

```
$ make test
```

> 测试模式会测试shtool库中所有的函数。如果全部通过测试，就可以将库安装到Linux系统中的公用位置，这样所有的脚本就都能够使用这个库了。要完成安装，需要使用`make` 命令的`install` 选项。不过你得以root用户的身份运行该命令。

#### shtool库函数

| **函数**   | **描述**                                                  |
| :--------- | :-------------------------------------------------------- |
| `Arx`      | 创建归档文件（包含一些扩展功能）                          |
| `Echo`     | 显示字符串，并提供了一些扩展构件                          |
| `fixperm`  | 改变目录树中的文件权限                                    |
| `install`  | 安装脚本或文件                                            |
| `mdate`    | 显示文件或目录的修改时间                                  |
| `mkdir`    | 创建一个或更多目录                                        |
| `Mkln`     | 使用相对路径创建链接                                      |
| `mkshadow` | 创建一棵阴影树                                            |
| `move`     | 带有替换功能的文件移动                                    |
| `Path`     | 处理程序路径                                              |
| `platform` | 显示平台标识                                              |
| `Prop`     | 显示一个带有动画效果的进度条                              |
| `rotate`   | 转置日志文件                                              |
| `Scpp`     | 共享的C预处理器                                           |
| `Slo`      | 根据库的类别，分离链接器选项                              |
| `Subst`    | 使用sed的替换操作                                         |
| `Table`    | 以表格的形式显示由 **字段分隔** （field-separated）的数据 |
| `tarball`  | 从文件和目录中创建tar文件                                 |
| `version`  | 创建版本信息文件                                          |

每个shtool函数都包含大量的选项和参数，你可以利用它们改变函数的工作方式。下面是shtool函数的使用格式：

```
shtool [options] [function [options] [args]]
```

#### 使用库函数

```
#!/bin/bash

shtool platform
```

> `platform` 函数会返回Linux发行版以及系统所使用的CPU硬件的相关信息。

## 2. 初识sed和gawk

### 2.1  文本处理

#### sed编辑器

sed编辑器可以根据命令来处理数据流中的数据，这些命令要么从命令行中输入，要么存储在一个命令文本文件中。sed编辑器会执行下列操作。

(1) 一次从输入中读取一行数据。

(2) 根据所提供的编辑器命令匹配数据。

(3) 按照命令修改流中的数据。

(4) 将新的数据输出到`STDOUT` 。

`sed` 命令的格式如下：

```
sed options script file
```

`sed` 选项：

| **选项**    | **描述**                                                 |
| :---------- | :------------------------------------------------------- |
| `-e script` | 在处理输入时，将 `script` 中指定的命令添加到已有的命令中 |
| `-f file`   | 在处理输入时，将 `file` 中指定的命令添加到已有的命令中   |
| `-n`        | 不产生命令输出，使用 `print` 命令来完成输出              |

> `script` 参数指定了应用于流数据上的单个命令。如果需要用多个命令，要么使用`-e` 选项在命令行中指定，要么使用`-f` 选项在单独的文件中指定。有大量的命令可用来处理数据。

直接将数据通过管道输入sed编辑器处理，示例如下：

```
$ echo "This is a test" | sed 's/test/big test/'
This is a big test
$
```

> `s` 命令会用斜线间指定的第二个文本字符串来替换第一个文本字符串模式。

在`sed` 命令行上执行多个命令时，只要用`-e` 选项:

```
$ sed -e 's/brown/green/; s/dog/cat/' data1.txt
```

> 两个命令都作用到文件中的每行数据上。命令之间必须用分号隔开，并且在命令末尾和分号之间不能有空格。

`-f` 选项，从文件中读取编辑器命令：

```
$ cat script1.sed

s/brown/green/

s/fox/elephant/

s/dog/cat/

$ sed -f script1.sed data1.txt
```

#### **gawk程序**

提供一个类编程环境来修改和重新组织文件中的数据。提供如下功能：

- 定义变量来保存数据；
- 使用算术和字符串操作符来处理数据；
- 使用结构化编程概念（比如`if-then` 语句和循环）来为数据处理增加处理逻辑；
- 通过提取数据文件中的数据元素，将其重新排列或格式化，生成格式化报告。

gawk程序的报告生成能力通常用来从大文本文件中提取数据元素，并将它们格式化成可读的报告。

gawk的格式如下：

```
gawk options program file
```

gawk选项如下：

| **选项**       | **描述**                           |
| :------------- | :--------------------------------- |
| `-F fs`        | 指定行中划分数据字段的字段分隔符   |
| `-f file`      | 从指定的文件中读取程序             |
| `-v var=value` | 定义gawk程序中的一个变量及其默认值 |
| `-mf N`        | 指定要处理的数据文件中的最大字段数 |
| `-mr N`        | 指定数据文件中的最大数据行数       |
| `-W keyword`   | 指定gawk的兼容模式或警告等级       |

示例：

```
$ gawk '{print "Hello World!"}'
```

> - 必须将脚本命令放到两个花括号（`{}` ）中
> - 必须将脚本放到单引号中
>
> 这个程序脚本定义了一个命令：`print` 命令。这个命令名副其实：它会将文本打印到`STDOUT` 。
>
> Ctrl+D组合键会在bash中产生一个EOF字符。这个组合键能够终止该gawk程序并返回到命令行界面提示符下。

<b>数据字段变量</b>

gawk的主要特性之一是其处理文本文件中数据的能力。它会自动给一行中的每个数据元素分配一个变量。默认情况下，gawk会将如下变量分配给它在文本行中发现的数据字段：

- `$0` 代表整个文本行；
- `$1` 代表文本行中的第1个数据字段；
- `$2` 代表文本行中的第2个数据字段；
- `$n` 代表文本行中的第*n* 个数据字段。

在文本行中，每个数据字段都是通过**字段分隔符** 划分的。gawk在读取一行文本时，会用预定义的字段分隔符划分每个数据字段。gawk中默认的字段分隔符是任意的空白字符。

```
$ gawk -F: '{print $1}' /etc/passwd
```

在命令行上的程序脚本中使用多条命令，只要在命令之间放个分号:

```
$ echo "My name is Rich" | gawk '{$4="Christine"; print $0}'

My name is Christine

$
```

> 第一条命令会给字段变量`$4` 赋值。第二条命令会打印整个数据字段。注意， gawk程序在输出中已经将原文本中的第四个数据字段替换成了新值。

从文件中读取命令：

```
$ cat script2.gawk

{print $1 "'s home directory is " $6}

$ gawk -F: -f script2.gawk /etc/passwd
```

> 要一条命令放一行

<b>处理数据前运行脚本</b>

可能需要在处理数据前运行脚本，比如为报告创建标题。`BEGIN` 关键字就是用来做这个的。它会强制gawk在读取数据前执行`BEGIN` 关键字后指定的程序脚本。

```
$ gawk 'BEGIN {print "Hello World!"}'
Hello World!
$
```

<b>处理数据后运行脚本</b>

与`BEGIN` 关键字类似，`END` 关键字允许你指定一个程序脚本，gawk会在读完数据后执行它。示例如下：

```
$ gawk 'BEGIN {print "The data3 File Contents:"}

> {print $0}

> END {print "End of File"}' data3.txt
```

<b>定义脚本</b>

```
$ cat script4.gawk

BEGIN {

print "The latest list of users and shells"

print " UserID \t Shell"

print "-------- \t -------"

FS=":"

}



{

print $1 "     \t "  $7

}



END {

print "This concludes the listing"

}
```

### 2.2  sed编辑器基础

#### 替换标记

格式如下：

```
s/pattern/replacement/flags
```

> 有4种可用的替换标记：
>
> - 数字，表明新文本将替换第几处模式匹配的地方；
> - `g` ，表明新文本将会替换所有匹配的文本；
> - `p` ，表明原先行的内容要打印出来；
> - `w` `file`，将替换的结果写到文件中。

`-n` 选项将禁止sed编辑器输出。但`p` 替换标记会输出修改过的行。将二者配合使用的效果就是只输出被替换命令修改过的行:

```
$ sed -n 's/test/trial/p' data5.txt
```

#### 替换字符

有时你会在文本字符串中遇到一些不太方便在替换模式中使用的字符。Linux中一个常见的例子就是正斜线（/）。sed编辑器允许选择其他字符来作为替换命令中的字符串分隔符：

```
$ sed 's!/bin/bash!/bin/csh!' /etc/passwd
```

#### 使用地址

默认情况下，在sed编辑器中使用的命令会作用于文本数据的所有行。如果只想将命令作用于特定行或某些行，则必须用**行寻址** （line addressing）。

在sed编辑器中有两种形式的行寻址：

- 以数字形式表示行区间
- 用文本模式来过滤出行

两种形式都使用相同的格式来指定地址：

```
[address]command
```

多个命令分组：

```
address {
    command1
    command2
    command3
}
```

<b>数字方式的行寻址</b>

sed编辑器会将文本流中的第一行编号为1，然后继续按顺序为接下来的行分配行号。

在命令中指定的地址可以是单个行号，或是用起始行号、逗号以及结尾行号指定的一定区间范围内的行。这里有个`sed` 命令作用到指定行号的例子：

```
$ sed '2s/dog/cat/' data1.txt
```

> 只修改地址指定的第二行的文本

使用行地址区间:

```
$ sed '2,3s/dog/cat/' data1.txt
```

命令作用到文本中从某行开始的所有行，可以用特殊地址——美元符:

```
$ sed '2,$s/dog/cat/' data1.txt
```

<b>命令组合</b>

如果需要在单行上执行多条命令，可以用花括号将多条命令组合在一起。示例如下：

```
$ sed '2{

> s/fox/elephant/

> s/dog/cat/

> }' data1.txt
```

#### 删除行

文本替换命令不是sed编辑器唯一的命令。如果需要删除文本流中的特定行，可以用删除命令。

删除命令`d` 名副其实，它会删除匹配指定寻址模式的所有行。

```
$ sed '3,$d' data6.txt
```

模式匹配特性也适用于删除命令:

```
$ sed '/number 1/d' data6.txt
```

#### 插入和附加文本

sed编辑器允许向数据流插入和附加文本行。两个操作的区别可能比较让人费解：

- 插入（`insert` ）命令（`i` ）会在指定行前增加一个新行；
- 附加（`append` ）命令（`a` ）会在指定行后增加一个新行。

格式如下：

```
sed '[address]command\

new line'
```

示例：

```
$ echo "Test Line 2" | sed 'i\Test Line 1'
$ echo "Test Line 2" | sed 'a\Test Line 1'
```

#### 修改行

修改（`change` ）命令允许修改数据流中整行文本的内容。它跟插入和附加命令的工作机制一样，你必须在`sed` 命令中单独指定新行，示例如下：

```
$ sed '3c\

> This is a changed line of text.' data6.txt
```

#### 打印行

使用`p` 标记和替换命令显示sed编辑器修改过的行。另外有3个命令也能用来打印数据流中的信息：

- `p` 命令用来打印文本行；
- 等号（`=` ）命令用来打印行号；
- `l` （小写的L）命令用来列出行。

`p` 打印指定行：

```
$ sed -n '2,3p' data6.txt
```

`=` 显示行号：

```
$ sed '=' data1.txt
```

`l` 列出列

```
$ sed -n 'l' data10.txt
```

#### 写入文件

`w` 命令用来向文件写入行。该命令的格式如下：

```
[address]w filename
```

示例：

```
$ sed -n '/Browncoat/w Browncoats.txt' data11.txt
```

## 3. 正则表达式

正则表达式是你所定义的**模式模板** （pattern template），Linux工具可以用它来过滤文本。Linux工具（比如sed编辑器或gawk程序）能够在处理数据时使用正则表达式对数据进行模式匹配。如果数据匹配模式，它就会被接受并进一步处理；如果数据不匹配模式，它就会被滤掉。

### 3.1  正则表达式类型

Linux中的不同应用程序可能会用不同类型的正则表达式。这其中包括编程语言（Java、Perl和Python）、Linux实用工具（比如sed编辑器、gawk程序和grep工具）以及主流应用（比如MySQL和PostgreSQL数据库服务器）。

正则表达式是通过**正则表达式引擎** （regular expression engine）实现的。正则表达式引擎是一套底层软件，负责解释正则表达式模式并使用这些模式进行文本匹配。

在Linux中，有两种流行的正则表达式引擎：

- POSIX基础正则表达式（basic regular expression，BRE）引擎
- POSIX扩展正则表达式（extended regular expression，ERE）引擎

### 3.2  BRE模式

#### 特殊字符

```
.*[]^${}\+?|()
```

如果要用某个特殊字符作为文本字符，就必须**转义** 。在转义特殊字符时，你需要在它前面加一个特殊字符来告诉正则表达式引擎应该将接下来的字符当作普通的文本字符。这个特殊字符就是反斜线（\）。

示例：

```
$ echo "3 / 2" | sed -n '/\//p'
```

#### 锚字符

<b>锁定行首</b>

脱字符（`^` ）定义从数据流中文本行的行首开始的模式。如果模式出现在行首之外的位置，正则表达式模式则无法匹配。

```
$ echo "The book store" | sed -n '/^book/p'
```

<b>锁定行尾</b>

特殊字符美元符（`$` ）定义了行尾锚点。将这个特殊字符放在文本模式之后来指明数据行必须以该文本模式结尾。

```
$ echo "This is a good book" | sed -n '/book$/p'
```

<b>组合锚点</b>

```
$ sed -n '/^this is a test$/p' data4
```

#### 点号字符

特殊字符点号用来匹配除换行符之外的任意单个字符。它必须匹配一个字符，如果在点号字符的位置没有字符，那么模式就不成立。

```
$ sed -n '/.at/p' data6
```

#### 字符组

使用方括号来定义一个字符组。方括号中包含所有你希望出现在该字符组中的字符。然后你可以在模式中使用整个组，就跟使用其他通配符一样。

```
$ sed -n '/[ch]at/p' data6
```

#### 排除型字符组

在正则表达式模式中，也可以反转字符组的作用。可以寻找组中没有的字符，而不是去寻找组中含有的字符。要这么做的话，只要在字符组的开头加个脱字符。

```
$ sed -n '/[^ch]at/p' data6
```

#### 区间

可以用单破折线符号在字符组中表示字符区间。只需要指定区间的第一个字符、单破折线以及区间的最后一个字符就行了。

```
$ sed -n '/^[0-9][0-9][0-9][0-9][0-9]$/p' data8
```

#### 特殊字符组

除了定义自己的字符组外，BRE还包含了一些特殊的字符组，可用来匹配特定类型的字符。

| **组**        | **描述**                                       |
| :------------ | :--------------------------------------------- |
| `[[:alpha:]]` | 匹配任意字母字符，不管是大写还是小写           |
| `[[:alnum:]]` | 匹配任意字母数字字符0~9、A~Z或a~z              |
| `[[:blank:]]` | 匹配空格或制表符                               |
| `[[:digit:]]` | 匹配0~9之间的数字                              |
| `[[:lower:]]` | 匹配小写字母字符a~z                            |
| `[[:print:]]` | 匹配任意可打印字符                             |
| `[[:punct:]]` | 匹配标点符号                                   |
| `[[:space:]]` | 匹配任意空白字符：空格、制表符、NL、FF、VT和CR |
| `[[:upper:]]` | 匹配任意大写字母字符A~Z                        |

示例：

```
$ echo "abc" | sed -n '/[[:digit:]]/p'
```

#### 星号

在字符后面放置星号表明该字符必须在匹配模式的文本中出现0次或多次。

```
$ echo "ik" | sed -n '/ie*k/p'
```

### 3.3  扩展正则表达式

POSIX ERE模式包括了一些可供Linux应用和工具使用的额外符号。gawk程序能够识别ERE模式，但sed编辑器不能。

#### 问号

问号类似于星号，不过有点细微的不同。问号表明前面的字符可以出现0次或1次，但只限于此。它不会匹配多次出现的字符。

```
$ echo "bet" | gawk '/be?t/{print $0}'
```

#### 加号

加号是类似于星号的另一个模式符号，但跟问号也有不同。加号表明前面的字符可以出现1次或多次，但必须至少出现1次。如果该字符没有出现，那么模式就不会匹配。

```
$ echo "beeet" | gawk '/be+t/{print $0}'
```

#### 花括号

ERE中的花括号允许你为可重复的正则表达式指定一个上限。这通常称为**间隔** （interval）。可以用两种格式来指定区间。

- `m` ：正则表达式准确出现`m` 次。
- `m, n` ：正则表达式至少出现`m` 次，至多`n` 次。

这个特性可以精确调整字符或字符集在模式中具体出现的次数。

```
$ echo "bet" | gawk --re-interval '/be{1,2}t/{print $0}'
```

> 默认情况下，gawk程序不会识别正则表达式间隔。必须指定gawk程序的`--re-interval` 命令行选项才能识别正则表达式间隔。

#### 管道符号

管道符号允许你在检查数据流时，用逻辑`OR` 方式指定正则表达式引擎要用的两个或多个模式。如果任何一个模式匹配了数据流文本，文本就通过测试。如果没有模式匹配，则数据流文本匹配失败。

使用管道符号的格式如下：

```
expr1|expr2|...
```

> 正则表达式和管道符号之间不能有空格，否则它们也会被认为是正则表达式模式的一部分。

#### 表达式分组

正则表达式模式也可以用圆括号进行分组。当你将正则表达式模式分组时，该组会被视为一个标准字符。

```
$ echo "Sat" | gawk '/Sat(urday)?/{print $0}'

Sat
```

## 4.  简单的脚本使用工具

