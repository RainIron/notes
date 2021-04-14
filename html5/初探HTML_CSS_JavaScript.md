# 初探HTML、CSS和JavaScript

此笔记记录HTML5、CSS3和JavaScript的简单使用。

[TOC]

## 1.  元素

### 1.1  元素说明

```html
I like <code>apple</code> and oranges.
```

`<code>apple</code>` 该元素由 `<code>` 开始标签，`</code> ` 结束标签，`apple` 内容三部分构成， 合称code元素。

### 1.2  常见标签

| 标签     | 作用                                 |
| -------- | ------------------------------------ |
| a        | 生成超链接                           |
| body     | 表示HTML文档的内容                   |
| button   | 生成用以提交表单的按钮               |
| code     | 表示计算机代码片段                   |
| DOCTYPE  | 表示HTML文档的开始                   |
| head     | 表示HTML文档的头部区域               |
| hr       | 表示主题的改变                       |
| html     | 表示文档的HTML部分                   |
| input    | 表示用户输入的数据                   |
| label    | 生成另一元素的说明标签               |
| p        | 表示段落                             |
| style    | 定义CSS样式                          |
| table    | 表示用表格组织的数据                 |
| td       | 表示表格单元格                       |
| textarea | 生成用于获取用户输入数据的多行文本框 |
| th       | 生成表头单元格                       |
| title    | 表示HTML文档的标题                   |
| tr       | 表示表格行                           |

### 1.3  空元素

```html
I like <code></code> apple and oranges.
```

此时code元素没有任何内容，也没有任何意义，但它仍然是有效的html代码。

### 1.4  虚元素

有些元素**只能** 使用一个标签表示，在其中放置任何内容都不符合HTML规范。这类元素称为**虚元素**，hr就是这样一个元素。他有两种表示方式， 第一种使用开始标签`<hr>` , 第二种在此基础上加上一个斜杠符号`<hr />`

```html
 I like <code>apple</code> and oranges.<br />
 I like <code></code> apple and oranges.<br />
 <hr />
```

 ### 1.5  元素属性

元素可以使用属性进行配置，但只能用在开始标签或单个标签上，不能用于结束标签。 属性分为属性和值两部分。

```
I like <a href="apple.html">apple</a> and oranges.
```

在a元素的开始标签中`href` 为属性名称，等号后面的`apple.html` 为属性值。

<b>一个元素应用多个属性</b>

```html
I like <a class="link" href="/apples.html" id="firstlink">apples</a> and oranges.
```

<b>布尔属性</b>

有些属性属于布尔属性，这种属性不需要设定一个值，只需将属性名添加到元素中即可.

```
Enter your name: <input disabled>
```

<b>自定义属性</b>

用户可自定义属性，这种属性必须以`data-` 开头。

```html
Enter your name: <input disabled="true" data-creator="adam" data-purpose="collection">
```

## 2.  HTML文档

### 2.1  外层结构

```html
<!DOCTYPE HTML>
<html>
    <!-- elements go here -->
</html>
```

上例中的`DOCTYPE` 元素让浏览器明白其处理的是HTML文档。这是用布尔属性`HTML` 表达的：

```
<!DOCTYPE HTML>
```

紧跟着`DOCTYPE` 元素的是`html` 元素的开始标签。它告诉浏览器：自此直到`html` 结束标签，所有元素内容都应作为HTML处理。

### 2.2  元数据

HTML文档的元数据部分可以用来向浏览器提供文档的一些信息。元数据包含在`head` 元素内部。除了可包含用于说明HTML文档的元素，`head` 元素还能用来规定文档与外部资源（如CSS样式表）的关系，定义内嵌CSS样式，放置和载入脚本程序。

```html
<!DOCTYPE HTML>
<html>
    <head>
        <!-- metadata goes here -->
        <title>Example</title>
    </head>

</html>
```

### 2.3  内容

文档的第三部分是文档内容，这也是最后一个部分，放在`body` 元素之中。

```html
<!DOCTYPE HTML>
<html>
    <head>
        <!-- metadata goes here -->
        <title>Example</title>
    </head>
    <body>
        <!-- content and elements go here -->
        I like <code>apples</code> and oranges.
    </body>
</html>
```

### 2.4  父元素、子元素、后代元素和兄弟元素

示例:

```html
<!DOCTYPE HTML>
<html>
    <head>
        <!-- metadata goes here -->
        <title>Example</title>
    </head>
    <body>
        <!-- content and elements go here -->
        I like <code>apples</code> and oranges.
    </body>
</html>
```

`body` 元素是`code` 元素的<b>父元素</b>，这是因为`code` 元素位于`body` 元素的开始标签和结束标签之间。反过来说，`code` 元素是`body` 元素的**子元素** 。一个元素可以拥有多个子元素，但只能有一个父元素。

`html` 元素包含着`body` 元素，而后者又包含着`code` 元素。`body` 元素和`code` 元素都是`html` 元素的**后代元素** ，但是二者中只有`body` 元素才是`html` 元素的子元素。子元素是关系最近的后代元素。具有同一个父元素的几个元素互为**兄弟元素** 。在代码清单中，`head` 元素和`body` 元素就是兄弟，这是因为它们都是`html` 元素的子元素。

### 2.5  元素类型

HTML5规范将元素分为三大类：**元数据元素** （metadata element）、**流元素** （flow element）和**短语元素** （phrasing element）。

元数据元素用来构建HTML文档的基本结构，以及就如何处理文档向浏览器提供信息和指示。

短语元素是HTML的基本成分。流元素是短语元素的超集。它们的用途是确定一个元素合法的父元素和子元素范围。

## 3. 常用HTML实体

| 字符 | 实体名称 | 实体编号 |
| :--- | :------- | :------- |
| `<`  | \&lt;    | \&#60;   |
| `>`  | \&gt;    | \&#62;   |
| `&`  | \&amp;   | \&#38;   |
| `€`  | \&euro;  | \&#8364; |
| `￡` | \&pound; | \&#163;  |
| `§`  | \&sect;  | \&#167;  |
| `©`  | \&copy;  | \&#169;  |
| `®`  | \&reg;   | \&#174;  |
| `™`  | \&trade; | \&#8482; |

## 4. 全局属性

每种元素都能规定自己的属性，这种属性称为**局部属性**。

用来配置所有元素共有的行为， 这种属性成为**全局属性**。全局属性可以用在任何一个元素身上，不过这不一定会带来有意义或有用的行为改变。

 ### 4.1  `accesskey` 属性

使用`accesskey` 属性可以设定一个或几个用来选择页面上的元素的快捷键。

```html
Name: <input type="text" name="name" accesskey="n"/>
```

其目的是让网页或网站的熟客可以使用快捷键访问经常用到的元素。用来触发`accesskey` 机制的按键组合因平台而异，在Windows系统上是同时按下Alt键和`accesskey` 属性值对应的键。

### 4.2  `class` 属性

`class` 属性用来将元素归类。这样做通常是为了能够找出文档中的某一类元素或为某一类元素应用CSS样式。

```html
<a class="class2 otherclass" href="http://w3c.org">W3C web site</a>
```

一个元素可以被归入多个类别，为此在`class` 属性值中提供多个用空格分隔的类名即可。

### 4.3 `contenteditable` 属性

`contenteditable` 是HTML5中新增加的属性，其用途是让用户能够修改页面上的内容。

```html
<p contenteditable="true">It is raining right now</p>
```

### 4.4  `contextmenu` 属性

`contextmenu` 属性用来为元素设定快捷菜单。这种菜单会在受到触发的时候（例如，Windows用户用鼠标右击时）弹出来。暂无浏览器支持。

### 4.5  `dir` 属性

`dir` 属性用来规定元素中文字的方向。其有效值有两个：`ltr` （用于从左到右的文字）和`rtl` （用于从右到左的文字）。

```
<p dir="rtl">This is right-to-left</p>
<p dir="ltr">This is left-to-right</p>
```

### 4.6  `draggable` 属性

`draggable` 属性是HTML5支持拖放操作的方式之一，用来表示元素是否可被拖放。

### 4.7  `dropzone` 属性

`dropzone` 属性是HTML5支持拖放操作的方式之一，与上述`draggable` 属性搭配使用。

### 4.8  `hidden` 属性

`hidden` 是个布尔属性，表示相关元素当前毋需关注。浏览器对它的处理方式是隐藏相关元素。

### 4.9  `id` 属性

`id` 属性用来给元素分配一个唯一的标识符。这种标识符通常用来将样式应用到元素上或在JavaScript程序中用来选择元素。

```html
<a id="w3clink" href="http://w3c.org">W3C web site</a>
```

为了根据`id` 属性值应用样式，需要在定义样式时使用一个以`#` 号开头后接`id` 属性值的选择器。

### 4.10  `lang` 属性

`lang` 属性用于说明元素内容使用的语言。

```html
<p lang="en">Hello - how are you?</p>
<p lang="fr">Bonjour - comment êtes-vous?</p>
<p lang="es">Hola -  cómo estás?</p>
```

`lang` 属性值必须使用有效的ISO语言代码。关于如何声明语言的全面说明可参阅这个网址：http://tools.ietf.org/html/bcp47 。

### 4.11  `spellcheck` 属性

`spellcheck` 属性用来表明浏览器是否应该对元素的内容进行拼写检查。这个属性只有用在用户可以编辑的元素上时才有意义。

```html
<textarea spellcheck="true">This is some mispelled text</textarea>
```

### 4.12  `style` 属性

`style` 属性用来直接在元素身上定义CSS样式。

### 4.13  `tabindex` 属性

HTML页面上的键盘焦点可以通过按Tab键在各元素之间切换。用`tabindex` 属性可以改变默认的转移顺序。

```html
<form>
    <label>Name: <input type="text" name="name" tabindex="1"/></label>
    <p/>
    <label>City: <input type="text" name="city" tabindex="-1"/></label>
    </p>
    <label>Country: <input type="text" name="country" tabindex="2"/></label>
    </p>
    <input type="submit" tabindex="3"/>
</form>
```

`tabindex` 值为1的元素会第一个被选中。用户按一下Tab键后，`tabindex` 值为2的那个元素会被选中，依次类推。`tabindex` 设置为-1的元素不会在用户按下Tab键后被选中。

### 4.14  `title` 属性

`title` 属性提供了元素的额外信息。浏览器通常用这些东西显示工具提示。

## 5.  初探CSS

CSS样式由一条或多条以分号隔开的样式声明组成。每条声明包含着一个CSS属性和该属性的值，二者以冒号分隔。

```
background-color:grey; color:white
```

### 5.1  样式的使用方式

<b>元素内嵌样式</b>

```
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>

    </head>
    <body>
        <a href="http://apress.com" style="background-color:grey; color:white">
            Visit the Apress website
        </a>
        <p>I like <span>apples</span> and oranges.</p>
        <a href="http://w3c.org">Visit the W3C website</a>
    </body>
</html>
```

<b>文档内嵌样式</b>

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <style type="text/css">
            a {
                background-color:grey;
                color:white
            }
        </style>
    </head>
    <body>
        <a href="http://apress.com">Visit the Apress website</a>
        <p>I like <span>apples</span> and oranges.</p>
        <a href="http://w3c.org">Visit the W3C website</a>
    </body>
</html>
```

<b>外部样式表</b>

styles.css

```css
a {
    background-color:grey;
    color:white
}
span {
    border: thin black solid;
    padding: 10px;
}
```

element.html

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <link rel="stylesheet" type="text/css" href="styles.css"></link>
    </head>
    <body>
        <a href="http://apress.com">Visit the Apress website</a>
        <p>I like <span>apples</span> and oranges.</p>
        <a href="http://w3c.org">Visit the W3C website</a>
    </body>
</html>
```

<b>@import</b>

用`@import` 语句将样式从一个样式表导入另一个样式表。

combind.css

```html
@import "styles.css";
span {
    border: medium black dashed;
    padding: 10px;
}
```

一个样式表中想要导入多少别的样式表都行，为每个样式表使用一条`@import` 语句即可。`@import` 语句必须位于样式表顶端，样式表自已的样式定义不能出现在它之前。在combined.css这个样式表中，先导入了styles.css，然后再定义了一条针对`span` 元素的新样式。

在CSS样式表中可以出现在`@import` 语句之前的只有`@charset` 语句。后者用于声明样式表使用的字符编码。

```
@charset "UTF-8";
@import "styles.css";
span {
    border: medium black dashed;
    padding: 10px;
}
```

<b>`@import` 语句的效率往往不如处理多个`link` 元素并靠样式层叠</b>

### 5.2  样式的层叠和继承

<b>浏览器根据层叠和继承规则确定显示一个元素时各种样式属性采用的值</b>。每个元素都有一套浏览器呈现页面时要用到的CSS属性。对于每一个这种属性，浏览器都需要查看一下其所有的样式来源。

<b>样式的来源</b>

除了<b>元素内嵌</b>、<b>文档内嵌</b>和<b>外部样式</b>表三种定义样式来源的方式，还有<b>浏览器样式</b>和<b>用户样式</b>两种。

- 元素内嵌样式、文档内嵌样式和外部样式表统称为作者样式

浏览器样式：

浏览器样式（更恰当的名称是**用户代理样式** ）是元素尚未设置样式时浏览器应用在它身上的默认样式。以a元素为例，链接的文字内容被显示为蓝色，而且带有下划线。推测其样式为：

```css
a {
    color: blue;
    text-decoration: underline;
}
```

用户样式：

大多数浏览器都允许用户定义自己的样式表。这类样式表中包含的样式称为用户样式。各种浏览器都有自己管理用户样式的方式。以谷歌的Chrome为例，它会在用户的个人配置信息目录中生成一个名为Default\User StyleSheets\Custom.css的文件。添加到这个文件中的任何样式都会被用于用户访问的所有网站。

<b>样式如何层叠</b>

明白了浏览器所要查看的所有样式来源之后，现在可以看看浏览器要显示元素时求索一个CSS属性值的次序。这个次序很明确：

(1) 元素内嵌样式（用元素的全局属性`style` 定义的样式）；

(2) 文档内嵌样式（定义在`style` 元素中的样式）；

(3) 外部样式（用`link` 元素导入的样式）；

(4) 用户样式（用户定义的样式）；

(5) 浏览器样式（浏览器应用的默认样式）。

<b>用重要样式调整层叠次序</b>

把样式属性值标记为重要（important），可以改变正常的层叠次序.

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <style type="text/css">
            a {
                color: black !important;
            }
        </style>
    </head>
    <body>
        <a style="color:red" href="http://apress.com">Visit the Apress website</a>
        <p>I like <span>apples</span> and oranges.</p>
        <a href="http://w3c.org">Visit the W3C website</a>
    </body>
</html>
```

在样式声明后附上`!important` 即可将对应属性值标记为重要。不管这种样式属性定义在什么地方，浏览器都会给予优先考虑。

<b>根据具体程度和定义次序解决同级样式冲突</b>

如果有两条定义于同一层次的样式都能应用于同一个元素，而且它们都包含着浏览器要查看的CSS属性值，这时就需要另加砝码助天平上持平的双方一决高下。为了判断该用哪个值，浏览器会评估两条样式的具体程度，然后选中较为特殊的那条。样式的具体程度通过统计三类特征得出：

(1) 样式的选择器中`id` 值的数目；

(2) 选择器中其他属性和伪类的数目；

(3) 选择器中元素名和伪元素的数目。

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <style type="text/css">
            a {
                color: black;
            }
            a.myclass {
                color:white;
                background:grey;
            }
        </style>
    </head>
    <body>
        <a href="http://apress.com">Visit the Apress website</a>
        <p>I like <span>apples</span> and oranges.</p>
        <a class="myclass" href="http://w3c.org">Visit the W3C website</a>
    </body>
</html>
```

在评定具体程度时要按a-b-c的形式（其中每一个字母依次代表上述三类特征的统计结果）生成一个数字。它不是一个三位数。如果对某个样式算出的a值最大，那么它就是具体程度高的那个。只有a值相等时浏览器才会比较b值，此时b值较大的样式具体程度更高。只有a值和b值都分别相等时浏览器才会考虑c值。也就是说，1-0-0这个得分比0-5-5这个得分代表的具体程度更高。

本例中`a.myclass` 这个选择器含有一个`class` 属性，于是该样式的值为0-1-0（0个`id` 值+1个其他属性+0个元素名）。另一条样式的具体程度值为0-0-0（因为它不包含`id` 值、其他属性或元素名）。因此浏览器在呈现被归入`myclass` 类的`a` 元素时将使用`a.myclass` 样式中设定的`color` 属性值。对于所有其他`a` 元素，使用的则是另一条样式中设定的值。

如果同一个样式属性出现在具体程度相当的几条样式中，那么浏览器会根据其位置的先后选择所用的值，规则是后来者居上。

<b>继承</b>

如果浏览器在直接相关的样式中找不到某个属性的值，就会求助于继承机制，使用父元素的这个样式属性值。但不是所有的样式都可以继承。

使用`inherit` 强制继承：

```
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <style type="text/css">
            p {
                color:white;
                background:grey;
                border: medium solid black;
            }
            span {
                border: inherit;
            }
        </style>
    </head>
    <body>
        <a href="http://apress.com">Visit the Apress website</a>
        <p>I like <span>apples</span> and oranges.</p>
        <a class="myclass1 myclass2" href="http://w3c.org">Visit the W3C website</a>
    </body>
</html>
```

### 5.3  CSS中的颜色

<b>使用十进制或十六进制设置红、绿、蓝三种颜色成分的值</b>

| 颜色名称 | 十六进制表示 | 十进制表示    |
| :------- | :----------- | :------------ |
| `black`  | `#000000`    | `0,0,0`       |
| `silver` | `#C0C0C0`    | `192,192,192` |
| `gray`   | `#808080`    | `128,128,128` |
| `white`  | `#FFFFFF`    | `255,255,255` |
| `maroon` | `#800000`    | `128,0,0`     |
| `red`    | `#FF0000`    | `255,0,0`     |
| `purple` | `#800080`    | `128,0,128`   |
| `fushia` | `#FF00FF`    | `255,0,255`   |
| `green`  | `#008000`    | `0,128,0`     |
| `lime`   | `#00FF00`    | `0,255,0`     |
| `olive`  | `#808000`    | `128,128,0`   |
| `yellow` | `#FFFF00`    | `255,255,0`   |
| `navy`   | `#000080`    | `0,0,128`     |
| `blue`   | `#0000FF`    | `0,0,255`     |
| `teal`   | `#008080`    | `0,128,128`   |
| `aqua`   | `#00FFFF`    | `0,255,255`   |

| 颜色名称         | 十六进制表示 | 十进制表示    |
| :--------------- | :----------- | :------------ |
| `darkgray`       | `#a9a9a9`    | `169,169,169` |
| `darkslategray`  | `#2f4f4f`    | `47,79,79`    |
| `dimgray`        | `#696969`    | `105,105,105` |
| `gray`           | `#808080`    | `128,128,128` |
| `lightgray`      | `#d3d3d3`    | `211,211,211` |
| `lightslategray` | `#778899`    | `119,136,153` |
| `slategray`      | `#708090`    | `112,128,144` |

<b>其他方式</b>

| 函数               | 说明                                                         | 示例                               |
| :----------------- | :----------------------------------------------------------- | :--------------------------------- |
| `rgb(r, g, b)`     | 用RGB模型表示颜色                                            | `color: rgb(112, 128, 144)`        |
| `rgba(r, g, b, a)` | 用RGB模型表示颜色，外加一个用于表示透明度的 *`α`* 值（0代表全透明，1代表完全不透明） | `color: rgba(112, 128, 144, 0.4)`  |
| `hsl(h, s, l)`     | 用HSL模型（色相〔hue〕、饱和度〔saturation〕和明度〔lightness〕）表示颜色 | `color: hsl(120, 100%, 22%)`       |
| `hsla(h, s, l, a)` | 与HSL模式类似，只不过增加了一个表示透明度的 *`α`* 值         | `color: hsla(120, 100%, 22%, 0.4)` |

### 5.4  CSS中的长度

<b>绝对长度</b>

这类单位是现实世界的度量单位。

| 单位标识符 | 说明                  |
| :--------- | :-------------------- |
| in         | 英寸                  |
| cm         | 厘米                  |
| mm         | 毫米                  |
| pt         | 磅（1磅等于1/72英寸） |
| pc         | pica（1pica等于12磅） |

<b>相对长度</b>

相对长度的规定和实现都比绝对长度更复杂，需要以严密、精确的语言明确定义。相对单位的测量需要依托其他类型的单位。

| 单位标识符 | 说明                                   |
| :--------- | :------------------------------------- |
| em         | 与元素字号挂钩                         |
| ex         | 与元素字体的“*x* 高度”挂钩             |
| rem        | 与根元素的字号挂钩                     |
| px         | CSS像素（假定显示设备的分辨率为96dpi） |
| %          | 另一属性的值的百分比                   |

### 5.5  其他CSS样式

<b>角度单位</b>

| 单位标识符 | 说明                               |
| :--------- | :--------------------------------- |
| deg        | 度（取值范围：0deg～360deg）       |
| grad       | 百分度（取值范围：0grad～400grad） |
| rad        | 弧度（取值范围：0rad～6.28rad）    |
| turn       | 圆周（1turn等于360deg）            |

<b>时间单位</b>

| 单位标识符 | 说明                 |
| :--------- | :------------------- |
| s          | 秒                   |
| ms         | 毫秒（1s等于1000ms） |

### 5.6  CSS的改进与框架

<b>改进</b>

CSS的用户很快就会发现用它来描述样式比较啰嗦。大量重复性内容的存在导致样式的长期维护工作既费时间又容易出错。

有一个名为LESS的工具可以用来扩展CSS，它使用JavaScript对CSS予以改进。这个工具有一些不错的特性，包括变量、样式间的继承以及函数等。我最近经常使用LESS，其效果令人满意。读者可从以下网站详细了解并下载该JavaScript库：[http://lesscss.org](http://lesscss.org/) 。

<b>框架</b>

- [Bootstrap](https://github.com/twbs/bootstrap)
- [Semantic-UI](https://github.com/Semantic-Org/Semantic-UI)
- [element](https://github.com/ElemeFE/element)
- [ant-design](https://github.com/ant-design/ant-design)
- [layui](https://github.com/sentsin/layui)
- [bulma](https://github.com/jgthms/bulma)

<h4>查看CSS样式在各浏览器支持情况：https://caniuse.com/</h4>

## 6. 初探JavaScript

### 6.1  使用javascript

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
    </head>
    <body>
        <script type="text/javascript">
            document.writeln("Hello");
        </script>
    </body>
</html>
```

### 6.2  语句

JavaScript的基本元素是语句。一条语句代表着一条命令，通常以分号（`;` ）结尾。实际上分号用不用都可以，不过加上分号可让代码更易阅读，并且可以在一行书写几条语句。

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
    </head>
    <body>
        <script type="text/javascript">
            document.writeln("This is a statement");
            document.writeln("This is also a statement");
        </script>
    </body>
</html>
```

### 6.3  定义和使用函数

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
    </head>
    <body>
        <script type="text/javascript">

            function myFunc() {
                document.writeln("This is a statement");
            };

            myFunc();
        </script>
    </body>
</html>
```

函数所含语句被包围在一对大括号（`{` 和`}` ）之间，称为代码块。这个代码清单定义了一个名为`myFunc` 的函数，其代码块中只含有一条语句。JavaScript是区分大小写的语言，因此`function` 这个关键字必须小写。

<b>传入参数</b>

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
    </head>
    <body>
        <script type="text/javascript">

            function myFunc(name, weather) {
                document.writeln("Hello " + name + ".");
                document.writeln("It is " + weather + " today");
            };

            myFunc("Adam", "sunny");
        </script>
    </body>
</html>
```

<b>返回值</b>

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
    </head>
    <body>
        <script type="text/javascript">

            function myFunc(name) {
                return ("Hello " + name + ".");
            };
            document.writeln(myFunc("Adam"));
        </script>
    </body>
</html>
```

### 6.4  变量和类型

定义变量要使用`var` 关键字，在定义的同时还可以像在一条单独的语句中那样为其赋值。定义在函数中的变量称为**局部变量** ，只能在该函数范围内使用。直接在`script` 元素中定义的变量称为**全局变量** ，可以在任何地方使用——包括在其他脚本中。

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
    </head>
    <body>
        <script type="text/javascript">
            var myGlobalVar = "apples";

            function myFunc(name) {
                var myLocalVar = "sunny";
                return ("Hello " + name + ". Today is " + myLocalVar + ".");
            };
            document.writeln(myFunc("Adam"));
        </script>
        <script type="text/javascript">
            document.writeln("I like " + myGlobalVar);
        </script>
    </body>
</html>
```

<b>基本类型</b>

JavaScript定义了一小批基本类型（primitive type），它们包括字符串类型（`string` ）、数值类型（`number` ）和布尔类型（`boolean` ）。

**字符串类型**

`string` 类型的值可以用夹在一对双引号或单引号之间的一串字符表示。

```
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
    </head>
    <body>
        <script type="text/javascript">
            var firstString = "This is a string";
            var secondString = 'And so is this';
        </script>
    </body>
</html>
```

**布尔类型**

`boolean` 类型有两个值：`true` 和`false` 。

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
    </head>
    <body>
        <script type="text/javascript">
            var firstBool = true;
            var secondBool = false;
        </script>
    </body>
</html>
```

**数值类型**

整数和浮点数（也称实数）都用`number` 类型表示。

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
    </head>
    <body>
        <script type="text/javascript">
            var daysInWeek = 7;
            var pi = 3.14;
            var hexValue = 0xFFFF;
        </script>
    </body>
</html>
```

### 6.5  对象

<b>创建对象</b>

第一种方式(new Object())：

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
    </head>
    <body>
        <script type="text/javascript">
            var myData = new Object();
            myData.name = "Adam";
            myData.weather = "sunny";

            document.writeln("Hello " + myData.name + ". ");
            document.writeln("Today is " + myData.weather + ".");
        </script>
    </body>
</html>
```

第二种方式(对象字面量)：

```
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
    </head>
    <body>
        <script type="text/javascript">
            var myData = {
                name: "Adam",
                weather: "sunny"
            };

            document.writeln("Hello " + myData.name + ". ");
            document.writeln("Today is " + myData.weather + ".");
        </script>
    </body>
</html>
```

<b>使用对象</b>

读取和修改对象属性值示例：

```
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
    </head>
    <body>
        <script type="text/javascript">
            var myData = {
                name: "Adam",
                weather: "sunny",
            };

            myData.name = "Joe";
            myData["weather"] = "raining";

            document.writeln("Hello " + myData.name + ".");
            document.writeln("It is " + myData["weather"]);

        </script>
    </body>
</html>
```

为对象添加新属性：

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
    </head>
    <body>
        <script type="text/javascript">
            var myData = {
                name: "Adam",
                weather: "sunny",
            };

            myData.dayOfWeek = "Monday";
        </script>
    </body>
</html>
```

为对象添加新方法：

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
    </head>
    <body>
        <script type="text/javascript">
            var myData = {
                name: "Adam",
                weather: "sunny",
            };

            myData.sayHello = function() {
              document.writeln("Hello");
            };

        </script>
    </body>
</html>
```

删除对象属性：

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
    </head>
    <body>
        <script type="text/javascript">
            var myData = {
                name: "Adam",
                weather: "sunny",
            };

            myData.sayHello = function() {
              document.writeln("Hello");
            };

            delete myData.name;
            delete myData["weather"];
            delete myData.sayHello;
        </script>
    </body>
</html>
```

检查对象是否具有某个属性：

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
    </head>
    <body>
        <script type="text/javascript">
            var myData = {
                name: "Adam",
                weather: "sunny",
            };

            var hasName = "name" in myData;
            var hasDate = "date" in myData;

            document.writeln("HasName: " + hasName);
            document.writeln("HasDate: " + hasDate);

        </script>
    </body>
</html>
```

### 6.6  常用运算符

| 运算符                      | 说明                           |
| :-------------------------- | :----------------------------- |
| `++` 、`--`                 | 前置或后置自增和自减           |
| `+` 、`-` 、`*` 、`/` 、`%` | 加、减、乘、除、求余           |
| `<` 、`<=` 、`>` 、`>=`     | 小于、小于等于、大于、大于等于 |
| `==` 、`!=`                 | 相等和不相等                   |
| `===` 、`!==`               | 等同和不等同                   |
| `&&` 、`||`                 | 逻辑与、逻辑或                 |
| `=`                         | 赋值                           |
| `+`                         | 字符串连接                     |
| `?:`                        | 三元条件语句                   |

相等和等同运算符需要特别说明一下。相等运算符会尝试将操作数转换为同一类型以便判断是否相等。如果想判断值**和** 类型是否都相同，那么应该使用的是等同运算符。

<b>强制类型转换</b>

数值转换为字符串

| 方法                                       | 说明                                                         | 返回   |
| :----------------------------------------- | :----------------------------------------------------------- | :----- |
| `toString()`                               | 以十进制形式表示数值                                         | 字符串 |
| `toString(2)` `toString(8)` `toString(16)` | 以二进制、八进制和十六进制形式表示数值                       | 字符串 |
| `toFixed(n)`                               | 以小数点后有*n* 位数字的形式表示实数                         | 字符串 |
| `toExponential(n)`                         | 以指数表示法表示数值。尾数的小数点前后分别有1位数字和*n* 位数字 | 字符串 |
| `toPrecision(n)`                           | 用*n* 位有效数字表示数值，在必要的情况下使用指数表示法       | 字符串 |

将字符串转换为数值

| 函数                | 说明                                     |
| :------------------ | :--------------------------------------- |
| `Number(<str>)`     | 通过分析指定字符串，生成一个整数或实数值 |
| `parseInt(<str>)`   | 通过分析指定字符串，生成一个整数值       |
| `parseFloat(<str>)` | 通过分析指定字符串，生成一个整数或实数值 |

### 6.7  数组

<b>创建数组</b>

第一种方式(new Array()):

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
    </head>
    <body>
        <script type="text/javascript">

            var myArray = new Array();
            myArray[0] = 100;
            myArray[1] = "Adam";
            myArray[2] = true;

        </script>
    </body>
</html>
```

第二种方式(字面量):

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
    </head>
    <body>
        <script type="text/javascript">

          var myArray = [100, "Adam", true];

        </script>
    </body>
</html>
```

读取和修改数组内容：

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
    </head>
    <body>
        <script type="text/javascript">
            var myArray = [100, "Adam", true];
            myArray[0] = "Tuesday";
            document.writeln("Index 0: " + myArray[0]);
        </script>
    </body>
</html>
```

常用方法：

| 方法                   | 说明                                                         | 返回   |
| :--------------------- | :----------------------------------------------------------- | :----- |
| `concat(<otherArray>)` | 将数组和参数所指数组的内容合并为一个新数组。可指定多个数组   | 数组   |
| `join(<separator>)`    | 将所有数组元素连接为一个字符串。各元素内容用参数指定的字符分隔 | 字符串 |
| `pop()`                | 把数组当做栈使用，删除并返回数组的最后一个元素               | 对象   |
| `push(<item>)`         | 把数组当做栈使用，将指定的数据添加到数组中                   | `void` |
| `reverse()`            | 就地反转数组元素的次序                                       | 数组   |
| `shift()`              | 类似`pop` ，但操作的是数组的第一个元素                       | 对象   |
| `slice(<start>,<end>)` | 返回一个子数组                                               | 数组   |
| `sort()`               | 就地对数组元素排序                                           | 数组   |
| `unshift(<item>)`      | 类似`push` ，但新元素被插到数组的开头位置                    | `void` |

### 6.8  错误处理

JavaScript用`try...catch` 语句处理错误。

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
    </head>
    <body>
        <script type="text/javascript">
            try {
                var myArray;
                for (var i = 0; i < myArray.length; i++) {
                    document.writeln("Index " + i + ": " + myArray[i]);
                }
            } catch (e) {
                document.writeln("Error: " + e);
            } finally {
                document.writeln("Statements here are always executed");
            }
        </script>
    </body>
</html>
```

**`Error` 对象**

| 属性      | 说明                           | 返回   |
| :-------- | :----------------------------- | :----- |
| `message` | 对错误条件的说明               | 字符串 |
| `name`    | 错误的名称，默认为`Error`      | 字符串 |
| `number`  | 该错误的错误代号（如果有的话） | 数值   |

`finally` 子句，不管是否发生错误都执行。

### 6.9  比较`undefined` 和`null` 值

JavaScript中有两个特殊值：`undefined` 和`null` ，在比较它们的时候需要留心。在读取未赋值的变量或试图读取对象没有的属性时得到的就是`undefined` 值。`null` ，这个值与`undefined` 略有不同。后者是在未定义值的情况下得到的值，而前者则用于表示已经赋了一个值但该值不是一个有效的`object` 、`string` 、`number` 或`boolean` 值。

