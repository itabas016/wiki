---
title: "CSharp Object"
layout: page
date: 2017-09-17 01:16:13
---

[TOC]

## Class & Struct ##

> `class` 与 `struct`的区别是在内存中的存储方式，访问方式和继承上的不同。

> `class` 是存储在堆(heap)上的引用类型(reference), `struct` 是存储在栈(stack)上的值类型(value)

> `struct` 不支持继承(extend), `System.Object` -> `System.ValueType` -> `struct` 

### function ###

> Contains `method(parameter??)`, `property`, `constructor` 三大部分. 

> 其中parameter关键字 `ref` & `out`注意使用场景. 另外还有方法的重载(virtual&override)与扩展(extention). 对于 `constructor`, 特别是静态构造函数, 它不包含参数，并且只会被系统调用一次.

> 


[[Previous Charpter: Core] <--](../csharp/csharp-core.html) | [--> [Next Charpter: Object]](../csharp/csharp-object.html)