---
title: "CSS语法梳理"
layout: page
date: 2017-08-12 12:33:43
---

[TOC]

## CSS Line Mode(套用方式) ##

* 行內套用 (Inline)
> `tag`内部直接声明`style`
``` html
<p style='font-family:verdana; font-size:15px;'>This is font size 15.</p>
```
* 嵌入套用 (Embed)
> `style`嵌入`header`
```html
<head> 
  <style type="text/css"> 
    div { 
      background-color:#FF0000; 
    } 
  </style> 
</head>
```
* 外部连接套用 (External Link)
> 常用的一种方式，在`header`里面声明`css link`
``` html
<link rel=stylesheet type="text/css" href="css/main.css">
```
* 导入套用 (Import)
> 现在已经不怎么用了

## CSS Cascading(串接) ##

> 若一个`html`包含多个`css`, 从高到底的优先级是：

> * 內行套用的样式表 (Inline stylesheet)
> * 嵌入套用的样式表 (Embedded stylesheet)
> * 导入套用的样式表 (Imported stylesheet)
> * 外部连接套用的样式表 (Linked stylesheet)
> * 浏览器本身的样式表 (Browser's own stylesheet)

## CSS Class与Id ##

> 这两者最大的不同，是在于 ID 选择器在一个 HTML 文件中只能被使用一次，而 Class 选择器在一个 HTML 文件中可以被使用多次。第二个不同的地方，是 ID 选择器可以被 Javascript 中的 GetElementByID 函数所使用，而 Class 选择器无法被 Javascript 使用到。

> 并沒有什么固定的规则，来決定什么时候要用 ID 及什么时候要用 Class。建议是尽量用 Class，因为最灵活 (同一个 HTML 文件可以利用这类的选择器多次)。唯一的例外，是当你要用 Javascript 的 GetElementByID 函数时。在這个情况下，你就应该要用 ID。

## CSS Div与Span ##

> Div是区块级的容器(block-level container)，在`</div>`后会换行。

> Span可以涵盖更细小的元素，常用于调整单一文字样式。在`</span>`后不会换行。

## CSS Length Units(长度单位)) ##

> Length units in CSS fall under two categories: 相对单位(relative units) and 绝对单位(absolute units).

* Relative units include:

> * px: pixels
> * em: em
> * ex: 字母高度(height of letter x)

* Absolute units include:

> * in: inches
> * cm: centimeters
> * mm: millimeters
> * pt: points, 1 pt = 1/72 in
> * pc: picas, 1 pc = 12 pt

> 如果没有注明单位，则默认单位是px(If no unit is specified, the default unit is px).

![css-length-units](../static/images/css-length-units.png)

## CSS Box Mode(盒子模式) ##

Box Model relevant command:

* Margin
* Border
* Padding

![css-box-model](../static/images/css-box-model.png)

## CSS Margin ##

> `margin: [margin-top] [margin-right] [margin-bottom] [margin-left]`

> 共有三种方式设定，分别是长度(length),百分比(oercent),自适应(auto) 

## CSS Border ##

* `border-style`
* `border-width`
* `border-color`
* `border-top-, border-left-, border-bottom-, border-right-`

#### border-style ####

> 边框形状

![css-border-style](../static/images/css-border-style.png)

#### border-width ####

> 边框宽度, `border-width: [thin] | [medium] | [thick] | [5px]`

#### border-color ####

> 边框颜色, `border-color: [red] | [#0000FF]`

#### border-child ####

> `[border-top-] | [border-left-] | [border-bottom-] | [border-right-] style` 子属性, 将边框分拆为四条边逐个设置。

## CSS Padding ##

> 属性值设置和margin相同, 只是用于不同的作用域。

> `padding: [padding-top] [padding-right] [padding-bottom] [padding-left]`

* padding-top
* padding-right 
* padding-bottom 
* padding-left

Examples:

`#container1 { padding: 15px 2em 30px 5%; }`

`#container2 { padding: 30px 10em 20px; }`

`#container3 { padding: 10px 5px; }`

`#container4 { padding: 30em; }`

## CSS Background ##

* background-color
* background-image
* background-repeat
* background-attachment
* background-position

另外这几个属性常遗忘，参照这里，快速翻阅 -->[background property](../css/css_element_general_reminder.html#bg)


## CSS Font ##

* font-family
* font-size
* font-weight
* font-style
* font-variant

`font-family`: 可以设置多个, 根据浏览器支持的字体属性来确定显示哪个属性。

> `p { font-family: Arial, Verdana, "New Times Roman" }`

`font-size`: 可参照length units, 也可以简单设置 

> `p { font-size: [xx-large] | [x-large] | [large] | [medium] | [small] | [x-small] | [xx-small] }`

`font-weight`: 设置字体厚度, value: 100~900, 也可以简单设置 

> `p { font-weight: bold | bolder | normal }`

`font-style`: 设置字体是否为斜体字 

> `p { font-style: italic | oblique }`

`font-variant`: 设置文字是否以小型大写字体(small caps), 字母都是大写，字体比normal小一些 

> `p { font-variant: small-caps | normal }`

## CSS Link ##

* a:link: 初始样式。
* a:visited: 访问过样式。
* a:hover: 滑过不点击样式。
* a:active: 点击时样式。

point梳理参照这里，快速翻阅 -->[a-link property](../css/css_element_general_reminder.html#a)

## CSS List Style ##

* list-style-type
* list-style-position
* list-style-image
* list-style

`list-style-type`: 最常见设定值: 

> * none (无)
> * disc (全黑圆圈)
> * circle (空心圆圈)
> * square (正方形)

`list-style-position`: 属性设定值为 `inside | outside` 使用outside属于标准缩进

`list-style-image`: format: `list-style-image:url([image_url]);`

## CSS Position ##

* position
* top
* right
* left
* bottom
* overflow
* z-index

#### position ####

* static (预设值): 这代表元素会被放在预设的地方。如果 position 的值是被设定为 statics 的话，那 top、bottom、left、和 right 的值就都沒有意义了。
* absolute: 这代表元素会被放在浏览器內的某个位置 (依 top、bottom、left、和 right 的值而定)。当使用者将网页往下拉时，元素也会跟著改变位置。
* relative: 这代表元素被放的地方将会与预设的地方有所不同。不同的程度是依照 top、bottom、left、和 right 的值而定。
* fixed: 这代表元素会被放在浏览器內的某个位置 (依 top、bottom、left、和 right 的值而定)。当使用者将网页往下拉时，元素的位置不会改变。

#### direction ####

每一个方向(direction: top, right, bottom, left)的位置值可以是长度(length), 百分比(percent)或自适应(auto)
``` css
p { 
  position:relative; 
  top:10%; 
  right:50px;
  bottom: 10%;
  left:50px;   
}
```
#### overflow ####

`overflow` 用来设定当内容放不下的时候, 采用何种方式显示内容。

* visible: 内容完全呈现，不管放得下还是放不下。
* hidden: 放不下的内容会不显示出来。
* scroll: 无论内容放下放不下，会加上上下左右滚动轴的功能。
* auto: 当内容放不下的时候，会加上滚动轴的功能。 

#### z-index ####

`z-index` 用来决定元素重叠的顺序, 值比较打的那个元素会被放在上面。

## CSS Text ##

* direction
* letter-spacing
* line-height
* text-align
* text-decoration
* text-indent
* text-transform
* word-spacing

`direction` 用来设定文字的方向
> `p { direction: ltr | rtl }` ltr: 从左侧开始input, rtl则相反

`letter-spacing` 用来设定每一个字母之间的空间
> `p { letter-spacing: 5px; }`

`word-spacing` 用来设定每个字与每个字之间的距离
> `p { word-spacing: 5px; }`

**PS.The difference is word and letter**

`line-height` 用来设定每一行之间的空间
> `p { line-height: 8px; }`

#### text-align ####

文字对齐方式

* left: 靠左对齐
* right：靠右对齐
* center：向中間对齐
* justify：左右对齐

#### text-decoration ####

文字装饰(style)

* underline: 文字下加上一条线。
* overline: 文字上加上一条线。
* line-through: 文字中间加上一条线划过去。
* blink: 让文字闪烁。

#### text-indent ####

每一段文字的第一行前面留多少空间(缩进-indent)
> `p { text-indent: 15px; }`

#### text-transform ####

控制大小写如何展现

* capitalize：每一个字的第一个字母都以大写展现，其他的字母则不改变。
* uppercase：所有的字母都以大写展现。
* lowercase：所有的字母都以小写展现。
* none：大小写不做任何改变。

## CSS Float and Clear ##

* CSS Float

> `span { float: left | right | none }` float 会影响整个页面的布局

* CSS Clear

> `span { clear: left | right | both | none }` 与float相反, 专门抵消float的作用

Reference: [css-tutorial](http://www.1keydata.com/css-tutorial/)