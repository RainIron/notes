# DOM

DOM（Document Object Model，文档对象模型）允许我们用JavaScript来探查和操作HTML文档里的内容。它对于创建丰富性内容而言是必不可少的一组功能。

[TOC]

## 1. 理解DOM

DOM是一组对象的集合，这些对象代表了HTML文档里的各个元素。顾名思义，DOM就像一个**模型** ，它由代表**文档** 的众多**对象** 组成。DOM是Web开发的关键工具之一，它是HTML文档的结构和内容与JavaScript之间的桥梁。

作为显示HTML文档过程中的一个步骤，浏览器会解析HTML并创建一个模型。这个模型保存了各个HTML元素之间的层级关系，每个元素都由一个JavaScript对象表示。

文档模型里任何代表某个元素的对象都**至少** 能支持`HTMLElement` 功能，其中一些还支持额外的功能。

### 1.1  理解DOM Level和兼容性

开始使用DOM时，你会碰到网络上一些文章和教程提到DOM Level（即DOM等级，比如某个特定功能是在DOM Level 3中定义的）。DOM Level是标准化过程中的版本号，在大多数情况下应该忽略它们。

DOM的标准化过程并不是完全成功的，每一个DOM Level都有描述它的标准和文档，但它们并没有被完整地实现，浏览器只是简单地挑选了其中的有用功能，而忽略了其他的。更糟糕的是，已经实现的功能之间还存在着某种程度的不一致性。

有多种方式可用来应对DOM功能的多变性。第一种方式是使用某个JavaScript库（比如jQuery），它消除了浏览器之间实现方式的差别。使用库的优点在于其一致性，但缺点是只能使用库支持的那些功能。如果想突破库原有功能的局限，就只能转回到直接操作DOM上，并重新面对之前的那些问题。（这并不是说jQuery和类似的库没有价值，它们很有用，非常值得去了解一下。）

第二种是保守方式：只使用你所知的被广泛支持的那些功能。这种方式一般来说是最为明智的，不过它需要仔细而全面的测试。不仅如此，你还必须仔细测试新版的浏览器，确保对这些功能的支持没有发生变化或者被移除。

### 1.2  DOM快速查询

#### **`Document` 对象**

| 名称                              | 说明                                                        | 返回                |
| :-------------------------------- | :---------------------------------------------------------- | :------------------ |
| `activeElement`                   | 返回代表文档中当前获得焦点元素的对象                        | `HTMLElement`       |
| `body`                            | 返回代表文档中`body` 元素的对象                             | `HTMLElement`       |
| `characterSet`                    | 返回文档的字符集编码。这是一个只读属性                      | 字符串              |
| `charset`                         | 获取或设置文档的字符集编码                                  | 字符串              |
| `childNodes`                      | 返回子元素的集合                                            | `HTMLElement[]`     |
| `compatMode`                      | 获取文档的兼容性模式                                        | 字符串              |
| `cookie`                          | 获取或设置当前文档的cookie                                  | 字符串              |
| `defaultCharset`                  | 获取浏览器所使用的默认字符编码                              | 字符串              |
| `defaultView`                     | 返回当前文档的`Window` 对象。关于这个对象的详情请参见第26章 | `Window`            |
| `dir`                             | 获取或设置文档的文本方向                                    | 字符串              |
| `domain`                          | 获取或设置当前文档的域名                                    | 字符串              |
| `embeds` `plugins`                | 返回所有代表文档中`embed` 元素的对象                        | `HTMLCollection`    |
| `firstChild`                      | 返回某个元素的第一个子元素                                  | `HTMLElement`       |
| `forms`                           | 返回所有代表文档中`form` 元素的对象                         | `HTMLCollection`    |
| `getElementById(<id>)`            | 返回带有指定`id` 值的元素                                   | `HTMLElement`       |
| `getElementsByClassName(<class>)` | 返回带有指定`class` 值的元素                                | `HTMLElement[]`     |
| `getElementsByName(<name>)`       | 返回带有指定`name` 值的元素                                 | `HTMLElement[]`     |
| `getElementsByTagName(<tag>)`     | 返回带有指定类型的元素                                      | `HTMLElement[]`     |
| `hasChildNodes()`                 | 如果当前元素有子元素则返回`true`                            | 布尔值              |
| `head`                            | 返回代表`head` 元素的对象                                   | `HTMLHeadElement`   |
| `images`                          | 返回所有代表`img` 元素的对象                                | `HTMLCollection`    |
| `implementation`                  | 提供关于DOM可用功能的信息                                   | `DOMImplementation` |
| `lastChild`                       | 返回最后一个子元素                                          | `HTMLElement`       |
| `lastModified`                    | 返回文档的最后修改时间                                      | 字符串              |
| `links`                           | 返回所有代表文档中具备`href` 属性的`a` 和`area` 元素的对象  | `HTMLCollection`    |
| `location`                        | 提供关于当前文档URL的信息                                   | `Location`          |
| `nextSibling`                     | 返回位于当前元素之后的兄弟元素                              | `HTMLElement`       |
| `parentNode`                      | 返回父元素                                                  | `HTMLElement`       |
| `previousSibling`                 | 返回位于当前元素之前的兄弟元素                              | `HTMLElement`       |
| `querySelector(<selector>)`       | 返回匹配特定CSS选择器的第一个元素                           | `HTMLElement`       |
| `querySelectorAll(<selector>)`    | 返回匹配特定CSS选择器的所有元素                             | `HTMLElement[]`     |
| `readyState`                      | 返回当前文档的状态                                          | 字符串              |
| `referrer`                        | 返回链接到当前文档的文档URL（它是对应HTTP标头的值）         | 字符串              |
| `scripts`                         | 返回所有代表`script` 元素的对象                             | `HTMLCollection`    |
| `title`                           | 获取或设置当前文档的标题                                    | 字符串              |

#### **`Location` 对象**

| 名称                | 说明                                  | 返回   |
| :------------------ | :------------------------------------ | :----- |
| `assign(<URL>)`     | 导航到指定的URL上                     | `void` |
| `hash`              | 获取或设置文档URL的锚（井号串）部分   | 字符串 |
| `host`              | 获取或设置文档URL的主机名和端口号部分 | 字符串 |
| `hostname`          | 获取或设置文档URL的主机名部分         | 字符串 |
| `href`              | 获取或设置当前文档的地址              | 字符串 |
| `pathname`          | 获取或设置文档URL的路径部分           | 字符串 |
| `port`              | 获取或设置文档URL的端口号部分         | 字符串 |
| `protocol`          | 获取或设置文档URL的协议部分           | 字符串 |
| `reload()`          | 重新加载当前文档                      | `void` |
| `replace(<URL>)`    | 清除当前文档并导航至URL所指定的新文档 | `void` |
| `resolveURL(<URL>)` | 将指定的相对URL解析为绝对URL          | 字符串 |
| `search`            | 获取或设置文档URL的查询（问号串）部分 | 字符串 |

#### **`Window` 对象**

| 名称                              | 说明                                            | 返回       |
| :-------------------------------- | :---------------------------------------------- | :--------- |
| `alert(<msg>)`                    | 向用户显示一个对话框窗口并等候其被关闭          | `void`     |
| `blur()`                          | 让窗口失去键盘焦点                              | `void`     |
| `clearInterval(<id>)`             | 撤销某个时间间隔计时器                          | `void`     |
| `clearTimeout(<id>)`              | 撤销某个超时计时器                              | `void`     |
| `close()`                         | 关闭窗口                                        | `void`     |
| `confirm(<msg>)`                  | 显示一个带有确认和取消提示的对话框窗口          | 布尔值     |
| `defaultView`                     | 返回活动文档的`Window`                          | `Window`   |
| `document`                        | 返回与此窗口关联的`Document` 对象               | `Document` |
| `focus()`                         | 让窗口获得键盘焦点                              | `void`     |
| `frames`                          | 返回文档内嵌`iframe` 元素的`Window` 对象数组    | `Window[]` |
| `history`                         | 提供对浏览器历史的访问                          | `History`  |
| `innerHeight`                     | 获取窗口内容区域的高度                          | 数值       |
| `innerWidth`                      | 获取窗口内容区域的宽度                          | 数值       |
| `length`                          | 返回文档内嵌的`iframe` 元素数量                 | 数值       |
| `location`                        | 提供当前文档地址的详细信息                      | `Location` |
| `opener`                          | 返回打开当前浏览器上下文环境`Window`            | `Window`   |
| `outerHeight`                     | 获取窗口的高度，包括边框和菜单栏等              | 数值       |
| `outerWidth`                      | 获取窗口的宽度，包括边框和菜单栏等              | 数值       |
| `pageXOffset`                     | 获取窗口从左上角算起水平滚动过的像素数          | 数值       |
| `pageYOffset`                     | 获取窗口从左上角算起垂直滚动过的像素数          | 数值       |
| `parent`                          | 返回当前`Window` 的父`Window`                   | `Window`   |
| `postMessage(<msg>, <origin>)`    | 给另一个文档发送消息                            | `void`     |
| `print()`                         | 提示用户打印页面                                | `void`     |
| `prompt(<msg>, <val>)`            | 显示对话框提示用户输入一个值                    | 字符串     |
| `screen`                          | 返回一个描述屏幕的`Screen` 对象                 | `Screen`   |
| `screenLeft` `screenX`            | 获取从窗口左边缘到屏幕左边缘的像素数            | 数值       |
| `screenTop` `screenY`             | 获取从窗口上边缘到屏幕上边缘的像素数            | 数值       |
| `scrollBy(<x>, <y>)`              | 让文档相对其当前位置进行滚动                    | `void`     |
| `scrollTo(<x>, <y>)`              | 滚动到指定的位置                                | `void`     |
| `self`                            | 返回当前文档的`Window`                          | `Window`   |
| `setInterval(<function>, <time>)` | 创建一个计时器，每隔`time` 毫秒调用指定的函数   | 整数       |
| `setTimeout(<function>, <time>)`  | 创建一个计时器，等待`time` 毫秒后调用指定的函数 | 整数       |
| `showModalDialog(<url>)`          | 弹出一个窗口，显示指定的URL                     | `void`     |
| `stop()`                          | 停止载入文档                                    | `void`     |
| `top`                             | 返回最上层的`Window`                            | `Window`   |

#### **`History` 对象**

| 名称                                    | 说明                                                         | 返回   |
| :-------------------------------------- | :----------------------------------------------------------- | :----- |
| `back()`                                | 在浏览历史里后退一步                                         | `void` |
| `forward()`                             | 在浏览历史里前进一步                                         | `void` |
| `go(<index>)`                           | 转到相对于当前文档的某个浏览历史位置。正值是前进，负值是后退 | `void` |
| `length`                                | 返回浏览历史里的项目数量                                     | 数值   |
| `pushState(<state>, <title>, <url>)`    | 向浏览器历史添加一个条目                                     | `void` |
| `replaceState(<state>, <title>, <url>)` | 替换浏览器历史中的当前条目                                   | `void` |
| `state`                                 | 返回浏览器历史里关联当前文档的状态数据                       | 对象   |

#### **`Screen` 对象**

| 名称          | 说明                                               | 返回 |
| :------------ | :------------------------------------------------- | :--- |
| `availHeight` | 返回屏幕上可供显示窗口部分的高度（排除工具栏之类） | 数值 |
| `availWidth`  | 返回屏幕上可供显示窗口部分的宽度（排除工具栏之类） | 数值 |
| `colorDepth`  | 返回屏幕的颜色深度                                 | 数值 |
| `height`      | 返回屏幕的高度                                     | 数值 |
| `width`       | 返回屏幕的宽度                                     | 数值 |

#### **`HTMLElement` 对象**

| 名称                                     | 说明                                         | 返回                 |
| :--------------------------------------- | :------------------------------------------- | :------------------- |
| `checked`                                | 获取或设置`checked` 属性的存在状态           | 布尔值               |
| `classList`                              | 获取或设置元素所属类的列表                   | `DOMTokenList`       |
| `className`                              | 获取或设置元素所属类的列表                   | 字符串               |
| `dir`                                    | 获取或设置`dir` 属性的值                     | 字符串               |
| `disabled`                               | 获取或设置`disabled` 属性的存在状态          | 布尔值               |
| `hidden`                                 | 获取或设置`hidden` 属性的存在状态            | 布尔值               |
| `id`                                     | 获取或设置`id` 属性的值                      | 字符串               |
| `lang`                                   | 获取或设置`lang` 属性的值                    | 字符串               |
| `spellcheck`                             | 获取或设置`spellcheck` 属性的存在状态        | 布尔值               |
| `tabIndex`                               | 获取或设置`tabindex` 属性的值                | 数值                 |
| `tagName`                                | 返回标签名（象征元素的类型）                 | 字符串               |
| `title`                                  | 获取或设置`title` 属性的值                   | 字符串               |
| `add(<class>)`                           | 给元素添加指定的类                           | `void`               |
| `contains(<class>)`                      | 如果元素属于指定的类则返回`true`             | 布尔值               |
| `length`                                 | 返回元素所属类的数量                         | 数值                 |
| `remove(<class>)`                        | 从元素上移除指定的类                         | `void`               |
| `toggle(<class>)`                        | 如果类不存在就添加它，如果存在则移除它       | 布尔值               |
| `attributes`                             | 返回应用到元素上的属性                       | `Attr[]`             |
| `dataset`                                | 返回以`data-` 开头的属性                     | 字符串数组`[<name>]` |
| `getAttribute(<name>)`                   | 返回指定属性的值                             | 字符串               |
| `hasAttribute(<name>)`                   | 如果元素带有指定属性则返回`true`             | 布尔值               |
| `removeAttribute(<name>)`                | 从元素上移除指定属性                         | `void`               |
| `setAttribute(<name>, <value>)`          | 应用一个指定名称和值的属性                   | `void`               |
| `appendChild(HTMLElement)`               | 将指定元素附加为当前元素的子元素             | `HTMLElement`        |
| `cloneNode(boolean)`                     | 复制某个元素                                 | `HTMLElement`        |
| `compareDocumentPosition(HTMLElement)`   | 判断某个元素的相对位置                       | 数值                 |
| `innerHTML`                              | 获取或设置元素的内容                         | 字符串               |
| `insertAdjacentHTML(<pos>, <text>)`      | 相对于元素的位置插入HTML                     | `void`               |
| `insertBefore(<newelem>, <childElem>)`   | 将第一个元素插入到第二个（子）元素之前       | `HTMLElement`        |
| `isEqualNode(<HTMLElement>)`             | 判断指定元素是否与当前元素等同               | 布尔值               |
| `isSameNode(HTMLElement)`                | 判断指定元素是否就是当前元素                 | 布尔值               |
| `outerHTML`                              | 获取或设置某个元素的HTML和内容               | 字符串               |
| `removeChild(HTMLElement)`               | 从当前元素上移除指定的子元素                 | `HTMLElement`        |
| `replaceChild(HTMLElement, HTMLElement)` | 替换当前元素的某个子元素                     | `HTMLElement`        |
| `createElement(<tag>)`                   | 用指定标签类型创建一个新的`HTMLElement` 对象 | `HTMLElement`        |
| `createTextNode(<text>)`                 | 用指定内容创建一个新的`Text` 对象            | `Text`               |

#### **`Text` 对象**

| 名称                                       | 说明                                                         | 返回   |
| :----------------------------------------- | :----------------------------------------------------------- | :----- |
| `appendData(<string>)`                     | 在文本块的末尾附加指定字符串                                 | `void` |
| `data`                                     | 获取或设置文本                                               | 字符串 |
| `deleteData(<offset>, <count>)`            | 移除字符串中的文本。第一个数字是偏移量，第二个数字是要移除的字符数量 | `void` |
| `insertData(<offset>, <string>)`           | 在指定的偏移量位置插入指定字符串                             | `void` |
| `length`                                   | 返回字符数量                                                 | 数值   |
| `replaceData(<offset>, <count>, <string>)` | 用指定字符串替换一部分文本                                   | `void` |
| `replaceWholeText(<string>)`               | 替换全部文本                                                 | `Text` |
| `splitText(<number>)`                      | 将现有的`Text` 元素在指定的偏移量处一分为二。                | `Text` |
| `substringData(<offset>, <count>)`         | 返回文本的子串                                               | 字符串 |
| `wholeText`                                | 获取文本                                                     | 字符串 |

### 1.3  DOM里的CSS属性

#### **`CSSStyleDeclaration` 对象的成员**

| 成员                   | 对应于                  |
| :--------------------- | :---------------------- |
| `background`           | `background`            |
| `backgroundAttachment` | `background-attachment` |
| `backgroundColor`      | `background-color`      |
| `backgroundImage`      | `background-image`      |
| `backgroundPosition`   | `background-position`   |
| `backgroundRepeat`     | `background-repeat`     |
| `border`               | `border`                |
| `borderBottom`         | `border-bottom`         |
| `borderBottomColor`    | `border-bottom-color`   |
| `borderBottomStyle`    | `border-bottom-style`   |
| `borderBottomWidth`    | `border-bottom-width`   |
| `borderCollapse`       | `border-collapse`       |
| `borderColor`          | `border-color`          |
| `borderLeft`           | `border-left`           |
| `borderLeftColor`      | `border-left-color`     |
| `borderLeftStyle`      | `border-left-style`     |
| `borderLeftWidth`      | `border-left-width`     |
| `borderRight`          | `border-right`          |
| `borderRightColor`     | `border-right-color`    |
| `borderRightStyle`     | `border-right-style`    |
| `borderRightWidth`     | `border-right-width`    |
| `borderSpacing`        | `border-spacing`        |
| `borderStyle`          | `border-style`          |
| `borderTop`            | `border-top`            |
| `borderTopColor`       | `border-top-color`      |
| `borderTopStyle`       | `border-top-style`      |
| `borderTopWidth`       | `border-top-width`      |
| `borderWidth`          | `border-width`          |
| `captionSide`          | `caption-side`          |
| `clear`                | `clear`                 |
| `color`                | `color`                 |
| `cssFloat`             | `float`                 |
| `cursor`               | `cursor`                |
| `direction`            | `direction`             |
| `display`              | `display`               |
| `emptyCells`           | `empty-cells`           |
| `font`                 | `font`                  |
| `fontFamily`           | `font-family`           |
| `fontSize`             | `font-size`             |
| `fontStyle`            | `font-style`            |
| `fontVariant`          | `font-variant`          |
| `fontWeight`           | `font-weight`           |
| `height`               | `height`                |
| `letterSpacing`        | `letter-spacing`        |
| `lineHeight`           | `line-height`           |
| `listStyle`            | `list-style`            |
| `listStyleImage`       | `list-style-image`      |
| `listStylePosition`    | `list-style-position`   |
| `listStyleType`        | `list-style-type`       |
| `margin`               | `margin`                |
| `marginBottom`         | `margin-bottom`         |
| `marginLeft`           | `margin-left`           |
| `marginRight`          | `margin-right`          |
| `marginTop`            | `margin-top`            |
| `maxHeight`            | `max-height`            |
| `maxWIdth`             | `max-width`             |
| `minHeight`            | `min-height`            |
| `minWidth`             | `min-width`             |
| `outline`              | `outline-19`            |
| `outlineColor`         | `outline-color`         |
| `outlineStyle`         | `outline-style`         |
| `outlineWidth`         | `outline-width`         |
| `overflow`             | `overflow`              |
| `padding`              | `padding`               |
| `paddingBottom`        | `padding-bottom`        |
| `paddingLeft`          | `padding-left`          |
| `paddingRight`         | `padding-right`         |
| `paddingTop`           | `padding-top`           |
| `tableLayout`          | `table-layout`          |
| `textAlign`            | `text-align`            |
| `textDecoration`       | `text-decoration`       |
| `textIndent`           | `text-indent`           |
| `textShadow`           | `text-shadow`           |
| `textTransform`        | `text-transform`        |
| `visibility`           | `visibility`            |
| `whitespace`           | `whitespace`            |
| `width`                | `width`                 |
| `wordSpacing`          | `word-spacing`          |
| `zIndex`               | `z-index`               |

### 1.4  DOM中的事件

#### **DOM的事件**

| 名称               | 说明                                                         |
| :----------------- | :----------------------------------------------------------- |
| `blur`             | 在元素失去键盘焦点时触发                                     |
| `click`            | 在按下鼠标按钮后释放时触发                                   |
| `dblclick`         | 在两次按下鼠标按钮并释放时触发                               |
| `focus`            | 在元素获得键盘焦点时触发                                     |
| `focusin`          | 在元素即将获得键盘焦点时触发                                 |
| `focusout`         | 在元素即将失去键盘焦点时触发                                 |
| `keydown`          | 在用户按下某个键时触发                                       |
| `keypress`         | 在用户按下某个键并释放时触发                                 |
| `keyup`            | 在用户释放某个键时触发                                       |
| `mousedown`        | 在鼠标按钮被按下时触发                                       |
| `mouseenter`       | 在光标移入元素或其下属元素所占据的屏幕区域时触发             |
| `mouseleave`       | 在光标移出元素及其所有下属元素所占据的屏幕区域时触发         |
| `mousemove`        | 在光标位于元素上方并移动时触发                               |
| `mouseout`         | 与`mouseleave` 相似，区别是当光标还在下属元素上方时此事件也会被触发 |
| `mouseover`        | 与`mouseenter` 相似，区别是当光标还在下属元素上方时此事件也会被触发 |
| `mouseup`          | 在鼠标按钮被释放时触发                                       |
| `onabort`          | 在文档或资源的加载过程被中止时触发                           |
| `onafterprint`     | 在用户打印文档后触发                                         |
| `onbeforeprint`    | 在调用`Window.print()` 方法之后，向用户呈现打印选项之前触发  |
| `onerror`          | 在文档或资源载入出错时触发                                   |
| `onhashchange`     | 在地址的锚（井号串）部分变动时触发                           |
| `onload`           | 在文档或资源载入完成时触发                                   |
| `onpopstate`       | 触发时会提供一个关联浏览器历史的状态对象。此事件的演示请参见第26章 |
| `onresize`         | 在窗口大小改变时触发                                         |
| `onunload`         | 在文档从窗口或浏览器中卸载时触发                             |
| `readystatechange` | 在`readyState` 属性的值改变时触发                            |
| `reset`            | 在某张表单被重置时触发                                       |
| `submit`           | 在某张表单被提交时触发                                       |

## 2. `Document` 对象

我们通过全局变量`document` 访问`Document` 对象，它是浏览器为我们创建的关键对象之一。`Document` 对象向你提供文档的整体信息，并让你能够访问模型里的各个对象。

示例：

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <meta name="author" content="Adam Freeman"/>
        <meta name="description" content="A simple example"/>
    </head>
    <body>
        <p id="fruittext">
            There are lots of different kinds of fruit - there are over 500
            varieties of <span id="banana">banana</span> alone. By the time we add the
            countless types of apples, oranges, and other well-known fruit, we are faced
            with thousands of choices.
        </p>
        <p id="apples">
            One of the most interesting aspects of fruit is the
            variety available in each country. I live near London, in an area which is
            known for its apples.
        </p>
        <script>
            document.writeln("<pre>URL: " + document.URL);
            var elems = document.getElementsByTagName("p");
            for (var i = 0; i < elems.length; i++) {
                document.writeln("Element ID: " + elems[i].id);
                elems[i].style.border = "medium double black";
                elems[i].style.padding = "4px";
            }
            document.write("</pre>");
        </script>
    </body>
</html>
```

```
document.writeln("<pre>URL: " + document.URL);
```

在这个例子中，我读取了`document.URL` 属性的值，它返回的是当前文档的URL。浏览器就是用这个URL载入此脚本所属文档的。这条语句还调用了`writeln` 方法, 此方法会将内容附加到HTML文档的末尾。

```
var elems = document.getElementsByTagName("p");
```

有许多方法可以用于选择元素。`getElementsByTagName` 选择属于某一给定类型的所有元素，在这个示例中是`p` 元素。任何包含在文档里的`p` 元素都会被该方法返回，并被存放在一个名为`elems` 的变量里。正如之前解释的，所有元素都是由`HTMLElement` 对象代表的，它提供了基本的功能以代表HTML元素。`getElementsByTagName` 方法返回的结果是`HTMLElement` 对象所组成的一个集合。

有了可以处理的`HTMLElement` 对象集合之后，我使用了一个`for` 循环来列举集合里的内容，处理浏览器从HTML文档里找出的各个`p` 元素：

```
for (var i = 0; i < elems.length; i++) {
    document.writeln("Element ID: " + elems[i].id);
    elems[i].style.border = "medium double black";
    elems[i].style.padding = "4px";
}
```

### 2.1  使用`Document` 元数据

#### **`Document` 元数据属性**

| 属性             | 说明                                                       | 返回                |
| :--------------- | :--------------------------------------------------------- | :------------------ |
| `characterSet`   | 返回文档的字符集编码。这是一个只读属性                     | 字符串              |
| `charset`        | 获取或设置文档的字符集编码                                 | 字符串              |
| `compatMode`     | 获取文档的兼容性模式                                       | 字符串              |
| `cookie`         | 获取或设置当前文档的cookie                                 | 字符串              |
| `defaultCharset` | 获取浏览器所使用的默认字符编码                             | 字符串              |
| `defaultView`    | 返回当前文档的`Window` 对象                                | `Window`            |
| `dir`            | 获取或设置文档的文本方向                                   | 字符串              |
| `domain`         | 获取或设置当前文档的域名                                   | 字符串              |
| `implementation` | 提供可用DOM功能的信息                                      | `DOMImplementation` |
| `lastModified`   | 返回文档的最后修改时间（如果修改时间不可用则返回当前时间） | 字符串              |
| `location`       | 提供当前文档的URL信息                                      | `Location`          |
| `readyState`     | 返回当前文档的状态。这是一个只读属性                       | 字符串              |
| `referrer`       | 返回链接到当前文档的文档URL（就是对应HTTP标头的值）        | 字符串              |
| `title`          | 获取或设置当前文档的标题（即`title` 元素的内容）           | 字符串              |

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <meta name="author" content="Adam Freeman"/>
        <meta name="description" content="A simple example"/>
    </head>
    <body>
        <script>
            document.writeln("<pre>");

            document.writeln("characterSet: " + document.characterSet);
            document.writeln("charset: " + document.charset);
            document.writeln("compatMode: " + document.compatMode);
            document.writeln("defaultCharset: " + document.defaultCharset);
            document.writeln("dir: " + document.dir);
            document.writeln("domain: " + document.domain);
            document.writeln("lastModified: " + document.lastModified);
            document.writeln("referrer: " + document.referrer);
            document.writeln("title: " + document.title);

            document.write("</pre>");
        </script>
    </body>
</html>
```

<b>`compatMode` 属性的值</b>

`compatMode` 属性告诉你浏览器是如何处理文档内容的。现如今存在着大量的非标准HTML，浏览器则试图显示出这类网页，哪怕它们并不遵循HTML规范。一些这样的内容依赖于浏览器的独特功能，而这些功能来源于浏览器依靠自身特点（而非遵循标准）进行竞争的年代。`compatMode` 属性会返回两个值中的一个。

| 值           | 说明                                                         |
| :----------- | :----------------------------------------------------------- |
| `CSS1Compat` | 此文档遵循某个有效的HTML规范（但不必是HTML5，有效的HTML4文档也会返回这个值） |
| `BackCompat` | 此文档含有非标准的功能，已触发怪异模式                       |

### 2.2  使用`Location` 对象

`document.location` 属性返回一个`Location` 对象，这个对象给你提供了细粒度的文档地址信息，也允许你导航到其他文档上。

#### **`Location` 对象的方法和属性**

| 属性                | 说明                                    | 返回   |
| :------------------ | :-------------------------------------- | :----- |
| `protocol`          | 获取或设置文档URL的协议部分             | 字符串 |
| `host`              | 获取或设置文档URL的主机和端口部分       | 字符串 |
| `href`              | 获取或设置当前文档的地址                | 字符串 |
| `hostname`          | 获取或设置文档URL的主机名部分           | 字符串 |
| `port`              | 获取或设置文档URL的端口部分             | 字符串 |
| `pathname`          | 获取或设置文档URL的路径部分             | 字符串 |
| `search`            | 获取或设置文档URL的查询（问号串）部分   | 字符串 |
| `hash`              | 获取或设置文档URL的锚（井号串）部分     | 字符串 |
| `assign(<URL>)`     | 导航到指定的URL上                       | `void` |
| `replace(<URL>)`    | 清除当前文档并导航到URL所指定的那个文档 | `void` |
| `reload()`          | 重新载入当前的文档                      | `void` |
| `resolveURL(<URL>)` | 将指定的相对URL解析成绝对URL            | 字符串 |

示例：

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <meta name="author" content="Adam Freeman"/>
        <meta name="description" content="A simple example"/>
    </head>
    <body>
        <script>
            document.writeln("<pre>");

            document.writeln("protocol: " + document.location.protocol);
            document.writeln("host: " + document.location.host);
            document.writeln("hostname: " + document.location.hostname);
            document.writeln("port: " + document.location.port);
            document.writeln("pathname: " + document.location.pathname);
            document.writeln("search: " + document.location.search);
            document.writeln("hash: " + document.location.hash);

            document.write("</pre>");
        </script>
    </bod
```

<b>注意当端口号为HTTP默认的80时，属性不会返回值。</b>

### 2.3  读取和创建cookie

`cookie` 属性让你可以读取、添加和更新文档所关联的cookie。

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <meta name="author" content="Adam Freeman"/>
        <meta name="description" content="A simple example"/>
    </head>
    <body>
        <p id="cookiedata">

        </p>
        <button id="write">Add Cookie</button>
        <button id="update">Update Cookie</button>
        <script>

            var cookieCount = 0;
            document.getElementById("update").onclick = updateCookie;
            document.getElementById("write").onclick = createCookie;
            readCookies();

            function readCookies() {
                document.getElementById("cookiedata").innerHTML = document.cookie;
            }

            function createCookie() {
                cookieCount++;
                document.cookie = "Cookie_" + cookieCount + "=Value_" + cookieCount;
                readCookies();
            }

            function updateCookie() {
                document.cookie = "Cookie_" + cookieCount + "=Updated_" + cookieCount;
                readCookies();
            }
        </script>
    </body>
</html>
```

<b>**cookie的额外字段**</b>

| 额外项              | 说明                                                       |
| :------------------ | :--------------------------------------------------------- |
| `path=<path>`       | 设置cookie关联的路径，如果没有指定则默认使用当前文档的路径 |
| `domain=<domain>`   | 设置cookie关联的域名，如果没有指定则默认使用当前文档的域名 |
| `max|age=<seconds>` | 设置cookie的有效期，以秒的形式从它创建之时起开始计算       |
| `expires=<date>`    | 设置cookie的有效期，用的是GMT格式的日期                    |
| `secure`            | 只有在安全（HTTPS）连接时才会发送cookie                    |

这些额外的项目可以被附加到名称/值对的后面，以分号分隔，就像这样：

```
document.cookie = "MyCookie=MyValue;max-age=10";
```

### 2.4  理解就绪状态

#### `document.readyState` 属性

`document.readyState` 属性向你提供了加载和解析HTML文档过程中当前处于哪个阶段的信息。在默认情况下浏览器会在遇到文档里的`script` 元素时立即开始执行脚本，但你可以使用`defer` 属性推迟脚本的执行。

| 值            | 说明                                                         |
| :------------ | :----------------------------------------------------------- |
| `loading`     | 浏览器正在加载和处理此文档                                   |
| `interactive` | 文档已被解析，但浏览器还在加载其中链接的资源（图像和媒体文件等） |
| `complete`    | 文档已被解析，所有的资源也已加载完毕                         |

示例：

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <meta name="author" content="Adam Freeman"/>
        <meta name="description" content="A simple example"/>
        <script>
            document.onreadystatechange = function() {
                if (document.readyState == "interactive") {
                    document.getElementById("pressme").onclick = function() {
                        document.getElementById("results").innerHTML = "Button Pressed";
                    }
                }
            }
        </script>
    </head>
    <body>
        <button id="pressme">Press Me</button>
        <pre id="results"></pre>
    </body>
</html>
```

### 2.5  获取DOM的实现情况

#### `document.implementation` 属性

`document.implementation` 属性向你提供了浏览器对DOM功能的实现信息。这个属性返回一个`DOMImplementation` 对象，它包含一个`hasFeature` 方法。可以使用这个方法来判断哪些DOM功能已实现。

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <meta name="author" content="Adam Freeman"/>
        <meta name="description" content="A simple example"/>
    </head>
    <body>
        <script>

            var features = ["Core", "HTML", "CSS", "Selectors-API"];
            var levels = ["1.0", "2.0", "3.0"];

            document.writeln("<pre>");
            for (var i = 0; i < features.length; i++) {
                document.writeln("Checking for feature: " + features[i]);
                for (var j = 0; j < levels.length; j++) {
                    document.write(features[i] + " Level " + levels[j] + ": ");
                    document.writeln(document.implementation.hasFeature(features[i],
                        levels[j]));
                }
            }
            document.write("</pre>")
        </script>
    </body>
</html>
```

这段脚本检测了若干不同的DOM功能，以及所定义的功能等级。它并不像看上去那么有用。首先，浏览器并不总是能正确报告它们实现的功能。某些功能实现并不会通过`hasFeature` 方法进行报告，而另一些报告了却根本没有实现。其次，浏览器报告了某项功能并不意味着它的实现方式是有用的。虽然这个问题不如以前严重，但DOM的实现是存在一些差别的。

如果你打算编写能在所有主流浏览器上工作的代码（你也应该这么想），那么`hasFeature` 方法的用处不大。你应该选择在测试阶段全面检查代码，在需要的时候测试支持情况和备用措施，同时也可以考虑使用某个JavaScript库（例如jQuery），它可以帮助消除不同DOM实现之间的差别。

## 3. 获取HTML元素对象

`Document` 对象的一大关键功能是作为一个入口，让你能访问代表文档里各个元素的对象。可以用几种不同的方法来执行这个任务。有些属性会返回代表特定文档元素类型的对象，有些方法能让你很方便地运用条件搜索来找到匹配的元素，还可以将DOM视为一棵树并沿着它的结构进行导航。

### 3.1  **`Document` 对象的元素属性**

| 属性               | 说明                                                       | 返回              |
| :----------------- | :--------------------------------------------------------- | :---------------- |
| `activeElement`    | 返回一个代表当前带有键盘焦点元素的对象                     | `HTMLElement`     |
| `body`             | 返回一个代表`body` 元素的对象                              | `HTMLElement`     |
| `Embeds` `plugins` | 返回所有代表`embed` 元素的对象                             | `HTMLCollection`  |
| `forms`            | 返回所有代表`form` 元素的对象                              | `HTMLCollection`  |
| `head`             | 返回一个代表`head` 元素的对象                              | `HTMLHeadElement` |
| `images`           | 返回所有代表`img` 元素的对象                               | `HTMLCollection`  |
| `links`            | 返回所有代表文档里具备`href` 属性的`a` 和`area` 元素的对象 | `HTMLCollection`  |
| `scripts`          | 返回所有代表`script` 元素的对象                            | `HTMLCollection`  |

<b>使用`HTMLCollection` 对象</b>

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <meta name="author" content="Adam Freeman"/>
        <meta name="description" content="A simple example"/>
        <link rel="shortcut icon" href="favicon.ico" type="image/x-icon" />
        <style>
            pre {border: medium double black;}
        </style>
    </head>
    <body>
        <pre id="results"></pre>
        <img id="lemon" src="lemon.png" alt="lemon"/>
        <p>
            There are lots of different kinds of fruit - there are over 500 varieties
            of banana alone. By the time we add the countless types of apples, oranges,
            and other well-known fruit, we are faced with thousands of choices.
        </p>
        <img id="apple" src="apple.png" alt="apple"/>
        <p>
            One of the most interesting aspects of fruit is the variety available in
            each country. I live near London, in an area which is known for
            its apples.

        </p>
        <img id="banana" src="banana-small.png" alt="small banana"/>
        <script>
            var resultsElement = document.getElementById("results");

            var elems = document.images;

            for (var i = 0; i < elems.length; i++) {
                resultsElement.innerHTML += "Image Element: " + elems[i].id + "\n";
            }

            var srcValue = elems.namedItem("apple").src;
            resultsElement.innerHTML += "Src for apple element is: " + srcValue + "\n";
        </script>
    </body>
</html>
```

第一种使用`HTMLCollection` 对象的方法是将它视为一个数组。它的`length` 属性会返回集合里的项目数量，它还支持使用标准的JavaScript数组索引标记（`element[i]` 这种表示法）来直接访问集合里的各个对象。

第二种方法是使用`namedItem` 方法，它会返回集合里带有指定`id` 或`name` 属性值（如果有的话）的项目。

### 3.2  搜索元素

`Document` 对象定义了许多方法，可以用它们搜索文档里的元素。

| 属性                              | 说明                              | 返回            |
| :-------------------------------- | :-------------------------------- | :-------------- |
| `getElementById(<id>)`            | 返回带有指定`id` 值的元素         | `HTMLElement`   |
| `getElementsByClassName(<class>)` | 返回带有指定`class` 值的元素      | `HTMLElement[]` |
| `getElementsByName(<name>)`       | 返回带有指定`name` 值的元素       | `HTMLElement[]` |
| `getElementsByTagName(<tag>)`     | 返回指定类型的元素                | `HTMLElement[]` |
| `querySelector(<selector>)`       | 返回匹配指定CSS选择器的第一个元素 | `HTMLElement`   |
| `querySelectorAll(<selector>)`    | 返回匹配指定CSS选择器的所有元素   | `HTMLElement[]` |

示例：

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <meta name="author" content="Adam Freeman"/>
        <meta name="description" content="A simple example"/>
        <link rel="shortcut icon" href="favicon.ico" type="image/x-icon" />
        <style>
            pre {border: medium double black;}
        </style>
    </head>
    <body>
        <pre id="results"></pre>
        <img id="lemon" class="fruits" name="apple" src="lemon.png" alt="lemon"/>
        <p>
            There are lots of different kinds of fruit - there are over 500 varieties
            of banana alone. By the time we add the countless types of apples, oranges,
            and other well-known fruit, we are faced with thousands of choices.
        </p>
        <img id="apple" class="fruits images" name="apple"  src="apple.png" alt="apple"/>
        <p>
            One of the most interesting aspects of fruit is the variety available in
            each country. I live near London, in an area which is known for
            its apples.

        </p>
        <img id="banana" src="banana-small.png" alt="small banana"/>
        <script>
            var resultsElement = document.getElementById("results");

            var elems = document.querySelectorAll("p, img#apple")
            resultsElement.innerHTML += "The selector matched " + elems.length
                + " elements\n";
        </script>
    </body>
</html>
```

### 3.3  在DOM树里导航

搜索元素的方法是将DOM视为一棵树，然后在它的层级结构里导航。所有的DOM对象都支持一组属性和方法来让我们做到这一点。

#### **`navigation` 属性和方法**

| 属性              | 说明                             | 返回            |
| :---------------- | :------------------------------- | :-------------- |
| `childNodes`      | 返回子元素组                     | `HTMLElement[]` |
| `firstChild`      | 返回第一个子元素                 | `HTMLElement`   |
| `hasChildNodes()` | 如果当前元素有子元素就返回`true` | 布尔值          |
| `lastChild`       | 返回倒数第一个子元素             | `HTMLElement`   |
| `nextSibling`     | 返回定义在当前元素之后的兄弟元素 | `HTMLElement`   |
| `parentNode`      | 返回父元素                       | `HTMLElement`   |
| `previousSibling` | 返回定义在当前元素之前的兄弟元素 | `HTMLElement`   |

## 4.  使用`Window` 对象

`Window` 对象已经作为HTML5的一部分被添加到HTML规范之中。在此之前，它已经是一种非正式的标准了。各种浏览器都已实现了一组基本相同的功能，并且通常是一致的。在HTML5规范中，`Window` 对象囊括了已是事实标准的功能，并加入了一些增强。

### 4.1  获取`Window` 对象

可以用两种方式获得`Window` 对象。正规的HTML5方式是在`Document` 对象上使用`defaultView` 属性。另一种则是使用所有浏览器都支持的全局变量`window` 。

示例：

```
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
    </head>
    <body id="bod">
        <table>
            <tr><th>outerWidth:</th><td id="owidth"></td></tr>
            <tr><th>outerHeight:</th><td id="oheight"></td></tr>
        </table>

        <script type="text/javascript">
            document.getElementById("owidth").innerHTML = window.outerWidth;
            document.getElementById("oheight").innerHTML
                = document.defaultView.outerHeight;
        </script>
    </body>
</html>
```

### 4.2  获取窗口信息

`Window` 对象的基本功能涉及当前文档所显示的窗口。

| 名称                   | 说明                                                         | 返回     |
| :--------------------- | :----------------------------------------------------------- | :------- |
| `innerHeight`          | 获取窗口内容区域的高度                                       | 数值     |
| `innerWidth`           | 获取窗口内容区域的宽度                                       | 数值     |
| `outerHeight`          | 获取窗口的高度，包括边框和菜单栏等                           | 数值     |
| `outerWidth`           | 获取窗口的宽度，包括边框和菜单栏等                           | 数值     |
| `pageXOffset`          | 获取窗口从左上角算起水平滚动过的像素数                       | 数值     |
| `pageYOffset`          | 获取窗口从左上角算起垂直滚动过的像素数                       | 数值     |
| `screen`               | 返回一个描述屏幕的`Screen` 对象                              | `Screen` |
| `screenLeft` `screenX` | 获取从窗口左边缘到屏幕左边缘的像素数（不是所有浏览器都同时实现了这两个属性，或是以同样的方法计算这个值） | 数值     |
| `screenTop` `screenY`  | 获取从窗口上边缘到屏幕上边缘的像素数（不是所有浏览器都同时实现了这两个属性，或是以同样的方法计算这个值） | 数值     |

示例：

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <style>
            table { border-collapse: collapse; border: thin solid black;}
            th, td { padding: 4px; }
        </style>
    </head>
    <body>
        <table border="1">
            <tr>
                <th>outerWidth:</th><td id="ow"></td><th>outerHeight:</th><td id="oh">
            </tr>
            <tr>
                <th>innerWidth:</th><td id="iw"></td><th>innerHeight:</th><td id="ih">
            </tr>
           <tr>
                <th>screen.width:</th><td id="sw"></td>
                <th>screen.height:</th><td id="sh">
            </tr>
        </table>

        <script type="text/javascript">
            document.getElementById("ow").innerHTML = window.outerWidth;
            document.getElementById("oh").innerHTML = window.outerHeight;
            document.getElementById("iw").innerHTML = window.innerHeight;
            document.getElementById("ih").innerHTML = window.innerHeight;
            document.getElementById("sw").innerHTML = window.screen.width;
            document.getElementById("sh").innerHTML = window.screen.height;
        </script>
    </body>
</html>
```

<b>`Screen` 对象的属性</b>

| 名称          | 说明                                                   | 返回 |
| :------------ | :----------------------------------------------------- | :--- |
| `availHeight` | 屏幕上可供显示窗口部分的高度（排除工具栏和菜单栏之类） | 数值 |
| `availWidth`  | 屏幕上可供显示窗口部分的宽度（排除工具栏和菜单栏之类） | 数值 |
| `colorDepth`  | 屏幕的颜色深度                                         | 数值 |
| `height`      | 屏幕的高度                                             | 数值 |
| `width`       | 屏幕的宽度                                             | 数值 |

### 4.3  与窗口进行交互

`Window` 对象提供了一组方法，可以用它们与包含文档的窗口进行交互。

| 名称                 | 说明                                             | 返回   |
| :------------------- | :----------------------------------------------- | :----- |
| `blur()`             | 让窗口失去键盘焦点                               | `void` |
| `close()`            | 关闭窗口（不是所有浏览器都允许某个脚本关闭窗口） | `void` |
| `focus()`            | 让窗口获得键盘焦点                               | `void` |
| `print()`            | 提示用户打印页面                                 | `void` |
| `scrollBy(<x>, <y>)` | 让文档相对于当前位置进行滚动                     | `void` |
| `scrollTo(<x>, <y>)` | 滚动到指定的位置                                 | `void` |
| `stop()`             | 停止载入文档                                     | `void` |

示例：

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
    </head>
    <body>
        <p>
            <button id="scroll">Scroll</button>
            <button id="print">Print</button>
            <button id="close">Close</button>
        </p>
        <p>
            There are lots of different kinds of fruit - there are over 500 varieties
            of banana alone. By the time we add the countless types of apples, oranges,
            and other well-known fruit, we are faced with thousands of choices.
            <img src="apple.png" alt="apple"/>
            One of the most interesting aspects of fruit is the variety available in
            each country. I live near London, in an area which is known for
            its apples.
            <img src="banana-small.png" alt="banana"/>
            When traveling in Asia, I was struck by how many different
            kinds of banana were available - many of which had unique flavors and
            which were only available within a small region.

            And, of course, there are fruits which are truly unique - I am put in mind
            of the durian, which is widely consumed in SE Asia and is known as the
            "king of fruits." The durian is largely unknown in Europe and the USA - if
            it is known at all, it is for the overwhelming smell, which is compared
            to a combination of almonds, rotten onions and gym socks.
        </p>
        <script>
            var buttons = document.getElementsByTagName("button");
            for (var i = 0; i < buttons.length; i++) {
                buttons[i].onclick = handleButtonPress;
            }

            function handleButtonPress(e) {
                if (e.target.id == "print") {
                    window.print();
                } else if (e.target.id == "close") {
                    window.close();
                } else {
                    window.scrollTo(0, 400);
                }
            }
        </script>
    </body>
</html>
```

### 4.4  对用户进行提示

`Window` 对象包含一组方法，能以不同的方式对用户进行提示。

| 名称                     | 说明                                   | 返回   |
| :----------------------- | :------------------------------------- | :----- |
| `alert(<msg>)`           | 向用户显示一个对话框窗口并等候其被关闭 | `void` |
| `confirm(<msg>)`         | 显示一个带有确认和取消提示的对话框窗口 | 布尔值 |
| `prompt(<msg>, <val>)`   | 显示对话框提示用户输入一个值           | 字符串 |
| `showModalDialog(<url>)` | 弹出一个窗口，显示指定的URL            | `void` |

示例

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <style>
            table { border-collapse: collapse; border: thin solid black;}
            th, td { padding: 4px; }
        </style>
    </head>
    <body>

        <button id="alert">Alert</button>
        <button id="confirm">Confirm</button>
        <button id="prompt">Prompt</button>
        <button id="modal">Modal Dialog</button>

        <script type="text/javascript">

            var buttons = document.getElementsByTagName("button");
            for (var i = 0 ; i < buttons.length; i++) {
                buttons[i].onclick = handleButtonPress;
            }

            function handleButtonPress(e) {
                if (e.target.id == "alert") {
                    window.alert("This is an alert");
                } else if (e.target.id == "confirm") {
                    var confirmed
                       = window.confirm("This is a confirm - do you want to proceed?");
                    alert("Confirmed? " + confirmed);
                } else if (e.target.id == "prompt") {
                    var response = window.prompt("Enter a word", "hello");
                    alert("The word was " + response);
                } else if (e.target.id == "modal") {
                    window.showModalDialog("http://apress.com");
                }
            }
        </script>
    </body>
</html>
```

### 4.5  获取基本信息

`Window` 对象让你能访问某些返回基本信息的对象，这些信息包括当前地址（即文档载入的来源URL）的详情和用户的浏览历史。

| 名称       | 说明                              | 返回       |
| :--------- | :-------------------------------- | :--------- |
| `document` | 返回与此窗口关联的`Document` 对象 | `Document` |
| `history`  | 提供对浏览器历史的访问            | `History`  |
| `location` | 提供当前文档地址的详细信息        | `Location` |

`Window.location` 属性返回的`Location` 对象和`Document.location` 属性返回的相同。

### 4.6  使用浏览器历史

`Window.history` 属性返回一个`History` 对象，你可以用它对浏览器历史进行一些基本的操作。

#### **`History` 对象的属性和方法**

| 名称                                    | 说明                                                         | 返回   |
| :-------------------------------------- | :----------------------------------------------------------- | :----- |
| `back()`                                | 在浏览历史中后退一步                                         | `void` |
| `forward()`                             | 在浏览历史中前进一步                                         | `void` |
| `go(<index>)`                           | 转到相对于当前文档的某个浏览历史位置。正值是前进，负值是后退 | `void` |
| `length`                                | 返回浏览历史中的项目数量                                     | 数值   |
| `pushState(<state>, <title>, <url>)`    | 向浏览器历史添加一个条目                                     | `void` |
| `replaceState(<state>, <title>, <url>)` | 替换浏览器历史中的当前条目                                   | `void` |
| `state`                                 | 返回浏览器历史中关联当前文档的状态数据                       | 对象   |

使用示例：

```
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
    </head>
    <body>
        <button id="back">Back</button>
        <button id="forward">Forward</button>
        <button id="go">Go</button>

        <script type="text/javascript">

            var buttons = document.getElementsByTagName("button");
            for (var i = 0 ; i < buttons.length; i++) {
                buttons[i].onclick = handleButtonPress;
            }

            function handleButtonPress(e) {
                if (e.target.id == "back") {
                    window.history.back();
                } else if (e.target.id == "forward") {
                    window.history.forward();
                } else if (e.target.id == "go") {
                    window.history.go("http://www.apress.com");
                }
            }
        </script>
    </body>
</html>
```

### 4.7  使用计时器

`Window` 对象提供的一个有用功能是可以设置一次性和循环的计时器。这些计时器被用于在预设的时间段后执行某个函数。

| 名称                              | 说明                                            | 返回   |
| :-------------------------------- | :---------------------------------------------- | :----- |
| `clearInterval(<id>)`             | 撤销某个时间间隔计时器                          | `void` |
| `clearTimeout(<id>)`              | 撤销某个超时计时器                              | `void` |
| `setInterval(<function>, <time>)` | 创建一个计时器，每隔`time` 毫秒调用指定的函数   | 整数   |
| `setTimeout(<function>, <time>)`  | 创建一个计时器，等待`time` 毫秒后调用指定的函数 | 整数   |

`setTimeout` 方法创建的计时器只执行一次指定的函数，而`setInterval` 方法创建的计时器会重复执行某个函数。这些方法返回一个唯一的标识符，你可以稍后把它们作为`clearTimeout` 和`clearInterval` 方法的参数来撤销计时器。

示例：

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
    </head>
    <body>
        <p id="msg"></p>
        <p>
            <button id="settime">Set Time</button>
            <button id="cleartime">Clear Time</button>
            <button id="setinterval">Set Interval</button>
            <button id="clearinterval">Clear Interval</button>
        </p>

        <script>
            var buttons = document.getElementsByTagName("button");
            for (var i = 0; i < buttons.length; i++) {
                buttons[i].onclick = handleButtonPress;
            }

            var timeID;
            var intervalID;
            var count = 0;

            function handleButtonPress(e) {
                if (e.target.id == "settime") {
                    timeID = window.setTimeout(function() {
                        displayMsg("Timeout Expired");
                    }, 5000);
                    displayMsg("Timeout Set");
                } else if (e.target.id == "cleartime") {
                    window.clearTimeout(timeID);
                    displayMsg("Timeout Cleared");
                } else if (e.target.id == "setinterval") {
                    intervalID = window.setInterval(function() {
                         displayMsg("Interval expired. Counter: " + count++);
                    }, 2000);
                    displayMsg("Interval Set");
                } else if (e.target.id == "clearinterval") {
                    window.clearInterval(intervalID);
                    displayMsg("Interval Cleared");
                }
            }

            function displayMsg(msg) {
                document.getElementById("msg").innerHTML = msg;
            }

        </script>
    </body>
</html>
```

## 5. 使用DOM元素

### 5.1  使用元素对象

| 属性         | 说明                                | 返回           |
| :----------- | :---------------------------------- | :------------- |
| `checked`    | 获取或设置`checked` 属性是否存在    | 布尔值         |
| `classList`  | 获取或设置元素所属的类列表          | `DOMTokenList` |
| `className`  | 获取或设置元素所属的类列表          | 字符串         |
| `dir`        | 获取或设置`dir` 属性的值            | 字符串         |
| `disabled`   | 获取或设置`disabled` 属性是否存在   | 布尔值         |
| `hidden`     | 获取或设置`hidden` 属性是否存在     | 布尔值         |
| `id`         | 获取或设置`id` 属性的值             | 字符串         |
| `lang`       | 获取或设置`lang` 属性的值           | 字符串         |
| `spellcheck` | 获取或设置`spellcheck` 属性是否存在 | 布尔值         |
| `tabIndex`   | 获取或设置`tabindex` 属性的值       | 数值           |
| `tagName`    | 返回标签名（标识元素类型）          | 字符串         |
| `title`      | 获取或设置`title` 属性的值          | 字符串         |

示例：

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <meta name="author" content="Adam Freeman"/>
        <meta name="description" content="A simple example"/>
        <link rel="shortcut icon" href="favicon.ico" type="image/x-icon" />
        <style>
            p {border: medium double black;}
        </style>
    </head>
    <body>

        <p id="textblock" dir="ltr" lang="en-US">
            There are lots of different  kinds of fruit - there are over 500 varieties
            of <span id="banana">banana</span> alone. By the time we add the countless
            types of <span id="apple">apples</span>,
            <span="orange">oranges</span>, and other well-known fruit, we are
            faced with thousands of choices.
        </p>
        <pre id="results"></pre>
        <script>
            var results = document.getElementById("results");
            var elem = document.getElementById("textblock");

            results.innerHTML += "tag: " + elem.tagName + "\n";
            results.innerHTML += "id: " + elem.id + "\n";
            results.innerHTML += "dir: " + elem.dir + "\n";
            results.innerHTML += "lang: " + elem.lang + "\n";
            results.innerHTML += "hidden: " + elem.hidden + "\n";
            results.innerHTML += "disabled: " + elem.disabled + "\n";
        </script>
    </body>
</html>
```

#### `className` 属性

使用`className` 属性，它会返回一个类的列表。通过改变这个字符串的值，你就能添加或移除类。

使用示例：

```html\
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <meta name="author" content="Adam Freeman"/>
        <meta name="description" content="A simple example"/>
        <link rel="shortcut icon" href="favicon.ico" type="image/x-icon" />
        <style>
            p {
                border: medium double black;
            }
            p.newclass {
                background-color: grey;
                color: white;
            }
        </style>
    </head>
    <body>
        <p id="textblock" class="fruit numbers">
            There are lots of different kinds of fruit - there are over 500 varieties
            of banana alone. By the time we add the countless types of apples, oranges,
            and other well-known fruit, we are faced with thousands of choices.
        </p>
        <button id="pressme">Press Me</button>
        <script>
            document.getElementById("pressme").onclick = function(e) {
                document.getElementById("textblock").className += " newclass";
            };
        </script>
    </body>
</html>
```

#### `classList` 属性

当你想要快速给某个元素添加类时，`className` 属性是易于使用的，但如果你想要做别的事（比如移除一个类），用它就很困难了。幸好，可以使用`classList` 属性，它返回的是一个`DOMTokenList` 对象。

<b>`DOMTokenList` 的成员</b>

| 成员                | 说明                                   | 返回   |
| :------------------ | :------------------------------------- | :----- |
| `add(<class>)`      | 给元素添加指定的类                     | `void` |
| `contains(<class>)` | 如果元素属于指定的类就返回`true`       | 布尔值 |
| `length`            | 返回元素所属类的数量                   | 数值   |
| `remove(<class>)`   | 从元素上移除指定的类                   | `void` |
| `toggle(<class>)`   | 如果类不存在就添加它，如果存在就移除它 | 布尔值 |

使用示例：

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <meta name="author" content="Adam Freeman"/>
        <meta name="description" content="A simple example"/>
        <link rel="shortcut icon" href="favicon.ico" type="image/x-icon" />
        <style>
            p {
                border: medium double black;
            }
            p.newclass {
                background-color: grey;
                color: white;
            }
        </style>
    </head>
    <body>
        <p id="textblock" class="fruit numbers">
            There are lots of different kinds of fruit - there are over 500 varieties
            of banana alone. By the time we add the countless types of apples, oranges,
            and other well-known fruit, we are faced with thousands of choices.
        </p>
        <pre id="results"></pre>
        <button id="toggle">Toggle Class</button>
        <script>
            var results = document.getElementById("results");
            document.getElementById("toggle").onclick = toggleClass;

            listClasses();
            function listClasses() {
                var classlist = document.getElementById("textblock").classList;
                results.innerHTML = "Current classes: "
                for (var i = 0; i < classlist.length; i++) {
                    results.innerHTML += classlist[i] + " ";
                }
            }

            function toggleClass() {
                document.getElementById("textblock").classList.toggle("newclass");
                listClasses();
            }
        </script>
    </body>
</html>
```

### 5.2  使用元素属性

`HTMLElement` 对象既有一些属性来对应最重要的HTML全局属性，又支持对单个元素的任意属性进行读取和设置。

| 成员                            | 说明                               | 返回                 |
| :------------------------------ | :--------------------------------- | :------------------- |
| `attributes`                    | 返回应用到元素上的属性             | `Attr[]`             |
| `dataset`                       | 返回以`data-` 开头的属性           | 字符串数组`[<name>]` |
| `getAttribute(<name>)`          | 返回指定属性的值                   | 字符串               |
| `hasAttribute(<name>)`          | 如果元素带有指定的属性则返回`true` | 布尔值               |
| `removeAttribute(<name>)`       | 从元素上移除指定属性               | `void`               |
| `setAttribute(<name>, <value>)` | 应用一个指定名称和值的属性         | `void`               |

使用示例：

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <meta name="author" content="Adam Freeman"/>
        <meta name="description" content="A simple example"/>
        <link rel="shortcut icon" href="favicon.ico" type="image/x-icon" />
        <style>
            p {border: medium double black;}
        </style>
    </head>
    <body>
        <p id="textblock" class="fruit numbers">
            There are lots of different kinds of fruit - there are over 500 varieties
            of banana alone. By the time we add the countless types of apples, oranges,
            and other well-known fruit, we are faced with thousands of choices.
        </p>
        <pre id="results"></pre>
        <script>
            var results = document.getElementById("results");
            var elem = document.getElementById("textblock");

            results.innerHTML = "Element has lang attribute: "
                + elem.hasAttribute("lang") + "\n";
            results.innerHTML += "Adding lang attribute\n";
            elem.setAttribute("lang", "en-US");
            results.innerHTML += "Attr value is : " + elem.getAttribute("lang") + "\n";
            results.innerHTML += "Set new value for lang attribute\n";
            elem.setAttribute("lang", "en-UK");
            results.innerHTML += "Value is now: " + elem.getAttribute("lang") + "\n";
        </script>
    </body>
</html>
```

<b>`Attr` 对象的属性</b>

| 属性    | 说明               | 返回   |
| :------ | :----------------- | :----- |
| `name`  | 返回属性的名称     | 字符串 |
| `value` | 获取或设置属性的值 | 字符串 |

### 5.3  使用`Text` 对象

元素的文本内容是由`Text` 对象代表的，它在文档模型里表现为元素的子对象。表示**一个元素和其内容的对象之间的关系**。

| 成员                                       | 说明                                                         | 返回   |
| :----------------------------------------- | :----------------------------------------------------------- | :----- |
| `appendData(<string>)`                     | 把指定字符串附加到文本块末尾                                 | `void` |
| `data`                                     | 获取或设置文本                                               | 字符串 |
| `deleteData(<offset>, <count>)`            | 从文本中移除字符串。第一个数字是偏移量，第二个是要移除的字符数量 | `void` |
| `insertData(<offset>, <string>)`           | 在指定偏移量处插入指定字符串                                 | `void` |
| `length`                                   | 返回字符的数量                                               | 数值   |
| `replaceData(<offset>, <count>, <string>)` | 用指定字符串替换一段文本                                     | `void` |
| `replaceWholeText(<string>)`               | 替换全部文本                                                 | `Text` |
| `splitText(<number>)`                      | 将现有的`Text` 元素在指定偏移量处一分为二                    | `Text` |
| `substringData(<offset>, <count>)`         | 返回文本的子串                                               | 字符串 |
| `wholeText`                                | 获取文本                                                     | 字符串 |

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <meta name="author" content="Adam Freeman"/>
        <meta name="description" content="A simple example"/>
        <link rel="shortcut icon" href="favicon.ico" type="image/x-icon" />
        <style>
            p {border: medium double black;}
        </style>
    </head>
    <body>
        <p id="textblock" class="fruit numbers" data-fruit="apple" data-sentiment="like">
            There are lots of different kinds of fruit - there are over <b>500</b>
            varieties of banana alone. By the time we add the countless types of apples,
            oranges, and other well-known fruit, we are faced with thousands of choices.
        </p>
        <button id="pressme">Press Me</button>
        <pre id="results"></pre>
        <script>
            var results = document.getElementById("results");
            var elem = document.getElementById("textblock");

            document.getElementById("pressme").onclick = function() {
                var textElem = elem.firstChild;
                results.innerHTML = "The element has " + textElem.length + " chars\n";
                textElem.replaceWholeText("This is a new string ");
            };
        </script>
    </body>
</html>
```

### 5.4  修改模型

| 成员                                     | 说明                                 | 返回          |
| :--------------------------------------- | :----------------------------------- | :------------ |
| `appendChild(HTMLElement)`               | 将指定元素添加为当前元素的子元素     | `HTMLElement` |
| `cloneNode(boolean)`                     | 复制一个元素                         | `HTMLElement` |
| `compareDocumentPosition(HTMLElement)`   | 判断一个元素的相对位置               | 数值          |
| `innerHTML`                              | 获取或设置元素的内容                 | 字符串        |
| `insertAdjacentHTML(<pos>, <text>)`      | 相对于元素插入HTML                   | `void`        |
| `insertBefore(<newElem>, <childElem>)`   | 在第二个（子）元素之前插入第一个元素 | `HTMLElement` |
| `isEqualNode(<HTMLElement>)`             | 判断指定元素是否与当前元素相同       | 布尔值        |
| `isSameNode(HTMLElement)`                | 判断指定元素是否就是当前元素         | 布尔值        |
| `outerHTML`                              | 获取或设置某个元素的HTML和内容       | 字符串        |
| `removeChild(HTMLElement)`               | 从当前元素上移除指定的子元素         | `HTMLElement` |
| `replaceChild(HTMLElement, HTMLElement)` | 替换当前元素的某个子元素             | `HTMLElement` |

这些属性和方法对所有元素对象都是可用的。另外，`document` 对象定义了两个允许你创建新元素的方法。

<b>DOM操作成员</b>

| 成员                     | 说明                                           | 返回          |
| :----------------------- | :--------------------------------------------- | :------------ |
| `createElement(<tag>)`   | 创建一个属于指定标签类型的新`HTMLElement` 对象 | `HTMLElement` |
| `createTextNode(<text>)` | 创建一个带有指定内容的新`Text` 对象            | `Text`        |

使用示例：

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <meta name="author" content="Adam Freeman"/>
        <meta name="description" content="A simple example"/>
        <link rel="shortcut icon" href="favicon.ico" type="image/x-icon" />
        <style>
            table {
                border: solid thin black;
                border-collapse: collapse;
                margin: 10px;
            }
            td { padding: 4px 5px; }
        </style>
    </head>
    <body>
        <table border="1">
            <thead><th>Name</th><th>Color</th></thead>
            <tbody id="fruitsBody">
                <tr><td>Banana</td><td>Yellow</td></tr>
                <tr><td>Apple</td><td>Red/Green</td></tr>
            </tbody>
        </table>

        <button id="add">Add Element</button>
        <button id="remove">Remove Element</button>

        <script>
            var tableBody = document.getElementById("fruitsBody");

            document.getElementById("add").onclick = function() {
              var row = tableBody.appendChild(document.createElement("tr"));
              row.setAttribute("id", "newrow");
              row.appendChild(document.createElement("td"))
                .appendChild(document.createTextNode("Plum"));
              row.appendChild(document.createElement("td"))
                .appendChild(document.createTextNode("Purple"));
            };

            document.getElementById("remove").onclick = function() {
                var row = document.getElementById("newrow");
                row.parentNode.removeChild(row);
            }
        </script>
    </body>
</html>
```

<b>复制元素</b>

```
<script>
            var tableBody = document.getElementById("fruitsBody");

            document.getElementById("add").onclick = function() {
                var count = tableBody.getElementsByTagName("tr").length + 1;

                var newElem = tableBody.getElementsByTagName("tr")[0].cloneNode(true);
                newElem.getElementsByClassName("sum")[0].firstChild.data = count
                    + " * " + count;
                newElem.getElementsByClassName("result")[0].firstChild.data =
                    count * count;

                tableBody.appendChild(newElem);
            };
</script>
```

<b>移动元素</b>

```
 <script>
            document.getElementById("move").onclick = function() {
                var elem = document.getElementById("apple");
                document.getElementById("fruitsBody").appendChild(elem);
            };
 </script>
```

### 5.5  使用HTML片段

`innerHTML` 属性，`outerHTML` 属性和`insertAdjacentHTML` 方法都是便利的语法捷径，它们让你能够使用HTML片段，从而不再需要创建元素和文本对象的详细层级结构。`outerHTML` 属性返回一个字符串，它包含定义这个元素及其所有子元素的HTML。`innerHTML` 属性则只返回子元素的HTML。

使用示例：

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <meta name="author" content="Adam Freeman"/>
        <meta name="description" content="A simple example"/>
        <link rel="shortcut icon" href="favicon.ico" type="image/x-icon" />
        <style>
            table {
                border: solid thin black;
                border-collapse: collapse;
                margin: 5px 2px;
                float: left;
            }
            td { padding: 4px 5px; }
            p {clear: left};
        </style>
    </head>
    <body>
        <table border="1">
            <thead><tr><th>Fruit</th><th>Color</th></tr></thead>
            <tbody>
                <tr id="applerow"><td>Plum</td><td>Purple</td></tr>
            </tbody>
        </table>
        <textarea  rows="3" id="results"></textarea>
        <p>
            <button id="inner">Inner HTML</button>
            <button id="outer">Outer HTML</button>
        </p>
        <script>
            var results = document.getElementById("results");
            var row = document.getElementById("applerow");

            document.getElementById("inner").onclick = function() {
                results.innerHTML = row.innerHTML;
            };

            document.getElementById("outer").onclick = function() {
                results.innerHTML = row.outerHTML;
            }
        </script>
    </body>
</html>
```

### 5.6  向文本块插入元素

修改模型的另一种重要方式是向由`Text` 对象代表的文本块添加元素。

使用示例：

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <meta name="author" content="Adam Freeman"/>
        <meta name="description" content="A simple example"/>
        <link rel="shortcut icon" href="favicon.ico" type="image/x-icon" />
    </head>
    <body>
        <p id="textblock">There are lots of different kinds of fruit - there are over
            500 varieties of banana alone. By the time we add the countless types of
            apples, oranges, and other well-known fruit, we are faced with thousands of
            choices.
        </p>
        <p>
            <button id="insert">Insert Element</button>
        </p>
        <script>
            document.getElementById("insert").onclick = function() {
                var textBlock = document.getElementById("textblock");
                textBlock.firstChild.splitText(10);
                var newText = textBlock.childNodes[1].splitText(4).previousSibling;
                textBlock.insertBefore(document.createElement("b"),
                    newText).appendChild(newText);
            }
        </script>
    </body>
</html>
```

## 6. 为DOM元素设置样式

### 6.1  使用样式表

通过`document.styleSheets` 属性访问文档中可用的CSS样式表，它会返回一组对象集合，这些对象代表了与文档关联的各个样式表。

| 属性                   | 说明             | 返回              |
| :--------------------- | :--------------- | :---------------- |
| `document.stylesheets` | 返回样式表的集合 | `CSSStyleSheet[]` |

每个样式表都由一个`CSSStyleSheet` 对象代表，它提供了一组属性和方法来操作文档里的样式。

<b>`CSSStyleSheet` 对象的成员</b>

| 成员                        | 说明                             | 返回          |
| :-------------------------- | :------------------------------- | :------------ |
| `cssRules`                  | 返回样式表的规则集合             | `CSSRuleList` |
| `deleteRule(<pos>)`         | 从样式表中移除一条规则           | `void`        |
| `disabled`                  | 获取或设置样式表的禁用状态       | 布尔值        |
| `href`                      | 返回链接样式表的`href`           | 字符串        |
| `insertRule(<rule>, <pos>)` | 插入一条新规则到样式表中         | 数值          |
| `media`                     | 返回应用到样式表上的媒介限制集合 | `MediaList`   |
| `ownerNode`                 | 返回样式所定义的元素             | `HTMLElement` |
| `title`                     | 返回`title` 属性的值             | 字符串        |
| `type`                      | 返回`type` 属性的值              | 字符串        |

示例：

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <meta name="author" content="Adam Freeman"/>
        <meta name="description" content="A simple example"/>
        <link rel="shortcut icon" href="favicon.ico" type="image/x-icon" />
        <style title="core styles">
            p {
                border: medium double black;
                background-color: lightgray;
            }
            #block1 { color: white;}
            table {border: thin solid black; border-collapse: collapse;
                    margin: 5px; float: left;}
                    td {padding: 2px;}
        </style>
        <link rel="stylesheet" type="text/css" href="styles.css"/>
        <style media="screen AND (min-width:500px)" type="text/css">
            #block2 {color:yellow; font-style:italic}
        </style>
    </head>
    <body>
        <p id="block1">There are lots of different kinds of fruit - there are over
            500 varieties of banana alone. By the time we add the countless types of
            apples, oranges, and other well-known fruit, we are faced with thousands of
            choices.
        </p>
        <p id="block2">
            One of the most interesting aspects of fruit is the variety available in
            each country. I live near London, in an area which is known for
            its apples.
        </p>
        <div id="placeholder"/>
        <script>
            var placeholder = document.getElementById("placeholder");
            var sheets = document.styleSheets;

            for (var i = 0; i < sheets.length; i++) {
                var newElem = document.createElement("table");
                newElem.setAttribute("border", "1");
                addRow(newElem, "Index", i);
                addRow(newElem, "href", sheets[i].href);
                addRow(newElem, "title", sheets[i].title);
                addRow(newElem, "type", sheets[i].type);
                addRow(newElem, "ownerNode", sheets[i].ownerNode.tagName);
                placeholder.appendChild(newElem);
            }

            function addRow(elem, header, value) {
                elem.innerHTML += "<tr><td>" + header + ":</td><td>"
                    +  value + "</td></tr>";
            }
        </script>
    </body>
</html>
```

### 6.2  使用媒介限制

当定义样式表时可以使用`media` 属性来限制样式应用的场合。可以使用`CSSStyleSheet.media` 属性访问这些限制，它会返回一个`MediaList` 对象。

**`MediaList` 对象的成员**

| 成员                     | 说明                     | 返回   |
| :----------------------- | :----------------------- | :----- |
| `appendMedium(<medium>)` | 添加一个新媒介到列表中   | `void` |
| `deleteMedium(<medium>)` | 从列表中移除一个媒介     | `void` |
| `item(<pos>)`            | 返回指定索引的媒介       | 字符串 |
| `length`                 | 返回媒介的数量           | 数值   |
| `mediaText`              | 返回`media` 属性的文本值 | 字符串 |

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <meta name="author" content="Adam Freeman"/>
        <meta name="description" content="A simple example"/>
        <link rel="shortcut icon" href="favicon.ico" type="image/x-icon" />
        <style title="core styles">
            p {
                border: medium double black;
                background-color: lightgray;
            }
            #block1 { color: white;}
            table {border: thin solid black; border-collapse: collapse;
                    margin: 5px; float: left;}
                    td {padding: 2px;}
        </style>
        <link rel="stylesheet" type="text/css" href="styles.css"/>
        <style media="screen AND (min-width:500px), PRINT" type="text/css">
            #block2 {color:yellow; font-style:italic}
        </style>
    </head>
    <body>
        <p id="block1">There are lots of different kinds of fruit - there are over
            500 varieties of banana alone. By the time we add the countless types of
            apples, oranges, and other well-known fruit, we are faced with thousands of
            choices.
        </p>
        <p id="block2">
            One of the most interesting aspects of fruit is the variety available in
            each country. I live near London, in an area which is known for
            its apples.
        </p>
        <div id="placeholder"/>
        <script>
            var placeholder = document.getElementById("placeholder");
            var sheets = document.styleSheets;

            for (var i = 0; i < sheets.length; i++) {
                if (sheets[i].media.length > 0) {
                    var newElem = document.createElement("table");
                    newElem.setAttribute("border", "1");
                    addRow(newElem, "Media Count", sheets[i].media.length);
                    addRow(newElem, "Media Text", sheets[i].media.mediaText);
                    for (var j =0; j < sheets[i].media.length; j++) {
                        addRow(newElem, "Media " + j, sheets[i].media.item(j));

                    }
                    placeholder.appendChild(newElem);
                }
            }

            function addRow(elem, header, value) {
                elem.innerHTML += "<tr><td>" + header + ":</td><td>"
                    +  value + "</td></tr>";
            }
        </script>
    </body>
</html>
```

### 6.3  禁用样式表

`CSSStyleSheet.disabled` 属性可用来一次性启用和禁用某个样式表里的所有样式。

示例：

```
 <script>
     document.getElementById("pressme").onclick = function() {
     document.styleSheets[0].disabled = !document.styleSheets[0].disabled;
     }
 </script>
```

### 6.4  `CSSRuleList` 对象的成员

`CSSStyleSheet.cssRules` 属性会返回一个`CSSRuleList` 对象，它允许你访问样式表里的各种样式。

**`CSSRuleList` 对象的成员**

| 成员          | 说明                   | 返回           |
| :------------ | :--------------------- | :------------- |
| `item(<pos>)` | 返回指定索引的CSS样式  | `CSSStyleRule` |
| `length`      | 返回样式表里的样式数量 | 数值           |

**`CSSStyleRule` 对象的成员**

| 成员               | 说明                               | 返回                  |
| :----------------- | :--------------------------------- | :-------------------- |
| `cssText`          | 获取或设置样式的文本（包括选择器） | 字符串                |
| `parentStyleSheet` | 获取此样式所属的样式表             | `CSSStyleSheet`       |
| `selectorText`     | 获取或设置样式的选择器文本         | 字符串                |
| `style`            | 获取一个代表具体样式属性的对象     | `CSSStyleDeclaration` |

使用示例：

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <meta name="author" content="Adam Freeman"/>
        <meta name="description" content="A simple example"/>
        <link rel="shortcut icon" href="favicon.ico" type="image/x-icon" />
        <style title="core styles">
            p {
                border: medium double black;
                background-color: lightgray;
            }
            #block1 { color: white; border: thick solid black; background-color: gray;}
            table {border: thin solid black; border-collapse: collapse;
                    margin: 5px; float: left;}
            td {padding: 2px;}
        </style>
    </head>
    <body>
        <p id="block1">There are lots of different kinds of fruit - there are over
            500 varieties of banana alone. By the time we add the countless types of
            apples, oranges, and other well-known fruit, we are faced with thousands of
            choices.
        </p>
        <p id="block2">
            One of the most interesting aspects of fruit is the variety available in
            each country. I live near London, in an area which is known for
            its apples.
        </p>
        <div><button id="pressme">Press Me </button></div>
        <div id="placeholder"></div>
        <script>
            var placeholder = document.getElementById("placeholder");
            processStyleSheet();

            document.getElementById("pressme").onclick = function() {

                document.styleSheets[0].cssRules.item(1).selectorText = "#block2";

                if (placeholder.hasChildNodes()) {
                    var childCount = placeholder.childNodes.length;
                    for (var i = 0; i < childCount; i++) {
                        placeholder.removeChild(placeholder.firstChild);
                    }
                }
                processStyleSheet();
            }

            function processStyleSheet() {
                var rulesList = document.styleSheets[0].cssRules;

                for (var i = 0; i < rulesList.length; i++) {
                    var rule = rulesList.item(i);

                    var newElem = document.createElement("table");
                    newElem.setAttribute("border", "1");

                    addRow(newElem, "parentStyleSheet", rule.parentStyleSheet.title);
                    addRow(newElem, "selectorText", rule.selectorText);
                    addRow(newElem, "cssText", rule.cssText);
                    placeholder.appendChild(newElem);
                }
            }

            function addRow(elem, header, value) {
                elem.innerHTML += "<tr><td>" + header + ":</td><td>"
                    +  value + "</td></tr>";
            }
        </script>
    </body>
</html>
```

### 6.5  使用元素样式

要获取某个元素的`style` 属性所定义的样式属性，需要读取`HTMLElement` 对象里定义的`style` 属性值。`style` 属性会返回一个`CSSStyleDeclaration` 对象，它和你通过样式表所获取的对象属于同一类型。

**`CSSStyleDeclaration` 对象的成员**

| 成员                                       | 说明                            | 返回                |
| :----------------------------------------- | :------------------------------ | :------------------ |
| `cssText`                                  | 获取或设置样式的文本            | 字符串              |
| `getPropertyCSSValue(<name>)`              | 获取指定的属性                  | `CSSPrimitiveValue` |
| `getPropertyPriority(<name>)`              | 获取指定属性的优先级            | 字符串              |
| `getPropertyValue(<name>)`                 | 获取字符串形式的指定值          | 字符串              |
| `item(<pos>)`                              | 获取指定位置的项目              | 字符串              |
| `length`                                   | 获取项目的数量                  | 数值                |
| `parentRule`                               | 如果存在样式规则就获取它        | `CSSStyleRule`      |
| `removeProperty(<name>)`                   | 移除指定的属性                  | 字符串              |
| `setProperty(<name>, <value>, <priority>)` | 设置指定属性的值和优先级        | `void`              |
| `<style>`                                  | 获取或设置指定CSS属性的便捷属性 | 字符串              |

## 7. 事件

### 7.1  简单事件处理器

#### 内联事件处理器

使用某个事件属性最直接的方式是给它指派一组JavaScript语句。当该事件被触发后，浏览器就会执行你提供的语句。

```
<p onmouseover="this.style.background='white'; this.style.color='black'">
    There are lots of different kinds of fruit - there are over
    500 varieties of banana alone. By the time we add the countless types of
    apples, oranges, and other well-known fruit, we are faced with thousands of
    choices.
</p>
```

#### **处理`MouseOver` 事件**

这种转变是单向的：当鼠标离开元素的屏幕区域时样式不会重置。许多事件是成双成对的。与`mouseover` 相对的事件被称为`mouseout` 。

```
<p onmouseover="this.style.background='white'; this.style.color='black'">
    There are lots of different kinds of fruit - there are over
    500 varieties of banana alone. By the time we add the countless types of
    apples, oranges, and other well-known fruit, we are faced with thousands of
    choices.
</p>
```

#### 处理MouseOut事件

与`mouseover` 相对。

```
<p onmouseover="this.style.background='white'; this.style.color='black'"
    onmouseout="this.style.removeProperty('color');
    this.style.removeProperty('background')">
    There are lots of different kinds of fruit - there are over
    500 varieties of banana alone. By the time we add the countless types of
    apples, oranges, and other well-known fruit, we are faced with thousands of
    choices.
</p>
```

使用示例：

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <style type="text/css">
            p {
                background: gray;
                color:white;
                padding: 10px;
                margin: 5px;
                border: thin solid black
            }
        </style>
        <script type="text/javascript">
            function handleMouseOver(elem) {
                elem.style.background='white';
                elem.style.color='black';
            }

            function handleMouseOut(elem) {
                elem.style.removeProperty('color');
                elem.style.removeProperty('background');
            }
        </script>
    </head>
    <body>
        <p onmouseover="handleMouseOver(this)" onmouseout="handleMouseOut(this)">
            There are lots of different kinds of fruit - there are over
            500 varieties of banana alone. By the time we add the countless types of
            apples, oranges, and other well-known fruit, we are faced with thousands of
            choices.
        </p>
        <p onmouseover="handleMouseOver(this)" onmouseout="handleMouseOut(this)">
            One of the most interesting aspects of fruit is the variety available in
            each country. I live near London, in an area which is known for
            its apples.
        </p>
    </body>
</html>
```

### 7.2  使用DOM和事件对象

使用示例：

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <style type="text/css">
            p {
                background: gray;
                color:white;
                padding: 10px;
                margin: 5px;
                border: thin solid black
            }
        </style>
    </head>
    <body>
        <p>
            There are lots of different kinds of fruit - there are over
            500 varieties of banana alone. By the time we add the countless types of
            apples, oranges, and other well-known fruit, we are faced with thousands of
            choices.
        </p>
        <p>
            One of the most interesting aspects of fruit is the variety available in
            each country. I live near London, in an area which is known for
            its apples.
        </p>
        <script type="text/javascript">

            var pElems = document.getElementsByTagName("p");
            for (var i = 0; i < pElems.length; i++) {
                pElems[i].onmouseover = handleMouseOver;
                pElems[i].onmouseout = handleMouseOut;
            }

            function handleMouseOver(e) {
                e.target.style.background='white';
                e.target.style.color='black';
            }

            function handleMouseOut(e) {
                e.target.style.removeProperty('color');
                e.target.style.removeProperty('background');
            }
        </script>
    </body>
</html>
```

#### `addEventListener` 和`removeEventListener` 方法

```
 <script type="text/javascript">

            var pElems = document.getElementsByTagName("p");
            for (var i = 0; i < pElems.length; i++) {
                pElems[i].addEventListener("mouseover", handleMouseOver);
                pElems[i].addEventListener("mouseout", handleMouseOut);
            }

            document.getElementById("pressme").onclick = function() {
                document.getElementById("block2").removeEventListener("mouseout",
                    handleMouseOut);
            }

            function handleMouseOver(e) {
                e.target.style.background='white';
                e.target.style.color='black';
            }

            function handleMouseOut(e) {
                e.target.style.removeProperty('color');
                e.target.style.removeProperty('background');
            }
 </script>
```

#### **`Event` 对象的函数和属性**

| 名称                         | 说明                                                         | 返回          |
| :--------------------------- | :----------------------------------------------------------- | :------------ |
| `type`                       | 事件的名称（如`mouseover` ）                                 | 字符串        |
| `target`                     | 事件指向的元素                                               | `HTMLElement` |
| `currentTarget`              | 带有当前被触发事件监听器的元素                               | `HTMLElement` |
| `eventPhase`                 | 事件生命周期的阶段                                           | 数值          |
| `bubbles`                    | 如果事件会在文档里冒泡则返回`true` ，否则返回`false`         | 布尔值        |
| `cancelable`                 | 如果事件带有可撤销的默认行为则返回`true` ，否则返回`false`   | 布尔值        |
| `timeStamp`                  | 事件的创建时间，如果时间不可用则为0                          | 字符串        |
| `stopPropagation()`          | 在当前元素的事件监听器被触发后终止事件在元素树中的流动       | `void`        |
| `stopImmediatePropagation()` | 立即终止事件在元素树中的流动。当前元素上未被触发的事件监听器会被忽略 | `void`        |
| `preventDefault()`           | 防止浏览器执行与事件关联的默认操作                           | `void`        |
| `defaultPrevented`           | 如果调用过`preventDefault()` 则返回`true`                    | 布尔值        |

### 7.3  按类型区分事件

`type` 属性会告诉你正在处理的是哪种类型的事件。

使用示例：

```
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <style type="text/css">
            p {
                background: gray;
                color:white;
                padding: 10px;
                margin: 5px;
                border: thin solid black
            }
        </style>
    </head>
    <body>
        <p>
            There are lots of different kinds of fruit - there are over
            500 varieties of banana alone. By the time we add the countless types of
            apples, oranges, and other well-known fruit, we are faced with thousands of
            choices.
        </p>
        <p id="block2">
            One of the most interesting aspects of fruit is the variety available in
            each country. I live near London, in an area which is known for
            its apples.
        </p>
        <script type="text/javascript">

            var pElems = document.getElementsByTagName("p");
            for (var i = 0; i < pElems.length; i++) {
                pElems[i].onmouseover = handleMouseEvent;
                pElems[i].onmouseout = handleMouseEvent;
            }

            function handleMouseEvent(e) {
                if (e.type == "mouseover") {
                    e.target.style.background='white';
                    e.target.style.color='black';
                } else {
                    e.target.style.removeProperty('color');
                    e.target.style.removeProperty('background');
                }
            }
        </script>
    </body>
</html>
```

### 7.4  理解事件流

一个事件的生命周期包括三个阶段：**捕捉** （capture）、**目标** （target）和**冒泡** （bubbling）

**`Event.eventPhase` 属性的值**

| 名称              | 说明               |
| :---------------- | :----------------- |
| `CAPTURING_PHASE` | 此事件处于捕捉阶段 |
| `AT_TARGET`       | 此事件处于目标阶段 |
| `BUBBLING_PHASE`  | 此事件处于冒泡阶段 |

#### **捕捉阶段**

当某个事件被触发时，浏览器会找出事件涉及的元素，它被称为该事件的**目标** 。浏览器会找出`body` 元素和目标之间的所有元素并分别检查它们，看看它们是否带有事件处理器且要求获得其后代元素触发事件的通知。浏览器会先触发这些事件处理器，然后才会轮到目标自身的处理器。

```
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <style type="text/css">
            p {
                background: gray;
                color:white;
                padding: 10px;
                margin: 5px;
                border: thin solid black
            }
            span {
                background: white;
                color: black;
                padding: 2px;
                cursor: default;
            }
        </style>
    </head>
    <body>
        <p id="block1">
            There are lots of different kinds of fruit - there are over
            500 varieties of <span id="banana">banana</span> alone. By the time we add
            the countless types of apples, oranges, and other well-known fruit, we are
            faced with thousands of choices.
        </p>
        <script type="text/javascript">

            var banana = document.getElementById("banana");
            var textblock = document.getElementById("block1");

            banana.addEventListener("mouseover", handleMouseEvent);
            banana.addEventListener("mouseout", handleMouseEvent);
            textblock.addEventListener("mouseover", handleDescendantEvent, true);
            textblock.addEventListener("mouseout", handleDescendantEvent, true);

            function handleDescendantEvent(e) {
              if (e.type == "mouseover" && e.eventPhase == Event.CAPTURING_PHASE) {
                    e.target.style.border = "thick solid red";
                    e.currentTarget.style.border = "thick double black";
              } else if (e.type == "mouseout" && e.eventPhase == Event.CAPTURING_PHASE) {
                    e.target.style.removeProperty("border");
                    e.currentTarget.style.removeProperty("border");
              }
            }

            function handleMouseEvent(e) {
                if (e.type == "mouseover") {
                    e.target.style.background='white';
                    e.target.style.color='black';
                } else {
                    e.target.style.removeProperty('color');
                    e.target.style.removeProperty('background');
                }
            }
        </script>
    </body>
</html>
```

这个例子中，定义了一个`span` 作为`p` 元素的子元素，然后注册了`mouseover` 和`mouseout` 事件的处理器。请注意当我注册父元素（即`p` 元素）时，我给`addEventListener` 方法添加了第三个参数，就像这样：

```
textblock.addEventListener("mouseover", handleDescendantEvent, true);
```

这个额外的参数告诉浏览器我想让`p` 元素在捕捉阶段接收后代元素的事件。当`mouseover` 事件被触发时，浏览器会从HTML文档的根节点起步，一路沿着DOM向目标（也就是触发事件的元素）前进。

对每一个元素，浏览器都会调用它所有启用捕捉的监听器。在这个例子中，浏览器会找到并调用注册在`p` 元素上的`handleDescendantEvent` 函数。当`handleDescendantEvent` 函数被调用时，`Event` 对象包含了目标元素的信息（通过`target` 属性），还有导致函数被调用的元素（通过`currentTarget` 属性）。我同时使用了这两个属性，这样就能修改`p` 元素和其子元素`span` 的样式了。

事件捕捉让目标元素的各个上级元素都有机会在事件传递到目标元素本身之前对其作出反应。上级元素的事件处理器可以阻止事件流向目标，方法是对`Event` 对象调用`stopPropagation` 或`stopImmediatePropagation` 函数。这两个函数的区别在于，`stopPropagation` 会确保调用当前元素上注册的所有事件监听器，而`stopImmediatePropagation` 会忽略任何未触发的监听器。

```
...
function handleDescendantEvent(e) {
    if (e.type == "mouseover" && e.eventPhase == Event.CAPTURING_PHASE) {
        e.target.style.border = "thick solid red";
        e.currentTarget.style.border = "thick double black";
    } else if (e.type == "mouseout" && e.eventPhase == Event.CAPTURING_PHASE) {
        e.target.style.removeProperty("border");
        e.currentTarget.style.removeProperty("border");
    }
    e.stopPropagation();
}
...
```

做了这个改动之后，浏览器的捕捉阶段就会在`p` 元素上的处理器被调用时结束。浏览器不会再检查其他任何元素，并且会跳过目标和冒泡阶段。

#### **目标阶段**

目标阶段是三个阶段中最简单的。当捕捉阶段完成后，浏览器会触发目标元素上任何已添加的事件类型监听器。

如果在目标阶段调用`stopPropagation` 或`stopImmediatePropagation` 函数，相当于终止了事件流，不再进入冒泡阶段。

#### **冒泡阶段**

完成目标阶段之后，浏览器开始转而沿着上级元素链朝`body` 元素前进。在沿途的每个元素上，浏览器都会检查是否存在针对该事件类型但没有启用捕捉的监听器（也就是说，`addEventListener` 函数的第三个参数是`false` ）。这就是所谓的**事件冒泡** 。

示例：

```
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <style type="text/css">
            p {
                background: gray;
                color:white;
                padding: 10px;
                margin: 5px;
                border: thin solid black
            }
            span {
                background: white;
                color: black;
                padding: 2px;
                cursor: default;
            }
        </style>
    </head>
    <body>
        <p id="block1">
            There are lots of different kinds of fruit - there are over
            500 varieties of <span id="banana">banana</span> alone. By the time we add
            the countless types of apples, oranges, and other well-known fruit, we are
            faced with thousands of choices.
        </p>
        <script type="text/javascript">

            var banana = document.getElementById("banana");
            var textblock = document.getElementById("block1");

            banana.addEventListener("mouseover", handleMouseEvent);
            banana.addEventListener("mouseout", handleMouseEvent);
            textblock.addEventListener("mouseover", handleDescendantEvent, true);
            textblock.addEventListener("mouseout", handleDescendantEvent, true);
            textblock.addEventListener("mouseover", handleBubbleMouseEvent, false);
            textblock.addEventListener("mouseout", handleBubbleMouseEvent, false);

            function handleBubbleMouseEvent(e) {
               if (e.type == "mouseover" && e.eventPhase == Event.BUBBLING_PHASE) {
                    e.target.style.textTransform = "uppercase";
               } else if (e.type == "mouseout" && e.eventPhase == Event.BUBBLING_PHASE) {
                    e.target.style.textTransform = "none";
               }
            }

            function handleDescendantEvent(e) {
              if (e.type == "mouseover" && e.eventPhase == Event.CAPTURING_PHASE) {
                    e.target.style.border = "thick solid red";
                    e.currentTarget.style.border = "thick double black";
              } else if (e.type == "mouseout" && e.eventPhase == Event.CAPTURING_PHASE) {
                    e.target.style.removeProperty("border");
                    e.currentTarget.style.removeProperty("border");
              }
            }

            function handleMouseEvent(e) {
                if (e.type == "mouseover") {
                    e.target.style.background='black';
                    e.target.style.color='white';
                } else {
                    e.target.style.removeProperty('color');
                    e.target.style.removeProperty('background');
                }
            }
        </script>
    </body>
</html>
```

添加了一个名为`handleBubbleMouseEvent` 的新函数，并把它附到文档的`p` 元素上。现在`p` 元素就有了两个事件监听器，一个启用了捕捉，另一个启用了冒泡。当使用`addEventListener` 方法时，你始终都处于这两种状态中的一种，这就意味着某个元素的监听器**除了自身事件的通知** ，还会收到后代元素事件的通知。你要选择的是在后代元素事件的目标阶段之前还是之后调用监听器。

这些新代码带来的结果是，文档里的`span` 元素在发生`mouseover` 事件时会触发三个监听函数。`handleDescendantEvent` 函数会在捕捉阶段被触发，`handleMouseEvent` 函数会在目标阶段被调用，而`handleBubbleMouseEvent` 则是在冒泡阶段。

### 7.5  使用HTML事件

#### 文档和窗口事件

| 名称               | 说明                                  |
| :----------------- | :------------------------------------ |
| `readystatechange` | 在`readyState` 属性的值发生变化时触发 |

#### **`Window` 对象的事件**

| 名称            | 说明                                                         |
| :-------------- | :----------------------------------------------------------- |
| `onabort`       | 在文档或资源加载过程被中止时触发                             |
| `onafterprint`  | 在已调用`Window.print()` 方法，但尚未给用户提供打印选项时触发 |
| `onbeforeprint` | 在用户完成文档打印后触发                                     |
| `onerror`       | 在文档或资源的加载发生错误时触发                             |
| `onhashchange`  | 在锚部分发生变化时触发                                       |
| `onload`        | 在文档或资源加载完成时触发                                   |
| `onpopstate`    | 触发后提供一个关联浏览器历史的状态对象                       |
| `onresize`      | 在窗口缩放时触发                                             |
| `onunload`      | 在文档从窗口或浏览器中卸载时触发                             |

### 7.6  使用鼠标事件

#### **与鼠标相关的事件**

| 名称         | 说明                                                         |
| :----------- | :----------------------------------------------------------- |
| `click`      | 在点击并释放鼠标键时触发                                     |
| `dblclick`   | 在两次点击并释放鼠标键时触发                                 |
| `mousedown`  | 在点击鼠标键时触发                                           |
| `mouseenter` | 在光标移入元素或某个后代元素所占据的屏幕区域时触发           |
| `mouseleave` | 在光标移出元素及所有后代元素所占据的屏幕区域时触发           |
| `mousemove`  | 当光标在元素上移动时触发                                     |
| `mouseout`   | 与`mouseleave` 基本相同，除了当光标仍然在某个后代元素上时也会触发 |
| `mouseover`  | 与`mouseenter` 基本相同，除了当光标仍然在某个后代元素上时也会触发 |
| `mouseup`    | 在释放鼠标键时触发                                           |

当某个鼠标事件被触发时，浏览器会指派一个`MouseEvent` 对象。它是一个`Event` 对象

**`MouseEvent` 对象**

| 名称       | 说明                                                   | 返回   |
| :--------- | :----------------------------------------------------- | :----- |
| `button`   | 标明点击的是哪个键。0是鼠标主键，1是中键，2是次键/右键 | 数值   |
| `altKey`   | 如果在事件触发时按下了`alt/option` 键则返回`true`      | 布尔值 |
| `clientX`  | 返回事件触发时鼠标相对于元素视口的X坐标                | 数值   |
| `clientY`  | 返回事件触发时鼠标相对于元素视口的Y坐标                | 数值   |
| `screenX`  | 返回事件触发时鼠标相对于屏幕坐标系的X坐标              | 数值   |
| `screenY`  | 返回事件触发时鼠标相对于屏幕坐标系的Y坐标              | 数值   |
| `shiftKey` | 如果在事件触发时按下了Shift键则返回`true`              | 布尔值 |
| `ctrlKey`  | 如果在事件触发时按下了Ctrl键则返回`true`               | 布尔值 |

```
 <script type="text/javascript">
            var textblock = document.getElementById("block1");
            var typeCell = document.getElementById("eType");
            var xCell = document.getElementById("eX");
            var yCell = document.getElementById("eY");

            textblock.addEventListener("mouseover", handleMouseEvent, false);
            textblock.addEventListener("mouseout", handleMouseEvent, false);
            textblock.addEventListener("mousemove", handleMouseEvent, false);

            function handleMouseEvent(e) {
                if (e.eventPhase == Event.AT_TARGET) {
                    typeCell.innerHTML = e.type;
                    xCell.innerHTML = e.clientX;
                    yCell.innerHTML = e.clientY;

                    if (e.type == "mouseover") {
                        e.target.style.background='black';
                        e.target.style.color='white';
                    } else {
                        e.target.style.removeProperty('color');
                        e.target.style.removeProperty('background');
                    }
                }
            }
        </script>
```

### 7.7  使用键盘焦点事件

与键盘焦点相关的事件触发于元素获得和失去焦点之时。

#### **键盘焦点相关的事件**

| 名称       | 说明                     |
| :--------- | :----------------------- |
| `blur`     | 在元素失去焦点时触发     |
| `focus`    | 在元素获得焦点时触发     |
| `focusin`  | 在元素即将获得焦点时触发 |
| `focusout` | 在元素即将失去焦点时触发 |

**FocusEvent对象**

| 名称            | 说明                                                         | 返回          |
| :-------------- | :----------------------------------------------------------- | :------------ |
| `relatedTarget` | 元素即将获得或失去焦点。这个属性只用于`focusin` 和`focusout` 事件 | `HTMLElement` |

使用示例：

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <style type="text/css">
            p {
                background: gray;
                color:white;
                padding: 10px;
                margin: 5px;
                border: thin solid black
            }
        </style>
    </head>
    <body>
        <form>
            <p>
                <label for="fave">Fruit: <input autofocus id="fave" name="fave"/></label>
            </p>
            <p>
                <label for="name">Name: <input id="name" name="name"/></label>
            </p>
            <button type="submit">Submit Vote</button>
            <button type="reset">Reset</button>
        </form>

        <script type="text/javascript">

            var inputElems = document.getElementsByTagName("input");
            for (var i = 0; i < inputElems.length; i++) {
                inputElems[i].onfocus = handleFocusEvent;
                inputElems[i].onblur = handleFocusEvent;
            }

            function handleFocusEvent(e) {
                if (e.type == "focus") {
                    e.target.style.backgroundColor = "lightgray";
                    e.target.style.border = "thick double red";
                } else {
                    e.target.style.removeProperty("background-color");
                    e.target.style.removeProperty("border");
                }
            }
        </script>
    </body>
</html>
```

### 7.8  使用键盘事件

| 名称       | 说明                         |
| :--------- | :--------------------------- |
| `keydown`  | 在用户按下某个键时触发       |
| `keypress` | 在用户按下并释放某个键时触发 |
| `keyup`    | 在用户释放某个键时触发       |

**`KeyboardEvent` 对象**

| 名称       | 说明                                        | 返回   |
| :--------- | :------------------------------------------ | :----- |
| `char`     | 返回按键代表的字符                          | 字符串 |
| `key`      | 返回所按的键                                | 字符串 |
| `ctrlKey`  | 如果在按键时Ctrl键处于按下状态则返回`true`  | 布尔值 |
| `shiftKey` | 如果在按键时Shift键处于按下状态则返回`true` | 布尔值 |
| `altKey`   | 如果在按键时Alt键处于按下状态则返回`true`   | 布尔值 |
| `repeat`   | 如果该键一直处于按下状态则返回`true`        | 布尔值 |

使用示例：

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <style type="text/css">
            p {
                background: gray;
                color:white;
                padding: 10px;
                margin: 5px;
                border: thin solid black
            }
        </style>
    </head>
    <body>
        <form>
            <p>
                <label for="fave">Fruit: <input autofocus id="fave" name="fave"/></label>
            </p>
            <p>
                <label for="name">Name: <input id="name" name="name"/></label>
            </p>
            <button type="submit">Submit Vote</button>
            <button type="reset">Reset</button>
        </form>
        <span id="message"></span>

        <script type="text/javascript">

            var inputElems = document.getElementsByTagName("input");
            for (var i = 0; i < inputElems.length; i++) {
                inputElems[i].onkeyup = handleKeyboardEvent;
            }

            function handleKeyboardEvent(e) {
                document.getElementById("message").innerHTML = "Key pressed: " +
                    e.keyCode + " Char: " + String.fromCharCode(e.keyCode);
            }
        </script>
    </body>
</html>
```

### 7.9  使用表单事件

`form` 元素定义了两种只适用于此元素的特殊事件。

| 名称     | 说明             |
| :------- | :--------------- |
| `submit` | 在表单提交时触发 |
| `reset`  | 在表单重置时触发 |