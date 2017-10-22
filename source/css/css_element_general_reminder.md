---
title: "CSS遗忘项"
layout: page
date: 2017-04-05 22:25:11
---

[TOC]

## bg 用图片做背景自适应屏幕 ##

如果采用`background-image`设置不是很理想, 即便设置了`background-position`和`background-size`以及`background-repeat`, 仍不能做到完美自适应屏幕。
``` css
body{
	background-image: url('img/fake.jpg');
    background-position: center;
    background-attachment: fixed;
    background-size: auto;
    background-repeat: no-repeat;
}
```
额外补充几个`background`属性：

`background-size: length|percentage|cover|contain;`

* length	设置背景图像的高度和宽度。
* percentage以父元素的百分比来设置背景图像的宽度和高度
* cover		把背景图像扩展至足够大，以使背景图像完全覆盖背景区域。
* contain	把图像图像扩展至最大尺寸，以使其宽度和高度完全适应内容区域

`background-attachment: scroll|fixed|inherit;`

* scroll	默认值。背景图像会随着页面其余部分的滚动而移动。
* fixed		当页面的其余部分滚动时，背景图像不会移动。
* inherit	规定应该从父元素继承 `background-attachment` 属性的设置。

`background-repeat: repeat|repeat-x|repeat-y|no-repeat|inherit;`

* repeat	默认。背景图像将在垂直方向和水平方向重复。
* no-repeat	背景图像将仅显示一次。
* inherit	规定应该从父元素继承 `background-repeat` 属性的设置。

所以可以采用div作为底层图层来填充, code如下：

``` html
<div id="wb-bg">
	<img src="img/fake.jpg" class="bg-img" />
</div>
```
``` css
body{
    margin: 0;
}
#wb-bg{
    position: fixed; 
    width:100%; 
    height:100%; 
    z-index:-1
}
.bg-img{
    height: 100%; 
    width: 100%
}
```

## ul li 元素横排显示 ##

``` css
ul{
    list-style-type: none;
}
li{
    list-style: none;
    display: inline;
}
```

## margin padding 区别 ##

> `margin`用来设置页面中一个元素所占空间的边缘到相邻元素之间的距离

* 如果提供全部四个参数值，将按上－右－下－左的顺序作用于四边。
* 如果只提供一个，将用于全部的四边。
* 如果提供两个，第一个用于上－下，第二个用于左－右。
* 如果提供三个，第一个用于上，第二个用于左－右，第三个用于下

``` css
body{
	margin: .5em .5em 0 1.5em;
}
```

> `padding`用来设置元素内容到元素边界的距离。

* 如果提供全部四个参数值，将按上－右－下－左的顺序作用于四边。
* 如果只提供一个，将用于全部的四条边。
* 如果提供两个，第一个用于上－下，第二个用于左－右。
* 如果提供三个，第一个用于上，第二个用于左－右，第三个用于下。
* 内联对象要使用该属性，必须先设定对象的height或width属性，或者设定position属性为absolute。不允许负值。

``` css
body{
	padding: 1em .5em .5em 1em;
}
```

## icon 圆形图标 ##

> `border-radius` 属性是一个简写属性，用于设置四个 `border-*-radius` 属性。

> 用法：`border-radius: 1-4 length|% / 1-4 length|%;`, sample 参照下面两种写法。

* sample 1
``` css
border-radius:2em;
```
``` css
border-top-left-radius:2em;
border-top-right-radius:2em;
border-bottom-right-radius:2em;
border-bottom-left-radius:2em;
```

* sample 2
``` css
border-radius: 2em 1em 4em / 0.5em 3em;
```
``` css
border-top-left-radius: 2em 0.5em;
border-top-right-radius: 1em 3em;
border-bottom-right-radius: 4em 0.5em;
border-bottom-left-radius: 1em 3em;
```

``` css
.icon{
    padding-top: .5em;
    border-radius: 50%;
    -webkit-background-size: 96px 96px;
    height: 96px;
    width: 96px;
}
```

## a 超链接变色 ##

* `a:link`, `a:hover`, `a:visited`, `a:active`这几个元素, 定义CSS时候的顺序不同, 也会直接导致链接显示的效果不同。
* 鼠标经过的"未访问链接"同时拥有`a:link, a:hover`两种属性, 后面的属性会覆盖前面的属性定义；
* 鼠标经过的"已访问链接"同时拥有`a:visited, a:hover`两种属性, 后面的属性会覆盖前面的属性定义；
* 正确顺序是`a:link, a:visited, a:hover, a:active` 也可以解释成"爱恨原则" -> (LoVe/HAte)

PS: [rgb颜色参数](https://www.w3schools.com/colors/colors_picker.asp)

``` css
a:link {
 color: #0000ff;
 text-decoration: none
}
a:visited {
 color: #990099;
 text-decoration: none
}
a:hover {
 color: #ff0000;
 text-decoration: underline;
}
a:active {
 color: #ff7f24;  
 text-decoration: underline;
}
```