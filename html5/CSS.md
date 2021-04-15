# CSS

CSS（Cascading Style Sheet，**层叠样式表** ）定义的属性。

[TOC]

## 1. 初涉CSS

### 1.1  CSS标准

具有相同名称的属性采用不同的方式处理，只能用浏览器特定的属性访问浏览器特定的功能。指定厂商前缀设置样式。

<b>浏览器特定厂商前缀:</b>

| 浏览器            | 厂商前缀   |
| :---------------- | :--------- |
| Chrome、Safari    | `-webkit-` |
| Opera             | `-o-`      |
| Firefox           | `-moz-`    |
| Internet Explorer | `-ms-`     |

### 1.2  盒模型

CSS中的一个基本概念是**盒模型** （box model）。元素盒子有两个部分是可见的：内容和边框。内边距是内容和边框之间的空间，外边距是边框和页面上其他元素之间的空间。理解这四部分之间的相互关系对于高效利用CSS至关重要。

### 1.3  选择器简明参考

| 选择器                      | 说明                                                         | CSS版本 |
| :-------------------------- | :----------------------------------------------------------- | :------ |
| `*`                         | 选取所有元素                                                 | 2       |
| `<type>`                    | 选取指定类型的元素                                           | 1       |
| `.<class>`                  | 选取指定类的元素                                             | 1       |
| `#<id>`                     | 选取`id` 属性为指定值的元素                                  | 1       |
| `[attr]`                    | 选取定义了`attr` 属性，且属性值任意的元素                    | 2       |
| `[attr="val"]`              | 选取定义了`attr` 属性，且属性值为`val` 的元素                | 2       |
| `[attr^="val"]`             | 选取定义了`attr` 属性，且属性值以`val` 字符串打头的元素      | 3       |
| `[attr$="val"]`             | 选取定义了`attr` 属性，且属性值以`val` 字符串结尾的元素      | 3       |
| `[attr*="val"]`             | 选取定义了`attr` 属性，且属性值包含`val` 字符串的元素        | 3       |
| `[attr~="val"]`             | 选取定义了`attr` 属性，且属性值包含多个值，而其中一个为`val` 的元素 | 2       |
| `[attr|="val"]`             | 选取定义了`attr` 属性，且属性值是以连字符分割的一串值，而第一个为`val` 的元素 | 2       |
| `<选择器>, <选择器>`        | 选取同时匹配所有选择器的元素                                 | 1       |
| `<选择器> <选择器>`         | 目标元素为匹配第一个选择器的元素的后代，且匹配第二个选择器   | 1       |
| `<选择器> > <选择器>`       | 目标元素为匹配第一个选择器的元素的直接后代，且匹配第二个选择器 | 2       |
| `<选择器> + <选择器>`       | 目标元素紧跟在匹配第一个选择器的元素之后，且匹配第二个选择器 | 2       |
| `<选择器> ~ <选择器>`       | 目标元素跟在匹配第一个选择器的元素之后，且匹配第二个选择器   | 3       |
| `::first-line`              | 选取块级元素文本的首行                                       | 1       |
| `::first-letter`            | 选取块级元素文本的首字母                                     | 1       |
| `:before` `:after`          | 在选取元素之前或之后插入内容                                 | 2       |
| `:root`                     | 选取文档中的根元素                                           | 3       |
| `:first-child`              | 选取元素的第一个子元素                                       | 2       |
| `:last-child`               | 选取元素的最后一个子元素                                     | 3       |
| `:only-child`               | 选取元素的唯一一个子元素                                     | 3       |
| `:only-of-type`             | 选取属于父元素的特定类型的唯一子元素                         | 3       |
| `:nth-child(n)`             | 选取父元素的第*n* 个子元素                                   |         |
| `:nth-last-child(n)`        | 选取父元素的倒数第*n* 个子元素                               | 3       |
| `:nth-of-type(n)`           | 选取属于父元素的特定类型的第*n* 个子元素                     | 3       |
| `:nth-last-of-type(n)`      | 选取属于父元素的特定类型的倒数第*n* 个子元素                 | 3       |
| `:enabled`                  | 选取已启用的元素                                             | 3       |
| `:disabled`                 | 选取被禁用的元素                                             | 3       |
| `:checked`                  | 选取所有选中的复选框和单选按钮                               | 3       |
| `:default`                  | 选取默认元素                                                 | 3       |
| `:valid` `:invalid`         | 选取基于输入验证判定的有效或者无效的`input` 元素             | 3       |
| `:in-range` `:out-of-range` | 选取被限定在指定范围之内或者之外的`input` 元素               | 3       |
| `:required` `:optional`     | 根据是否允许使用`required` 属性选取`input` 元素              | 3       |
| `:link`                     | 选取未访问的链接元素                                         | 1       |
| `:visited`                  | 选取用户已访问的链接元素                                     | 1       |
| `:hover`                    | 选取鼠标指针悬停在其上面的元素                               | 2       |
| `:active`                   | 选取当前被用户激活的元素，这通常意味着用户即将点击该元素     | 2       |
| `:focus`                    | 选取获得焦点的元素                                           | 2       |
| `:not(<选择器>)`            | 否定选择（如选取所有不匹配`<选择器>` 的元素）                | 3       |
| `:empty`                    | 选取不包含任何子元素或文本的元素                             | 3       |
| `:lang(<语言>)`             | 选取`lang` 属性为指定值的元素                                | 2       |
| `:target`                   | 选取URL片段标识符指向的元素                                  | 3       |

### 1.4  属性简明参考

| 属性                         | 说明                                                   | CSS版本 |
| :--------------------------- | :----------------------------------------------------- | :------ |
| `background`                 | 设置所有背景值的简写属性                               | 1       |
| `background-attachment`      | 设置元素的背景附着属性，决定背景图片是否随页面一起滚动 | 1       |
| `background-clip`            | 设置元素背景颜色和图像的裁剪区域                       | 3       |
| `background-color`           | 设置背景颜色                                           | 1       |
| `background-image`           | 设置元素背景图像                                       | 1       |
| `background-origin`          | 设置背景图像绘制的起始位置                             | 3       |
| `background-position`        | 设置背景图像在元素盒子中的位置                         | 1       |
| `background-repeat`          | 设置背景图像的重复方式                                 | 1       |
| `background-size`            | 设置背景图像的绘制尺寸                                 | 3       |
| `border`                     | 为所有边界设置所有边框宽度的简写属性                   | 1       |
| `border-bottom`              | 为所有下边框设置宽度的简写属性                         | 1       |
| `border-bottom-color`        | 为所有下边框设置颜色                                   | 1       |
| `border-bottom-left-radius`  | 将边框左下角设置为圆角                                 | 3       |
| `border-bottom-right-radius` | 将边框右下角设置为圆角                                 | 3       |
| `border-bottom-style`        | 设置元素下边框的样式                                   | 1       |
| `border-bottom-width`        | 设置元素下边框的宽度                                   | 1       |
| `border-color`               | 设置四条边框的颜色                                     | 1       |
| `border-image`               | 使用图像作为边框的简写属性                             | 3       |
| `border-image-outset`        | 指定图像向边框盒外部扩展的区域                         | 3       |
| `border-image-repeat`        | 指定边框图像的缩放和重复方式                           | 3       |
| `border-image-slice`         | 指定边框图像的切割方式                                 | 3       |
| `border-image-source`        | 设置边框图像的来源路径                                 | 3       |
| `border-image-width`         | 设置边框图像的宽度                                     | 3       |
| `border-left`                | 设置元素左边框的简写属性                               | 1       |
| `border-left-color`          | 设置左边框的颜色                                       | 1       |
| `border-left-style`          | 设置左边框的样式                                       | 1       |
| `border-left-width`          | 设置左边框的宽度                                       | 1       |
| `border-radius`              | 指定圆角边框的简写属性                                 | 3       |
| `border-right`               | 设置元素右边框的简写属性                               | 1       |
| `border-right-color`         | 设置右边框的颜色                                       | 1       |
| `border-right-style`         | 设置右边框的样式                                       | 1       |
| `border-right-width`         | 设置右边框的宽度                                       | 1       |
| `border-style`               | 设置所有边框样式的简写属性                             | 1       |
| `border-top`                 | 设置上边框的简写属性                                   | 1       |
| `border-top-color`           | 设置上边框的颜色                                       | 1       |
| `border-top-left-radius`     | 将边框左上角设置为圆角                                 | 3       |
| `border-top-right-radius`    | 将边框右上角设置为圆角                                 | 3       |
| `border-top-style`           | 设置上边框的样式                                       | 1       |
| `border-top-width`           | 设置上边框的宽度                                       | 1       |
| `border-width`               | 设置四个边框的宽度                                     | 1       |
| `box-shadow`                 | 设置元素的一个或者多个阴影效果                         | 3       |
| `outline-color`              | 设置元素边框外围轮廓线的颜色                           | 2       |
| `outline-offset`             | 设置轮廓距离元素边框边缘的偏移量                       | 2       |
| `outline-style`              | 设置轮廓的样式                                         | 2       |
| `outline-width`              | 设置轮廓的宽度                                         | 2       |
| `outline`                    | 在一条声明中设置轮廓的简写属性                         | 2       |

### 1.5  盒模型属性

| 选择器           | 说明                                                         | CSS版本 |
| :--------------- | :----------------------------------------------------------- | :------ |
| `box-sizing`     | 设置要应用盒子尺寸相关属性的元素                             | 3       |
| `clear`          | 设置盒子的左边界、右边界或左右两个边界不允许出现浮动元素     | 1       |
| `display`        | 设置元素盒子的类型                                           | 1       |
| `float`          | 将元素移动到其包含块的左边界或者右边界，或者另一个浮动元素的边界 | 1       |
| `height`         | 设置元素盒子的高度                                           | 1       |
| `margin`         | 设置元素盒子四个外边距宽度的简写属性                         | 1       |
| `margin-bottom`  | 设置盒子下外边距的宽度                                       | 1       |
| `margin-left`    | 设置盒子左外边距的宽度                                       | 1       |
| `margin-right`   | 设置盒子右外边距的宽度                                       | 1       |
| `margin-top`     | 设置盒子上外边距的宽度                                       | 1       |
| `max-height`     | 设置元素的最大高度                                           | 2       |
| `max-width`      | 设置元素的最大宽度                                           | 2       |
| `min-height`     | 设置元素的最小高度                                           | 2       |
| `min-width`      | 设置元素的最小宽度                                           | 2       |
| `overflow`       | 设置内容横向和竖向溢出盒子时处理方式的简写属性               | 2       |
| `overflow-x`     | 设置内容横向溢出盒子时的处理方式                             | 3       |
| `overflow-y`     | 设置内容竖向溢出盒子时的处理方式                             | 3       |
| `padding`        | 设置元素盒子四个内边距宽度的简写属性                         | 1       |
| `padding-bottom` | 设置盒子下内边距的宽度                                       | 1       |
| `padding-left`   | 设置盒子左内边距的宽度                                       | 1       |
| `padding-right`  | 设置盒子右内边距的宽度                                       | 1       |
| `padding-top`    | 设置盒子上内边距的宽度                                       | 1       |
| `visibility`     | 设置元素的可见性                                             | 2       |
| `width`          | 设置元素的宽度                                               | 1       |

### 1.6  布局属性

| 选择器                                                 | 说明                                         | CSS版本 |
| :----------------------------------------------------- | :------------------------------------------- | :------ |
| `bottom`                                               | 设置元素下外边距边界与包含块下边界之间的偏移 | 2       |
| `column-count`                                         | 指定多列布局的列数                           | 3       |
| `column-fill`                                          | 多列布局中列与列之间的内容如何分布           | 3       |
| `column-gap`                                           | 指定多列布局中列与列之间的间隔               | 3       |
| `column-rule`                                          | 多列布局中定义列与列之间的规则的简写属性     | 3       |
| `column-rule-color`                                    | 设置多列布局中的颜色规则                     | 3       |
| `column-rule-style`                                    | 设置多列布局中的样式规则                     | 3       |
| `column-rule-width`                                    | 设置多列布局中的宽度规则                     | 3       |
| `columns`                                              | 在多列布局中设置列数和列宽度的简写属性       | 3       |
| `column-span`                                          | 指定多列布局中元素能跨多少列                 | 3       |
| `column-width`                                         | 指定多列布局中列的宽度                       | 3       |
| `display`                                              | 指定元素在页面上的显示方式                   | 1       |
| `flex-align` `flex-direction` `flex-order` `flex-pack` | 它们都是由弹性盒子布局定义的，目前还没有实现 | 3       |
| `left`                                                 | 设置元素左外边距边界与包含块左边界之间的偏移 | 2       |
| `position`                                             | 设置元素的定位方法                           | 2       |
| `right`                                                | 设置元素右外边距边界与包含块右边界之间的偏移 | 2       |
| `top`                                                  | 设置元素上外边距边界与包含块上边界之间的偏移 | 2       |
| `z-index`                                              | 设置定位元素的堆叠顺序                       | 2       |

### 1.7  文本属性

| 属性              | 说明                                           | CSS版本 |
| :---------------- | :--------------------------------------------- | :------ |
| `@font-face`      | 指定网页使用的字体                             | 3       |
| `direction`       | 指定文本方向                                   | 2       |
| `font`            | 在一条声明中设置文本字体、大小和颜色的简写属性 | 1       |
| `font-family`     | 指定文本所用的字体系列，排在前面的优先使用     | 1       |
| `font-size`       | 指定字体大小                                   | 1       |
| `font-style`      | 指定采用正常字体、斜体还是倾斜字体             | 1       |
| `font-variant`    | 指定字体是否以小型大写字母显示                 | 1       |
| `font-weight`     | 设置文本粗细                                   | 1       |
| `letter-spacing`  | 设置字母间距                                   | 1       |
| `line-height`     | 设置行高                                       | 1       |
| `text-align`      | 设置文本对齐方式                               | 1       |
| `text-decoration` | 规定添加到文本的修饰（如下划线）               | 1       |
| `text-indent`     | 规定文本块中首行文本的缩进                     | 1       |
| `text-justify`    | 设置文本对齐方式                               | 3       |
| `text-shadow`     | 指定文本块的阴影效果                           | 3       |
| `text-transform`  | 控制文本块字母大小写                           | 1       |
| `word-spacing`    | 指定单词间距                                   | 1       |

### 1.8  过度、动画和变换属性

| 属性                         | 说明                             | CSS版本 |
| :--------------------------- | :------------------------------- | :------ |
| `@keyframes`                 | 为动画指定一个以上的关键帧       | 3       |
| `animation`                  | 设置动画的简写属性               | 3       |
| `animation-delay`            | 指定动画开始前的延迟时间         | 3       |
| `animation-direction`        | 指定动画重复播放时的播放方向     | 3       |
| `animation-duration`         | 指定动画持续时间                 | 3       |
| `animation-iteration-count`  | 指定动画的循环次数               | 3       |
| `animation-name`             | 指定用于动画的关键帧集合的名称   | 3       |
| `animation-play-state`       | 指定动画状态（播放或暂停）       | 3       |
| `animation-timing-function`  | 指定关键帧之间计算属性值的函数   | 3       |
| `transform`                  | 指定应用于元素的变换             | 3       |
| `transform-origin`           | 指定元素变换的起点               | 3       |
| `transition`                 | 指定CSS属性过渡效果的简写属性    | 3       |
| `transition-delay`           | 指定触发过渡的延迟时间           | 3       |
| `transition-duration`        | 指定过渡的持续时间               | 3       |
| `transition-property`        | 指定带有过渡效果的属性           | 3       |
| `transition-timing-function` | 指定过渡期间计算中间属性值的函数 | 3       |

### 1.9  其他属性

| 属性                  | 说明                                 | CSS版本 |
| :-------------------- | :----------------------------------- | :------ |
| `border-collapse`     | 指定表格相邻单元格的边框的显示样式   | 2       |
| `border-spacing`      | 指定相邻单元格的边框的距离           | 2       |
| `caption-side`        | 指定表格标题的位置                   | 2       |
| `color`               | 设置元素的前景色                     | 1       |
| `cursor`              | 指定光标的形状                       | 2       |
| `empty-cells`         | 指定是否显示表格中的空单元格         | 2       |
| `list-style`          | 设置列表样式的简写属性               | 1       |
| `list-style-image`    | 指定列表项标记使用的图像             | 1       |
| `list-style-position` | 指定列表项标记相对于列表项内容的位置 | 1       |
| `list-style-type`     | 指定列表项标记的类型                 | 1       |
| `opacity`             | 设置元素的透明度                     | 3       |
| `table-layout`        | 指定表格单元格、行和列的算法规则     | 2       |

## 2. CSS选择器

### 2.1  基本选择器

#### **通用选择器**

**通用选择器** 匹配文档中的所有元素。它是最基本的选择器，不过使用很少，因为匹配过于广泛。

| 选择器          | `*`      |
| --------------- | -------- |
| 匹配            | 所有元素 |
| 最低支持CSS版本 | 2        |

```
* {
    border: thin black solid;
    padding: 4px;
  }
```

#### 元素选择器

指定元素类型为选择器可以选取一个文档中该元素的所有实例（例如，如果你想选中所有的`a` 元素就可以使用`a` 作为选择器）。

| 选择器            | `<元素类型>`       |
| ----------------- | ------------------ |
| 匹配              | 所有指定类型的元素 |
| 开始支持的CSS版本 | 1                  |

```
a {
    border: thin black solid;
    padding: 4px;
  }
```

#### 类选择器

类选择器采用全局属性`class` 匹配指定类的元素。

| 选择器          | `<类名>（或 *.<类名>` ） `<元素类型>.<类名>`                 |
| --------------- | ------------------------------------------------------------ |
| 匹配            | 属于指定类的元素；当跟元素类型一起使用时，匹配属于指定类的特定类型的元素 |
| 最低支持CSS版本 | 1                                                            |

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <style type="text/css">
            .class2 {
                border: thin black solid;
                padding: 4px;
            }
        </style>
    </head>
    <body>
        <a class="class1 class2" href="http://apress.com">Visit the Apress website</a>
        <p>I like <span class="class2">apples</span> and oranges.</p>
        <a href="http://w3c.org">Visit the W3C website</a>
    </body>
</html>
```

#### `id` 选择器

| 选择器          | `#<id值>` `<元素类型>.#<id值>` |
| --------------- | ------------------------------ |
| 匹配            | 具有指定全局属性`id` 值的元素  |
| 最低支持CSS版本 | 1                              |

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <style type="text/css">
            #w3canchor {
                border: thin black solid;
                padding: 4px;
            }
        </style>
    </head>
    <body>
        <a id="apressanchor" class="class1 class2" href="http://apress.com">
            Visit the Apress website
        </a>
        <p>I like <span class="class2">apples</span> and oranges.</p>
        <a id="w3canchor" href="http://w3c.org">Visit the W3C website</a>
    </body>
</html>
```

#### 元素属性选择器

使用属性选择器能基于属性的不同方面匹配属性。

| 选择器          | `[<条件>]或<元素类型>[<条件>]` |
| --------------- | ------------------------------ |
| 匹配            | 具有匹配指定条件的属性的元素   |
| 最低支持CSS版本 | 视条件而异                     |

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <style type="text/css">
            [href] {
                border: thin black solid;
                padding: 4px;
            }
        </style>
    </head>
    <body>
        <a id="apressanchor" class="class1 class2" href="http://apress.com">
            Visit the Apress website
        </a>
        <p>I like <span class="class2">apples</span> and oranges.</p>
        <a id="w3canchor" href="http://w3c.org">Visit the W3C website</a>
    </body>
</html>
```

<b>条件</b>

| 条件            | 说明                                                         | CSS版本 |
| :-------------- | :----------------------------------------------------------- | :------ |
| `[attr]`        | 选择定义`attr` 属性的元素，忽略属性值（代码清单17-6就是这种情况） | 2       |
| `[attr="val"]`  | 选择定义`attr` 属性，且属性值为`val` 的元素                  | 2       |
| `[attr^="val"]` | 选择定义`attr` 属性，且属性值以字符串`val` 打头的元素        | 3       |
| `[attr$="val"]` | 选择定义`attr` 属性，且属性值以字符串`val` 结尾的元素        | 3       |
| `[attr*="val"]` | 选择定义`attr` 属性，且属性值包含字符串`val` 的元素          | 3       |
| `[attr~="val"]` | 选择定义`attr` 属性，且属性值具有多个值，其中一个为字符串`val` 的元素。代码清单17-7是使用该选择器的一个示例 | 2       |
| `[attr|="val"]` | 选择定义`attr` 属性，且属性值为连字符分割的多个值，其中第一个为字符串`val` 的元素。代码清单17-8是使用该选择器的一个示例 | 2       |

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <style type="text/css">
            [class~="class2"] {
                border: thin black solid;
                padding: 4px;
            }
        </style>
    </head>
    <body>
        <a id="apressanchor" class="class1 class2" href="http://apress.com">
            Visit the Apress website
        </a>
        <p>I like <span class="class2">apples</span> and oranges.</p>
        <a id="w3canchor" href="http://w3c.org">Visit the W3C website</a>
    </body>
</html>
```

### 2.2  复合选择器

组合使用不同的选择器可以匹配更特定的元素。

#### **并集选择器**

创建由逗号分隔的多个选择器可以将样式应用到单个选择器匹配的所有元素。

| 选择器          | `<选择器>, <选择器>, <选择器>` |
| --------------- | ------------------------------ |
| 匹配            | 单个选择器匹配的所有元素的并集 |
| 最低支持CSS版本 | 1                              |

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <style type="text/css">
            a, [lang|="en"] {
                border: thin black solid;
                padding: 4px;
            }
        </style>
    </head>
    <body>
        <a id="apressanchor" class="class1 class2" href="http://apress.com">
            Visit the Apress website
        </a>
        <p>I like <span lang="en-uk" class="class2">apples</span> and oranges.</p>
        <a id="w3canchor" href="http://w3c.org">Visit the W3C website</a>
    </body>
</html>
```

#### **后代选择器**

**后代选择器** 用于选择包含在其他元素中的元素。

| 选择器          | `<第一个选择器> <第二个选择器>`                            |
| --------------- | ---------------------------------------------------------- |
| 匹配            | 目标元素为匹配第一个选择器的元素的后代，且匹配第二个选择器 |
| 最低支持CSS版本 | 1                                                          |

先应用第一个选择器，再从匹配元素的**后代** 中找出匹配第二个选择器的元素。后代选择器会匹配**任意** 包含在匹配第一个选择器的元素中的元素，而不仅是直接子元素。

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <style type="text/css">
            #mytable td {
                border: thin black solid;
                padding: 4px;
            }
        </style>
    </head>
    <body>
        <table id="mytable">
            <tr><th>Name</th><th>City</th></tr>
            <tr><td>Adam Freeman</td><td>London</td></tr>
            <tr><td>Joe Smith</td><td>New York</td></tr>
            <tr><td>Anne Jones</td><td>Paris</td></tr>
        </table>

        <p>I like <span lang="en-uk" class="class2">apples</span> and oranges.</p>

        <table id="othertable">
            <tr><th>Name</th><th>City</th></tr>
            <tr><td>Peter Pererson</td><td>Boston</td></tr>
            <tr><td>Chuck Fellows</td><td>Paris</td></tr>
            <tr><td>Jane Firth</td><td>Paris</td></tr>
        </table>
    </body>
</html>
```

#### **子代选择器**

子代选择器跟后代选择器很像，不过只选择匹配元素中的直接后代。

| 选择器          | `<第一个选择器> > <第二个选择器>`                            |
| --------------- | ------------------------------------------------------------ |
| 匹配            | 目标元素为匹配第一个选择器的元素的直接后代，且匹配第二个选择器 |
| 最低支持CSS版本 | 2                                                            |

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <style type="text/css">
            body > * > span, tr > th {
                border: thin black solid;
                padding: 4px;
            }
        </style>
    </head>
    <body>
        <table id="mytable">
            <tr><th>Name</th><th>City</th></tr>
            <tr><td>Adam Freeman</td><td>London</td></tr>
            <tr><td>Joe Smith</td><td>New York</td></tr>
            <tr><td>Anne Jones</td><td>Paris</td></tr>
        </table>

        <p>I like <span lang="en-uk" class="class2">apples</span> and oranges.</p>

        <table id="othertable">
            <tr><th>Name</th><th>City</th></tr>
            <tr><td>Peter Pererson</td><td>Boston</td></tr>
            <tr><td>Chuck Fellows</td><td>Paris</td></tr>
            <tr><td>Jane Firth</td><td>Paris</td></tr>
        </table>
    </body>
</html>
```

#### **相邻兄弟选择器**

使用**相邻兄弟选择器** 可以选择紧跟在某元素之后的元素。

| 选择器          | `<第一个选择器> + <第二个选择器>`                      |
| --------------- | ------------------------------------------------------ |
| 匹配            | 目标元素紧跟匹配第一个选择器的元素，且匹配第二个选择器 |
| 最低支持CSS版本 | 2                                                      |

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <style type="text/css">
            p + a {
                border: thin black solid;
                padding: 4px;
            }
        </style>
    </head>
    <body>
        <a href="http://apress.com">Visit the Apress website</a>
        <p>I like <span lang="en-uk" class="class2">apples</span> and oranges.</p>
        <a href="http://w3c.org">Visit the W3C website</a>
        <a href="http://google.com">Visit Google</a>
    </body>
</html>
```

代码分析：代码清单中的选择器会匹配紧跟在`p` 元素之后的`a` 元素。从中可以看到，文档中只有一个`a` 元素符合要求，即指向W3C网站的超链接。

#### **普通兄弟选择器**

使用**普通兄弟选择器** 选择范围会稍微宽松一些，它匹配的元素在指定元素之后，但不一定相邻。

| 选择器          | `<第一个选择器> ~ <第二个选择器>`                          |
| --------------- | ---------------------------------------------------------- |
| 匹配            | 目标元素位于匹配第一个选择器的元素之后，且匹配第二个选择器 |
| 最低支持CSS版本 | 3                                                          |

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <style type="text/css">
            p ~ a {
                border: thin black solid;
                padding: 4px;
            }
        </style>
    </head>
    <body>
        <a href="http://apress.com">Visit the Apress website</a>
        <p>I like <span lang="en-uk" class="class2">apples</span> and oranges.</p>
        <a href="http://w3c.org">Visit the W3C website</a>
        <a href="http://google.com">Visit Google</a>
    </body>
</html>
```

### 2.3  伪元素选择器

#### **`::first-line` 伪元素选择器**

`::first-line` 选择器匹配文本块的首行。

| 选择器          | `::first-line` |
| --------------- | -------------- |
| 匹配            | 文本内容的首行 |
| 最低支持CSS版本 | 1              |

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <style type="text/css">
            ::first-line {
                background-color:grey;
                color:white;
            }
        </style>
    </head>
    <body>
        <p>Fourscore and seven years ago our fathers brought forth
           on this continent a new nation, conceived in liberty, and
           dedicated to the proposition that all men are created equal.</p>

        <p>I like <span lang="en-uk" class="class2">apples</span> and oranges.</p>

        <a href="http://w3c.org">Visit the W3C website</a>
    </body>
</html>
```

#### **`::first-letter` 伪元素选择器**

`::first-letter` 选择器的作用：选择文本块的首字母。

| 选择器          | `::first-letter` |
| --------------- | ---------------- |
| 匹配            | 文本内容的首字母 |
| 最低支持CSS版本 | 1                |

#### **`:before` 和`:after` 选择器**

这两个选择器跟之前的选择器都不太一样，因为它们会生成内容，并将其插入文档。

| 选择器    | 说明                         | CSS版本 |
| :-------- | :--------------------------- | :------ |
| `:before` | 在选中元素的内容之前插入内容 | 2       |
| `:after`  | 在选中元素的内容之后插入内容 | 2       |

```htnl
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <style type="text/css">
            a:before {
                content: "Click here to "
            }
            a:after {
                content: "!"
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

#### CSS计数器

`:before` 和`:after` 选择器经常跟CSS**计数器** 特性一起使用，结合两者可生成数值内容。

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <style type="text/css">
            body {
                counter-reset: paracount;
            }
            p:before {
                content: counter(paracount) ". ";
                counter-increment: paracount;
            }
        </style>
    </head>
    <body>
        <a href="http://apress.com">Visit the Apress website</a>
        <p>I like <span>apples</span> and oranges.</p>
        <p>I also like <span>mangos</span> and cherries.</p>
        <a class="myclass1 myclass2" href="http://w3c.org">Visit the W3C website</a>
    </body>
</html>
```

要创建计数器，需要使用专门的`counter-reset` 属性为计数器设置名称，如下所示：

```
counter-reset: paracount;
```

这一行代码会初始化名为`paracount` 的计数器，将它的值设置为1。你也可以指定其他初始值，只需要在计数器名称后面添加一个数字即可，像这样：

```
counter-reset: paracount 10;
```

如果你想多定义几个计数器，只需在同一条`counter-reset` 声明中添加计数器名称就可以了（也可以带上初始值），像这样：

```
counter-reset: paracount 10 othercounter;
```

这条声明创建了两个计数器：一个名为`paracount` ，初始值为10；另一个名为`othercounter` ，初始值为1。计数器初始化后就能够作为`content` 属性的值，跟`:before` 和`:after` 选择器一起使用来指定样式了，像这样：

```
content: counter(paracount) ". ";
```

这条声明用在包括`:before` 的选择器中，其效果是将当前计数器的值呈现在选择器匹配的所有元素之前，此处，还要在相应的值后面追加一个句点和空格。计数器的值默认表示为十进制整数（1、2、3等），不过，也可以指定其他数值格式，像这样：

```
content: counter(paracount, lower-alpha) ". ";
```

此处对计数器添加了参数`lower-alpha` ，其功能是指定数值样式。这个参数可以是`list-style-type` 属性支持的任意值。

`counter-increment` 属性专门用来设置计数器增量，该属性的值是要增加计数的计数器的名称，像这样：

```
counter-increment: paracount;
```

计数器默认增量为1，当然也可以自行指定其他增量，像这样：

```
counter-increment: paracount 2;
```

### 2.4  伪类

使用结构性伪类构造选择器：使用结构性伪类选择器能够根据元素在文档中的位置选择元素。这类选择器都有一个冒号字符前缀（`:` ），例如`:empty` 。它们可以单独使用，也可以跟其他选择器组合使用，如`p:empty` 。

#### **`:root` 选择器**

`:root` 选择器匹配文档中的根元素。它可能是用得最少的一个伪类选择器，因为总是返回`html` 元素。

| 选择器          | `:root`                            |
| --------------- | ---------------------------------- |
| 匹配            | 选择文档中的根元素，总是返回`html` |
| 最低支持CSS版本 | 3                                  |

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <style type="text/css">
              :root {
                border: thin black solid;
                padding: 4px;
            }
        </style>
    </head>
    <body>
         <a href="http://apress.com">Visit the Apress website</a>
        <p>I like <span lang="en-uk" class="class2">apples</span> and oranges.</p>
        <a href="http://w3c.org">Visit the W3C website</a>
    </body>
</html>
```

#### **子元素选择器**

使用**子元素选择器** 匹配直接包含在其他元素中的单个元素。

| 选择器          | 说明                         | CSS版本 |
| :-------------- | :--------------------------- | :------ |
| `:first-child`  | 选择元素的第一个子元素       | 2       |
| `:last-child`   | 选择元素的最后一个子元素     | 3       |
| `:only-child`   | 选择元素的唯一子元素         | 3       |
| `:only-of-type` | 选择元素指定类型的唯一子元素 | 3       |

<b>`:first-child` 选择器匹配由包含它们的元素（即父元素）定义的第一个子元素</b>

```
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <style type="text/css">
              :first-child {
                border: thin black solid;
                padding: 4px;
            }
        </style>
    </head>
    <body>
        <a href="http://apress.com">Visit the Apress website</a>
        <p>I like <span>apples</span> and <span>oranges</span>.</p>
        <a href="http://w3c.org">Visit the W3C website</a>
    </body>
</html>
```

<b>`:last-child` 选择器匹配由包含它们的元素定义的最后一个元素</b>

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <style type="text/css">
              :last-child {
                border: thin black solid;
                padding: 4px;
            }
        </style>
    </head>
    <body>
        <a href="http://apress.com">Visit the Apress website</a>
        <p>I like <span>apples</span> and <span>oranges</span>.</p>
        <a href="http://w3c.org">Visit the W3C website</a>
    </body>
</html>
```

<b>`:only-child` 选择器匹配父元素包含的唯一子元素</b>

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <style type="text/css">
              :only-child {
                border: thin black solid;
                padding: 4px;
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

<b>`:only-of-type` 选择器匹配父元素定义类型的唯一子元素</b>

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <style type="text/css">
              :only-of-type {
                border: thin black solid;
                padding: 4px;
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

#### **`:nth-child` 选择器**

`:nth-child` 选择器跟上一节讲的子元素选择器类似，但使用这类选择器可以指定一个索引以匹配特定位置的元素。

| 选择器                 | 说明                                   | CSS版本 |
| :--------------------- | :------------------------------------- | :------ |
| `:nth-child(n)`        | 选择父元素的第*n* 个子元素             | 3       |
| `:nth-last-child(n)`   | 选择父元素的倒数第*n* 个子元素         | 3       |
| `:nth-of-type(n)`      | 选择父元素定义类型的第*n* 个子元素     | 3       |
| `:nth-last-of-type(n)` | 选择父元素定义类型的倒数第*n* 个子元素 | 3       |

这类选择器都带有一个参数，索引从1开始。

<b>使用`:nth-child` 选择器</b>

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <style type="text/css">
              body > :nth-child(2) {
                border: thin black solid;
                padding: 4px;
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

### 2.5  UI伪类选择器

#### **UI选择器**

使用UI伪类选择器可以根据元素的状态匹配元素。

| 选择器                      | 说明                                               | CSS版本 |
| :-------------------------- | :------------------------------------------------- | :------ |
| `:enabled`                  | 选择启用状态的元素                                 | 3       |
| `:disabled`                 | 选择禁用状态的元素                                 | 3       |
| `:checked`                  | 选择被选中的`input` 元素（只用于单选按钮和复选框） | 3       |
| `:default`                  | 选择默认元素                                       | 3       |
| `:valid` `:invalid`         | 根据输入验证选择有效或者无效的`input` 元素         | 3       |
| `:in-range` `:out-of-range` | 选择在指定范围之内或者之外受限的`input` 元素       | 3       |
| `:required` `:optional`     | 根据是否允许`:required` 属性选择`input` 元素       | 3       |

<b>使用`:enabled` 选择器</b>

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <style type="text/css">
              :enabled {
                border: thin black solid;
                padding: 4px;
            }
        </style>
    </head>
    <body>
        <textarea> This is an enabled textarea</textarea>
        <textarea disabled> This is a disabled textarea</textarea>
    </body>
</html>
```

<b>使用`:checked` 选择器</b>

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <meta name="author" content="Adam Freeman"/>
        <meta name="description" content="A simple example"/>
        <link rel="shortcut icon" href="favicon.ico" type="image/x-icon" />
        <style>
            :checked + span {
                background-color: red;
                color: white;
                padding: 5px;
                border: medium solid black;
            }
        </style>
    </head>
    <body>
        <form method="post" action="http://titan:8080/form">
            <p>
                <label for="apples">Do you like apples:</label>
                <input type="checkbox" id="apples" name="apples"/>
                <span>This will go red when checked</span>
            </p>
            <input type="submit" value="Submit"/>
        </form>
    </body>
</html>
```

<b>使用`:default` 元素选择器</b>

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <meta name="author" content="Adam Freeman"/>
        <meta name="description" content="A simple example"/>
        <link rel="shortcut icon" href="favicon.ico" type="image/x-icon" />
        <style>
            :default {
                outline: medium solid red;
            }
        </style>
    </head>
    <body>
        <form method="post" action="http://titan:8080/form">
            <p>
                <label for="name">Name: <input id="name" name="name"/></label>
            </p>
            <button type="submit">Submit Vote</button>
            <button type="reset">Reset</button>
        </form>
    </body>
</html>
```

<b>使用`:in-range` 和`:out-of-range` 选择器</b>

关于输入验证的一种具体程度更高的变体是选择值限于指定范围的`input` 元素。`:in-range` 选择器匹配位于指定范围内的`input` 元素，`:out-of-range` 选择器匹配位于指定范围之外的`input` 元素。

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <meta name="author" content="Adam Freeman"/>
        <meta name="description" content="A simple example"/>
        <link rel="shortcut icon" href="favicon.ico" type="image/x-icon" />
        <style>
            :in-range {
                outline: medium solid green;
            }
            :out-of-range: {
                outline: medium solid red;
            }
        </style>
    </head>
    <body>
        <form method="post" action="http://titan:8080/form">
            <p>
                <label for="price">
                    $ per unit in your area:
                    <input type="number" min="0" max="100"
                          value="1" id="price" name="price"/>
                </label>
            </p>
            <input type="submit" value="Submit"/>
        </form>
    </body>
</html>
```

#### **`:link` 和`:visited` 选择器**

`:link` 选择器匹配超级链接，`:visited` 选择器匹配用户已访问的超级链接。

| 属性       | 说明                     | CSS版本 |
| :--------- | :----------------------- | :------ |
| `:link`    | 选择链接元素             | 1       |
| `:visited` | 选择用户已访问的链接元素 | 1       |

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <style type="text/css">
            :link {
                border: thin black solid;
                background-color: lightgrey;
                padding: 4px;
                color:red;
            }
            :visited {
                background-color: grey;
                color:white;
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

#### **`:hover` 选择器**

:hover选择器匹配用户鼠标悬停在其上的任意元素。

| 选择器          | `:hover`             |
| --------------- | -------------------- |
| 匹配            | 鼠标悬停在其上的元素 |
| 最低支持CSS版本 | 2                    |

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <style type="text/css">
            :hover {
                border: thin black solid;
                padding: 4px;
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

#### **`:active` 选择器**

`:active` 选择器匹配当前被用户激活的元素。

| 选择器          | `:active`                                                    |
| --------------- | ------------------------------------------------------------ |
| 匹配            | 当前被用户激活的元素，通常意味着用户即将点击（或者按压）该元素 |
| 最低支持CSS版本 | 2                                                            |

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <style type="text/css">
            :active {
                border: thin black solid;
                padding: 4px;
            }
        </style>
    </head>
    <body>
        <a href="http://apress.com">Visit the Apress website</a>
        <p>I like <span>apples</span> and oranges.</p>
        <button>Hello</button>
    </body>
</html>
```

#### **`:focus` 选择器**

`focus` 选择器，它匹配当前获得焦点的元素。

| 选择器          | `:focus`           |
| --------------- | ------------------ |
| 匹配            | 当前获得焦点的元素 |
| 最低支持CSS版本 | 2                  |

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <style type="text/css">
            :focus{
                border: thin black solid;
                padding: 4px;
            }
        </style>
    </head>
    <body>
        <form>
            Name: <input type="text" name="name"/>
            <p/>
            City: <input type="text" name="city"/>
            <p/>
            <input type="submit"/>
        </form>
    </body>
</html>
```

### 2.6  其他选择器

#### **否定选择器**

否定选择器可以对任意选择取反。

| 选择器          | `:not(<选择器>)`         |
| --------------- | ------------------------ |
| 匹配            | 对括号内选择器的选择取反 |
| 最低支持CSS版本 | 3                        |

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <style type="text/css">
            a:not([href*="apress"]) {
                border: thin black solid;
                padding: 4px;
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

#### **`:empty` 选择器**

`:empty` 选择器匹配没有定义任何子元素的元素。

| 选择器          | `:empty`         |
| --------------- | ---------------- |
| 匹配            | 没有子元素的元素 |
| 最低支持CSS版本 | 3                |

#### **`:lang` 选择器**

`:lang` 选择器匹配基于`lang` 全局属性值的元素。

| 选择器          | `:lang(<目标语言>)`             |
| --------------- | ------------------------------- |
| 匹配            | 选择基于`lang` 全局属性值的元素 |
| 最低支持CSS版本 | 1                               |

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <style type="text/css">
            :lang(en) {
                border: thin black solid;
                padding: 4px;
            }
        </style>
    </head>
    <body>
        <a lang="en-us" id="apressanchor" class="class1 class2" href="http://apress.com">
            Visit the Apress website
        </a>
        <p>I like <span lang="en-uk" class="class2">apples</span> and oranges.</p>
        <a lang="en" id="w3canchor" href="http://w3c.org">Visit the W3C website</a>
    </body>
</html>
```

#### **`:target` 选择器**

`:target` 选择器匹配URL片段标识符指向的元素。

| 选择器          | `:target`               |
| --------------- | ----------------------- |
| 匹配            | URL片段标识符指向的元素 |
| 最低支持CSS版本 | 3                       |

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <style type="text/css">
            :target {
                border: thin black solid;
                padding: 4px;
                color:red;
            }
        </style>
    </head>
    <body>
        <a href="http://apress.com">Visit the Apress website</a>
        <p id="mytarget">I like <span>apples</span> and oranges.</p>
        <a id="w3clink" href="http://w3c.org">Visit the W3C website</a>
    </body>
</html>
```

## 3. 边框和背景

### 3.1  基本边框属性

#### border-width` 、`border-style` 和`border-color

| 属性           | 说明                   | 值       |
| :------------- | :--------------------- | :------- |
| `border-width` | 设置边框的宽度         | 参见下表 |
| `border-style` | 设置绘制边框使用的样式 | 参见下表 |
| `border-color` | 设置边框的颜色         | `<颜色>` |

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <meta name="author" content="Adam Freeman"/>
        <meta name="description" content="A simple example"/>
        <link rel="shortcut icon" href="favicon.ico" type="image/x-icon" />
        <style type="text/css">
            p {
                border-width: 5px;
                border-style: solid;
                border-color: black;
            }
        </style>
    </head>
    <body>
    <p>
        There are lots of different kinds of fruit - there are over 500 varieties
        of banana alone. By the time we add the countless types of apples, oranges,
        and other well-known fruit, we are faced with thousands of choices.
    </p>
    </body>
</html>
```

<b>`border-width` 属性的取值</b>

| 值                      | 说明                                                         |
| :---------------------- | :----------------------------------------------------------- |
| `<长度值>`              | 将边框宽度值设为以CSS度量单位（如`em` 、`px` 、`cm` ）表达的长度值 |
| `<百分数>`              | 将边框宽度值设为边框绘制区域的宽度的百分数                   |
| `Thin` `medium` `thick` | 将边框宽度设为预设宽度，这三个值的具体意义是由浏览器定义的，不过，所有浏览器中这三个值代表的宽度依次增大 |

<b>`border-style` 属性的取值</b>

| 值       | 说明                         |
| :------- | :--------------------------- |
| `none`   | 没有边框                     |
| `dashed` | 破折线式边框                 |
| `dotted` | 圆点线式边框                 |
| `double` | 双线式边框                   |
| `groove` | 槽线式边框                   |
| `inset`  | 使元素内容具有内嵌效果的边框 |
| `outset` | 使元素内容具有外凸效果的边框 |
| `ridge`  | 脊线边框                     |
| `solid`  | 实线边框                     |

<b>特定边的边框属性</b>

| 属性                                                         | 说明     | 值                 |
| :----------------------------------------------------------- | :------- | :----------------- |
| `border-top-width` `border-top-style` `border-top-color`     | 定义顶边 | 跟通用属性的值一样 |
| `border-bottom-width` `border-bottom-style` `border-bottom-color` | 定义底边 | 跟通用属性的值一样 |
| `border-left-width` `border-left-style` `border-left-color`  | 定义左边 | 跟通用属性的值一样 |
| `border-right-width` `border-right-style` `border-right-color` | 定义右边 | 跟通用属性的值一样 |

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <meta name="author" content="Adam Freeman"/>
        <meta name="description" content="A simple example"/>
        <link rel="shortcut icon" href="favicon.ico" type="image/x-icon" />
        <style type="text/css">
            p {
                border-width: 5px;
                border-style: solid;
                border-color: black;
                border-left-width: 10px;
                border-left-style: dotted;
                border-top-width: 10px;
                border-top-style: dotted;
            }
        </style>
    </head>
    <body>
    <p>
        There are lots of different kinds of fruit - there are over 500 varieties
        of banana alone. By the time we add the countless types of apples, oranges,
        and other well-known fruit, we are faced with thousands of choices.
    </p>
    </body>
</html>
```

<b>`border` 简写属性</b>

| 属性                                                      | 说明             | 值                     |
| :-------------------------------------------------------- | :--------------- | :--------------------- |
| `border`                                                  | 设置所有边的边框 | `<宽度> <样式> <颜色>` |
| `border-top` `border-bottom` `border-left` `bottom-right` | 设置一条边的边框 | `<宽度> <样式> <颜色>` |

### 3.2  圆角边属性

可以使用边框的`radius` 特性创建圆角边框。

| 属性                                                         | 说明                     | 值                                                     |
| :----------------------------------------------------------- | :----------------------- | :----------------------------------------------------- |
| `border-top-left-radius` `border-top-right-radius` `border-bottom-left-radius` `border-bottom-right-radius` | 设置一个圆角             | 一对长度值或百分数值，百分数跟边框盒子的宽度和高度相关 |
| `border-radius`                                              | 一次设置四个角的简写属性 | 一对或四对长度值或百分数值，由/字符分割                |

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <meta name="author" content="Adam Freeman"/>
        <meta name="description" content="A simple example"/>
        <link rel="shortcut icon" href="favicon.ico" type="image/x-icon" />
        <style type="text/css">
            p {
                border: medium solid black;
                border-top-left-radius: 20px 15px;
            }
        </style>
    </head>
    <body>
    <p>
        There are lots of different kinds of fruit - there are over 500 varieties
        of banana alone. By the time we add the countless types of apples, oranges,
        and other well-known fruit, we are faced with thousands of choices.
    </p>
    </body>
</html>
```

### 3.3  将图像做边框

边框不仅限于用`border-style` 属性定义的样式，我们可以使用图像为元素创建真正的自定义边框。

| 属性                  | 说明                             | 值                                              |
| :-------------------- | :------------------------------- | :---------------------------------------------- |
| `border-image-source` | 设置图像来源                     | `none` 或者`url(<图像>)`                        |
| `border-image-slice`  | 设置切分图像的偏移               | 1～4个长度值或者百分数，受图像的宽度和高度影响  |
| `border-image-width`  | 设置图像边框的宽度               | `auto` 或1～4个长度值或者百分数                 |
| `border-image-outset` | 指定边框图像向外扩展的部分       | 1～4个长度值或者百分数                          |
| `border-image-repeat` | 设置图像填充边框区域的模式       | `stretch` 、`repeat` 和`round` 中的一个或两个值 |
| `border-image`        | 在一条声明中设置所有值的简写属性 | 跟单个属性的值一样，请参考下面的示例            |

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <meta name="author" content="Adam Freeman"/>
        <meta name="description" content="A simple example"/>
        <link rel="shortcut icon" href="favicon.ico" type="image/x-icon" />
        <style type="text/css">
            p {
                -webkit-border-image: url(bordergrid.png) 30 / 50px;
                -moz-border-image: url(bordergrid.png) 30 / 50px;
                -o-border-image: url(bordergrid.png) 30 / 50px;
            }
        </style>
    </head>
    <body>
    <p>
        There are lots of different kinds of fruit - there are over 500 varieties
        of banana alone. By the time we add the countless types of apples, oranges,
        and other well-known fruit, we are faced with thousands of choices.
    </p>
    </body>
</html>
```

<b>`border-image-repeat` 样式的值</b>

| 值        | 说明                                                         |
| :-------- | :----------------------------------------------------------- |
| `stretch` | 拉伸切分图填满整个空间，默认值                               |
| `repeat`  | 平铺切分图填满整个空间（可能导致图片被截断）                 |
| `round`   | 在不截断切分图的情况下，平铺切分图并拉伸以填满整个空间       |
| `space`   | 在不截断切分图的情况下，平铺切分图并在图片之间保留一定的间距以填满整个空间 |

### 3.4  设置元素背景

| 属性                    | 说明                                                         | 值                   |
| :---------------------- | :----------------------------------------------------------- | :------------------- |
| `background-color`      | 设置元素的背景颜色，总是显示在背景图像下面                   | `<颜色>`             |
| `background-image`      | 设置元素的背景图像，如果指定一个以上的图像，则后面的图像绘制在前面的图像下面 | `none` 或`url(图像)` |
| `background-repeat`     | 设置图像的重复样式                                           | 参见下文             |
| `background-size`       | 设置背景图像的尺寸                                           | 参见下文             |
| `background-position`   | 设置背景图像的位置                                           | 参见下文             |
| `background-attachment` | 设置元素中的图像是否固定或随页面一起滚动                     | 参见下文             |
| `background-clip`       | 设置背景图像裁剪方式                                         | 参见下文             |
| `background-origin`     | 设置背景图像绘制的起始位置                                   | 参见下文             |
| `background`            | 简写属性                                                     | 参见下文             |

#### 设置背景图像和颜色

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <meta name="author" content="Adam Freeman"/>
        <meta name="description" content="A simple example"/>
        <link rel="shortcut icon" href="favicon.ico" type="image/x-icon" />
        <style type="text/css">
            p {
                 border: medium solid black;
                 background-color: lightgray;
                 background-image: url(banana.png);
                 background-size: 40px 40px;
                 background-repeat: repeat-x;
            }
        </style>
    </head>
    <body>
    <p>
        There are lots of different kinds of fruit - there are over 500 varieties
        of banana alone. By the time we add the countless types of apples, oranges,
        and other well-known fruit, we are faced with thousands of choices.
    </p>
    </body>
</html>
```

<b>`background-repeat` 属性的值</b>

| 值          | 说明                                                         |
| :---------- | :----------------------------------------------------------- |
| `repeat-x`  | 水平方向平铺图像，图像可能被截断                             |
| `repeat-y`  | 垂直方向平铺图像，图像可能被截断                             |
| `repeat`    | 水平和垂直方向同时平铺图像，图像可能被截断                   |
| `space`     | 水平或者垂直方向平铺图像，但在图像与图像之间设置统一间距，确保图像不被截断 |
| `round`     | 水平或者垂直方向平铺图像，但调整图像大小，确保图像不被截断   |
| `no-repeat` | 禁止平铺图像                                                 |

<b>`background-size` 属性的值</b>

| 值        | 说明                                                         |
| :-------- | :----------------------------------------------------------- |
| `contain` | 等比例缩放图像，使其宽度、高度中较大者与容器横向或纵向重合，背景图像始终包含在容器内 |
| `cover`   | 等比例缩放图像，使图像至少覆盖容器，有可能超出容器           |
| `auto`    | 默认值，图像以本身尺寸完全显示                               |

#### 设置背景图像位置

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <meta name="author" content="Adam Freeman"/>
        <meta name="description" content="A simple example"/>
        <link rel="shortcut icon" href="favicon.ico" type="image/x-icon" />
        <style type="text/css">
            p {
                 border: 10px double black;
                 background-color: lightgray;
                 background-image: url(banana.png);
                 background-size: 40px 40px;
                 background-repeat: no-repeat;
                 background-position: 30px 10px;
            }
        </style>
    </head>
    <body>
    <p>
        There are lots of different kinds of fruit - there are over 500 varieties
        of banana alone. By the time we add the countless types of apples, oranges,
        and other well-known fruit, we are faced with thousands of choices.
    </p>
    </body>
</html>
```

这条声明告诉浏览器将背景图像设置为距离左边界30像素，距离顶部边界10像素。

<b>`background-position` 属性的值</b>

| 值       | 说明                         |
| :------- | :--------------------------- |
| `top`    | 将背景图像定位到盒子顶部边界 |
| `left`   | 将背景图像定位到盒子左边界   |
| `right`  | 将背景图像定位到盒子右边界   |
| `bottom` | 将背景图像定位到盒子底部边界 |
| `center` | 将背景图像定位到中间位置     |

#### 设置背景附着方式

为具有视窗的元素应用背景时，可以指定背景附着内容的方式。使用`background-attachment` 属性可以控制背景的附着方式。

<b>`background-attachment` 属性的值</b>

| 值       | 说明                                   |
| :------- | :------------------------------------- |
| `fixed`  | 背景固定到视窗上，即内容滚动时背景不动 |
| `local`  | 背景附着到内容上，即背景随内容一起滚动 |
| `scroll` | 背景固定到元素上，不会随内容一起滚动   |

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <meta name="author" content="Adam Freeman"/>
        <meta name="description" content="A simple example"/>
        <link rel="shortcut icon" href="favicon.ico" type="image/x-icon" />
        <style type="text/css">
            textarea {
                 border: medium solid black;
                 background-color: lightgray;
                 background-image: url(banana.png);
                 background-size: 60px 60px;
                 background-repeat: repeat;
                 background-attachment: scroll;
            }
        </style>
    </head>
    <body>
    <p>
        <textarea rows="8" cols="30">
        There are lots of different kinds of fruit - there are over 500 varieties
        of banana alone. By the time we add the countless types of apples, oranges,
        and other well-known fruit, we are faced with thousands of choices.
        </textarea>
    </p>
    </body>
</html>
```

#### 设置背景图像的开始位置和裁剪样式

背景的起始点（origin）指定背景颜色和背景图像应用的位置。裁剪样式决定了背景颜色和图像在元素盒子中绘制的区域。`background-origin` 和`background-clip` 属性控制着这些特性。

<b>`background-origin` 和`background-clip` 属性的值</b>

| 值            | 说明                                   |
| :------------ | :------------------------------------- |
| `border-box`  | 在边框盒子内部绘制背景颜色和背景图像   |
| `padding-box` | 在内边距盒子内部绘制背景颜色和背景图像 |
| `content-box` | 在内容盒子内部绘制背景颜色和背景图像   |

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <meta name="author" content="Adam Freeman"/>
        <meta name="description" content="A simple example"/>
        <link rel="shortcut icon" href="favicon.ico" type="image/x-icon" />
        <style type="text/css">
            p {
                 border: 10px double black;
                 background-color: lightgray;
                 background-image: url(banana.png);
                 background-size: 40px 40px;
                 background-repeat: repeat;
                 background-origin: border-box;
                 background-clip: content-box;
            }
        </style>
    </head>
    <body>
    <p>
        There are lots of different kinds of fruit - there are over 500 varieties
        of banana alone. By the time we add the countless types of apples, oranges,
        and other well-known fruit, we are faced with thousands of choices.
    </p>
    </body>
</html>
```

### 3.5  盒子阴影

为元素的盒子添加阴影效果，这要通过`box-shadow` 属性来实现。

#### **`box-shadow` 属性**

| 属性         | 说明           | 值       |
| :----------- | :------------- | :------- |
| `box-shadow` | 为元素指定阴影 | 参见下表 |

<b>`box-shadow` 属性的值</b>

| 值        | 说明                                                         |
| :-------- | :----------------------------------------------------------- |
| `hoffset` | 阴影的水平偏移量，是一个长度值，正值代表阴影向右偏移，负值代表阴影向左偏移 |
| `voffset` | 阴影的垂直偏移量，是一个长度值，正值代表阴影位于元素盒子下方，负值代表阴影位于元素盒子上方 |
| `blur`    | （可选）指定模糊值，是一个长度值，值越大盒子的边界越模糊。默认值为0，边界清晰 |
| `spread`  | （可选）指定阴影的延伸半径，是一个长度值，正值代表阴影向盒子各个方向延伸扩大，负值代表阴影沿相反方向缩小 |
| `color`   | （可选）设置阴影的颜色，如果省略，浏览器会自行选择一个颜色   |
| `inset`   | （可选）将外部阴影设置为内部阴影（内嵌到盒子中）             |

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <meta name="author" content="Adam Freeman"/>
        <meta name="description" content="A simple example"/>
        <link rel="shortcut icon" href="favicon.ico" type="image/x-icon" />
        <style type="text/css">
            p {
                border: 10px double black;

                box-shadow: 5px 4px 10px 2px rgb(170, 11, 11), 4px 4px 6px gray inset;
            }
        </style>
    </head>
    <body>
    <p>
        There are lots of different kinds of fruit - there are over 500 varieties
        of banana alone. By the time we add the countless types of apples, oranges,
        and other well-known fruit, we are faced with thousands of choices.
    </p>
    </body>
</html>
```

### 3.6  应用轮廓

<b>轮廓属性</b>

| 属性             | 说明                             | 值                                        |
| :--------------- | :------------------------------- | :---------------------------------------- |
| `outline-color`  | 设置外围轮廓的颜色               | `<颜色>`                                  |
| `outline-offset` | 设置轮廓距离元素边框边缘的偏移量 | `<长度>`                                  |
| `outline-style`  | 设置轮廓样式                     | 跟`border-style` 属性的值一样，参见表19-4 |
| `outline-width`  | 设置轮廓的宽度                   | `thin` 、`medium` 、`thick` 、`<长度>`    |
| `outline`        | 在一条声明中设置轮廓的简写属性   | `<颜色> <样式> <宽度>`                    |

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
            p {
                width: 30%;
                padding: 5px;
                border: medium double black;
                background-color: lightgray;
                margin: 2px;
                float: left;
            }
            #fruittext {
                outline: thick solid red;
            }
        </style>
    </head>
    <body>
        <p id="fruittext">
            There are lots of different kinds of fruit - there are over 500
            varieties of banana alone. By the time we add the countless types of
            apples, oranges, and other well-known fruit, we are faced with
            thousands of choices.
        </p>
        <button>Outline Off</button>
        <button>Outline On</button>
        <script>
            var buttons = document.getElementsByTagName("BUTTON");
            for (var i = 0; i < buttons.length; i++) {
                buttons[i].onclick = function(e) {
                    var elem = document.getElementById("fruittext");
                    if (e.target.innerHTML == "Outline Off") {
                        elem.style.outline = "none";
                    } else {
                        elem.style.outlineColor = "red";
                        elem.style.outlineStyle = "solid";
                        elem.style.outlineWidth = "thick";
                    }
                };
            }
        </script>
    </body>
</html>
```

## 4. 盒模型

### 4.1  内边距

应用内边距会在元素内容和边框之间添加空白。

#### **内边距属性**

| 属性             | 说明                                     | 值                   |
| :--------------- | :--------------------------------------- | :------------------- |
| `padding-top`    | 为顶边设置内边距                         | 长度值或者百分数     |
| `padding-right`  | 为右边设置内边距                         | 长度值或百分数       |
| `padding-bottom` | 为底边设置内边距                         | 长度值或百分数       |
| `padding-left`   | 为左边设置内边距                         | 长度值或百分数       |
| `padding`        | 简写属性，在一条声明中设置所有边的内边距 | 1～4个长度值或百分数 |

如果使用百分数值指定内边距，百分数总是跟包含块的宽度相关，高度不考虑在内。

示例：

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <meta name="author" content="Adam Freeman"/>
        <meta name="description" content="A simple example"/>
        <link rel="shortcut icon" href="favicon.ico" type="image/x-icon" />
        <style type="text/css">
            p {
                border: 10px double black;
                background-color: lightgray;
                background-clip: content-box;
                padding-top: 0.5em;
                padding-bottom: 0.3em;
                padding-right: 0.8em;
                padding-left: 0.6em;
            }
        </style>
    </head>
    <body>
    <p>
        There are lots of different kinds of fruit - there are over 500 varieties
        of banana alone. By the time we add the countless types of apples, oranges,
        and other well-known fruit, we are faced with thousands of choices.
    </p>
    </body>
</html>
```

### 4.2  外边距

外边距是元素边框和页面上围绕在它周围的所有东西之间的空白区域。围绕在它周围的东西包括其他元素和它的父元素。

#### **外边距属性**

| 属性            | 说明                                     | 值                   |
| :-------------- | :--------------------------------------- | :------------------- |
| `margin-top`    | 为顶边设置外边距                         | 长度值或者百分数     |
| `margin-right`  | 为右边设置外边距                         | 长度值或百分数       |
| `margin-bottom` | 为底边设置外边距                         | 长度值或百分数       |
| `margin-left`   | 为左边设置外边距                         | 长度值或百分数       |
| `margin`        | 简写属性，在一条声明中设置所有边的外边距 | 1～4个长度值或百分数 |

跟内边距属性相似，即使是为顶边和底边应用内边距，百分数值是和包含块的宽度相关的。

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <meta name="author" content="Adam Freeman"/>
        <meta name="description" content="A simple example"/>
        <link rel="shortcut icon" href="favicon.ico" type="image/x-icon" />
        <style type="text/css">
            img {
                border: 4px solid black;
                background: lightgray;
                padding: 4px;
                margin:4px 20px;
            }
        </style>
    </head>
    <body>
        <img src="banana-small.png" alt="small banana">
        <img src="banana-small.png" alt="small banana">
    </body>
</html>
```

### 4.3  元素的尺寸

浏览器会基于页面上内容的流设置元素的尺寸。

#### 尺寸属性

| 属性                     | 说明                                 | 值                                                          |
| :----------------------- | :----------------------------------- | :---------------------------------------------------------- |
| `Width` `Height`         | 设置元素的宽度和高度                 | `auto` 、长度值或者百分数                                   |
| `min-width` `min-height` | 为元素设置最小可接受宽度和高度       | `auto` 、长度值或百分数                                     |
| `max-width` `max-height` | 为元素设置最大可接受宽度和高度       | `auto` 、长度值或百分数                                     |
| `box-sizing`             | 设置尺寸调整应用到元素盒子的哪一部分 | `content-box` 、`padding-box` 、`border-box` 、`margin-box` |

前三个属性的默认值都是`auto` ，意思是浏览器会为我们设置好元素的宽度和高度。你也可以使用长度值和百分数值显式指定尺寸。百分数值是根据包含块的宽度来计算的。

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <meta name="author" content="Adam Freeman"/>
        <meta name="description" content="A simple example"/>
        <link rel="shortcut icon" href="favicon.ico" type="image/x-icon" />
        <style type="text/css">
            div {
                width: 75%;
                height: 100px;
                border: thin solid black;
            }
            img {
                background: lightgray;
                border: 4px solid black;
                margin: 2px;
                height: 50%;
            }
            #first {
                box-sizing: border-box;
                width: 50%;
            }
            #second {
                box-sizing: content-box;
            }
        </style>
    </head>
    <body>
        <div>
            <img id="first" src="banana-small.png" alt="small banana">
            <img id="second" src="banana-small.png" alt="small banana">
        </div>
    </body>
</html>
```

### 4.4  处理内容溢出

内容太大，已经无法完全显示在元素的内容盒中。这时的默认处理方式是内容溢出，并继续显示。可以使用`overflow` 属性改变这种行为。

#### **`overflow` 属性**

| 属性                      | 说明                             | 值                                 |
| :------------------------ | :------------------------------- | :--------------------------------- |
| `overflow-x` `overflow-y` | 设置水平方向和垂直方向的溢出方式 | 参见表20-6                         |
| `overflow`                | 简写属性                         | `overflow` `overflow-x overflow-y` |

`overflow-x` 和`overflow-y` 属性分别设置水平方向和垂直方向的溢出方式，`overflow` 简写属性可在一条声明中声明两个方向的溢出方式。

<b>溢出属性的值</b>

| 值           | 说明                                                         |
| :----------- | :----------------------------------------------------------- |
| `auto`       | 浏览器自行处理溢出内容。通常，如果内容被剪掉就显示滚动条，否则就不显示（这是相较`scroll` 值来说的，设置该值后，无论内容是否溢出都有滚动条） |
| `hidden`     | 多余的部分直接剪掉，只显示内容盒里面的内容。如果用户想看看剪掉的这部分内容，对不起，做不到 |
| `no-content` | 如果内容无法全部显示，就直接移除。主流浏览器都不支持这个值   |
| `no-display` | 如果内容无法全部显示，就隐藏所有内容。主流浏览器都不支持这个值 |
| `scroll`     | 为了让用户看到所有内容，浏览器会添加滚动机制。通常是滚动条，不过这个值跟具体的平台和浏览器相关。即使内容没有溢出也能看到滚动条 |
| `visible`    | 默认值，不管是否溢出内容盒，都显示元素内容                   |

示例：

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <meta name="author" content="Adam Freeman"/>
        <meta name="description" content="A simple example"/>
        <link rel="shortcut icon" href="favicon.ico" type="image/x-icon" />
        <style type="text/css">
            p {
                width: 200px;
                height: 100px;
                border: medium double black;
            }

            #first {overflow: hidden;}
            #second { overflow: scroll;}
        </style>
    </head>
    <body>
        <p id="first">
            There are lots of different kinds of fruit - there are over 500 varieties
            of banana alone. By the time we add the countless types of apples, oranges,
            and other well-known fruit, we are faced with thousands of choices.
        </p>

        <p id="second">
            There are lots of different kinds of fruit - there are over 500 varieties
            of banana alone. By the time we add the countless types of apples, oranges,
            and other well-known fruit, we are faced with thousands of choices.
        </p>
    </body>
</html>
```

### 4.5  控制元素的可见性

使用`visibility` 属性控制元素的可见性。

#### **`visibility` 属性**

| 属性         | 说明             | 值                            |
| :----------- | :--------------- | :---------------------------- |
| `visibility` | 设置元素的可见性 | `collapse` `hidden` `visible` |

<b>`visibility` 属性的值</b>

| 值         | 说明                                 |
| :--------- | :----------------------------------- |
| `collapse` | 元素不可见，且在页面布局中不占据空间 |
| `hidden`   | 元素不可见，但在页面布局中占据空间   |
| `visible`  | 默认值，元素在页面上可见             |

示例：

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <meta name="author" content="Adam Freeman"/>
        <meta name="description" content="A simple example"/>
        <link rel="shortcut icon" href="favicon.ico" type="image/x-icon" />
        <style type="text/css">
            tr > th { text-align:left; background:gray; color:white}
            tr > th:only-of-type {text-align:right; background: lightgray; color:gray}
        </style>
    </head>
    <body>
        <table>
            <tr>
                <th>Rank</th><th>Name</th><th>Color</th><th>Size</th>
            </tr>
            <tr id="firstchoice">
                <th>Favorite:</th><td>Apples</td><td>Green</td><td>Medium</td>
            </tr>
            <tr>
                <th>2nd Favorite:</th><td>Oranges</td><td>Orange</td><td>Large</td>
            </tr>
        </table>
        <p>
            <button>Visible</button>
            <button>Collapse</button>
            <button>Hidden</button>
        </p>
        <script>
            var buttons = document.getElementsByTagName("BUTTON");
            for (var i = 0; i < buttons.length; i++) {
                buttons[i].onclick = function(e) {
                    document.getElementById("firstchoice").style.visibility =
                        e.target.innerHTML;
                };
            }
        </script>
    </body>
</html>
```

### 4.6  元素的盒类型

`display` 属性提供了一种改变元素盒类型的方式，这相应会改变元素在页面上的布局方式。

**`display` 属性的值**

| 值                                                           | 说明                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| `inline`                                                     | 盒子显示为文本行中的字                                       |
| `block`                                                      | 盒子显示为段落                                               |
| `inline-block`                                               | 盒子显示为文本行                                             |
| `list-item`                                                  | 盒子显示为列表项，通常具有项目符号或者其他标记符（如索引号） |
| `run-in`                                                     | 盒子类型取决于周围的元素                                     |
| `compact`                                                    | 盒子的类型为块或者标记盒（跟`list-item` 类型产生的类似）。本书撰写时主流浏览器都不支持这个值 |
| `flexbox`                                                    | 这个值跟弹性盒布局相关，会在第21章介绍                       |
| `table` `inline-table` `table-row-group` `table-header-group` `table-footer-group` `table-row` `table-column-group` `table-column` `table-cell` `table-caption` | 这些值跟表格中的元素布局相关，详细信息参见第21章             |
| `ruby` `ruby-base` `ruby-text` `ruby-base-group` `ruby-text-group` | 这些值跟带`ruby` 注释的文本布局相关                          |
| `none`                                                       | 元素不可见，且在页面布局中不占空间                           |

#### 块级元素

将`display` 属性设置为`block` 值会创建一个块级元素。块级元素会在垂直方向跟周围元素有所区别。通常在元素前后放置换行符也能达到这种效果，在元素和周围元素之间制造分割的感觉，就像文本中的段落。

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <meta name="author" content="Adam Freeman"/>
        <meta name="description" content="A simple example"/>
        <link rel="shortcut icon" href="favicon.ico" type="image/x-icon" />
        <style type="text/css">
            p {border: medium solid black}
            span {
                display: block;
                border: medium double black;
                margin: 2px;
            }
        </style>
    </head>
    <body>
        <p>
            There are lots of different kinds of fruit - there are over 500 varieties
            of banana alone. By the time we add the countless types of apples, oranges,
            and other well-known fruit, we are faced with thousands of choices.
        </p>
        <p>
            One of the most interesting aspects of fruit is the variety available in
            each country. <span>I live near London</span>, in an area which is known for
            its apples. When travelling in Asia, I was struck by how many different
            kinds of banana were available - many of which had unique flavours and
            which were only avaiable within a small region.
        </p>
    </body>
</html>
```

#### 行内元素

将`display` 属性设置为`inline` 值会创建一个行内元素，它在视觉上跟周围内容的显示没有区别。

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <meta name="author" content="Adam Freeman"/>
        <meta name="description" content="A simple example"/>
        <link rel="shortcut icon" href="favicon.ico" type="image/x-icon" />
        <style type="text/css">
            p {
                display: inline;
            }
            span {
                display: inline;
                border: medium double black;
                margin: 2em;
                width: 10em;
                height: 2em;
            }
        </style>
    </head>
    <body>
        <p>
            There are lots of different kinds of fruit - there are over 500 varieties
            of banana alone. By the time we add the countless types of apples, oranges,
            and other well-known fruit, we are faced with thousands of choices.
        </p>
        <p>
            One of the most interesting aspects of fruit is the variety available in
            each country. <span>I live near London</span>, in an area which is known for
            its apples. When travelling in Asia, I was struck by how many different
            kinds of banana were available - many of which had unique flavours and
            which were only avaiable within a small region.
        </p>
    </body>
</html>
```

#### 行内-块级元素

将`display` 属性设置为`inline-block` 值会创建一个其盒子混合了块和行内特征的元素。盒子整体上作为行内元素显示，这意味着垂直方向上该元素和周围的内容并排显示，没有区别。但盒子内部作为块元素显示，这样，`width` 、`height` 和`margin` 属性都能应用到盒子上。

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <meta name="author" content="Adam Freeman"/>
        <meta name="description" content="A simple example"/>
        <link rel="shortcut icon" href="favicon.ico" type="image/x-icon" />
        <style type="text/css">
            p {
                display: inline;
            }
            span {
                display: inline-block;
                border: medium double black;
                margin: 2em;
                width: 10em;
                height: 2em;
            }
        </style>
    </head>
    <body>
        <p>
            There are lots of different kinds of fruit - there are over 500 varieties
            of banana alone. By the time we add the countless types of apples, oranges,
            and other well-known fruit, we are faced with thousands of choices.
        </p>
        <p>
            One of the most interesting aspects of fruit is the variety available in
            each country. <span>I live near London</span>, in an area which is known for
            its apples. When travelling in Asia, I was struck by how many different
            kinds of banana were available - many of which had unique flavours and
            which were only avaiable within a small region.
        </p>
    </body>
</html>
```

#### 插入元素

`display` 属性设置为`run-in` 值会创建一个这样的元素：其盒子类型取决于周围元素。通常，浏览器必须评估以下三种情况，以确定插入框的特性。

(1) 如果插入元素包含一个`display` 属性值为`block` 的元素，那么插入元素就是块级元素。

(2) 如果插入元素的相邻兄弟元素是块级元素，那么插入元素就是兄弟元素中的第一个行内元素。

(3) 其他情况下，插入元素均作为块级元素对待。

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <meta name="author" content="Adam Freeman"/>
        <meta name="description" content="A simple example"/>
        <link rel="shortcut icon" href="favicon.ico" type="image/x-icon" />
        <style type="text/css">
            p {
                display: inline;
            }
            span {
                display: run-in;
                border: medium double black;
            }
        </style>
    </head>
    <body>
        <span>
            There are lots of different kinds of fruit - there are over 500 varieties
            of banana alone.
        </span>
        <p>
            By the time we add the countless types of apples, oranges,
            and other well-known fruit, we are faced with thousands of choices.
        </p>
    </body>
</html>
```

#### 隐藏元素

将`display` 属性设置为`none` 值就是告诉浏览器不要为元素创建任何类型的盒子，也就是说元素没有后代元素。这时元素在页面布局中不占据任何空间。

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
        <p id="toggle">
            There are lots of different kinds of fruit - there are over 500 varieties
            of banana alone. By the time we add the countless types of apples, oranges,
            and other well-known fruit, we are faced with thousands of choices.
        </p>
        <p>
            One of the most interesting aspects of fruit is the variety available in
            each country. <span>I live near London</span>, in an area which is known for
            its apples. When travelling in Asia, I was struck by how many different
            kinds of banana were available - many of which had unique flavours and
            which were only avaiable within a small region.
        </p>
        <p>
            <button>Block</button>
            <button>None</button>
        </p>

        <script>
            var buttons = document.getElementsByTagName("BUTTON");
            for (var i = 0; i < buttons.length; i++) {
                buttons[i].onclick = function(e) {
                    document.getElementById("toggle").style.display=
                        e.target.innerHTML;
                };
            }
        </script>
    </body>
</html>
```

### 4.7  浮动盒子

可以使用`float` 属性创建浮动盒，浮动盒会将元素的左边界或者右边界移动到包含块或另一个浮动盒的边界。

#### **`float` 属性**

| 属性    | 说明               | 值                    |
| :------ | :----------------- | :-------------------- |
| `float` | 设置元素的浮动样式 | `left` `right` `none` |

**`float` 属性的值**

| 值      | 说明                                                         |
| :------ | :----------------------------------------------------------- |
| `left`  | 移动元素，使其左边界挨着包含块的左边界，或者另一个浮动元素的右边界 |
| `right` | 移动元素，使其右边界挨着包含块的右边界，或者另一个浮动元素的左边界 |
| `none`  | 元素位置固定                                                 |

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <meta name="author" content="Adam Freeman"/>
        <meta name="description" content="A simple example"/>
        <link rel="shortcut icon" href="favicon.ico" type="image/x-icon" />
        <style>
            p.toggle {
                float:left;
                border: medium double black;
                width: 40%;
                margin: 2px;
                padding: 2px;
            }
        </style>
    </head>
    <body>
        <p class="toggle">
            There are lots of different kinds of fruit - there are over 500 varieties
            of banana alone. By the time we add the countless types of apples, oranges,
            and other well-known fruit, we are faced with thousands of choices.
        </p>
        <p class="toggle">
            One of the most interesting aspects of fruit is the variety available in
            each country. I live near London, in an area which is known for
            its apples.
        </p>
        <p>
            When travelling in Asia, I was struck by how many different
            kinds of banana were available - many of which had unique flavours and
            which were only available within a small region.
        </p>
        <p>
            <button>Left</button>
            <button>Right</button>
            <button>None</button>
        </p>
        <script>
            var buttons = document.getElementsByTagName("BUTTON");
            for (var i = 0; i < buttons.length; i++) {
                buttons[i].onclick = function(e) {
                    var elements = document.getElementsByClassName("toggle");
                    for (var j = 0; j < elements.length; j++) {
                        elements[j].style.cssFloat = e.target.innerHTML;
                    }
                };
            }
        </script>
    </body>
</html>
```

#### 阻止浮动元素堆叠

<b>`clear` 属性</b>

| 属性    | 说明                                                     | 值                           |
| :------ | :------------------------------------------------------- | :--------------------------- |
| `clear` | 设置元素的左边界、右边界或左右两个边界不允许出现浮动元素 | `left` `right` `both` `none` |

**`clear` 属性的值**

| 值      | 说明                               |
| :------ | :--------------------------------- |
| `left`  | 元素的左边界不能挨着另一个浮动元素 |
| `right` | 元素的右边界不能挨着另一个浮动元素 |
| `both`  | 元素的左右边界都不能挨着浮动元素   |
| `none`  | 元素的左右边界都可以挨着浮动元素   |

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <meta name="author" content="Adam Freeman"/>
        <meta name="description" content="A simple example"/>
        <link rel="shortcut icon" href="favicon.ico" type="image/x-icon" />
        <style>
            p.toggle {
                float:left;
                border: medium double black;
                width: 40%;
                margin: 2px;
                padding: 2px;
            }

            p.cleared {
                clear:left;
            }

        </style>
    </head>
    <body>
        <p class="toggle">
            There are lots of different kinds of fruit - there are over 500 varieties
            of banana alone. By the time we add the countless types of apples, oranges,
            and other well-known fruit, we are faced with thousands of choices.
        </p>
        <p class="toggle cleared">
            One of the most interesting aspects of fruit is the variety available in
            each country. I live near London, in an area which is known for
            its apples.
        </p>
        <p>
            When travelling in Asia, I was struck by how many different
            kinds of banana were available - many of which had unique flavours and
            which were only avaiable within a small region.
        </p>

        <p>
            <button>Left</button>
            <button>Right</button>
            <button>None</button>
        </p>

        <script>
            var buttons = document.getElementsByTagName("BUTTON");
            for (var i = 0; i < buttons.length; i++) {
                buttons[i].onclick = function(e) {
                    var elements = document.getElementsByClassName("toggle");
                    for (var j = 0; j < elements.length; j++) {
                        elements[j].style.cssFloat = e.target.innerHTML;
                    }
                };
            }
        </script>
    </body>
</html>
```

