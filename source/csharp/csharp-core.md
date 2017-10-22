---
title: "CSharp Core - Points"
layout: page
date: 2017-09-16 22:56:12
---

[TOC]

## Data type(数据类型) ##

### Value type ###

### Reference type ###

## Flow Control(流控制) ##

Contains `condition statement`, `loop statement` and `rediect statement`

### Conditon ###

> `if-else`, `switch-case`

### Loop ###

> `for`, `while`, `do-while`, `foreach`

> Comparte `for` & `foreach`, `while` & `do-while`

```csharp
for (initializer; condition; iterator)
    statement(s)
```

### Rediect ###

> `goto`, `break`, `continue`, `return`

## Enum(枚举) ##

## Namespace(命名空间) ##

## Compile(编译) ##

## Preprocess(预处理) ##

* `#define` & `#undef`
* `#if` & `#elif` & `#else` & `#endif`
* `#warning` & `#error`
* `#region` & `endregion`
* `#line`
* `#pragma`
> `#pragma`指令可以抑制或还原指定的编译警告。
```csharp
#pragma warning disable 169
public class MyClass
{
    //statement    
}
#pragma warning restore 169
```

[[Index Charpter: Core] <--](../csharp/csharp-core.html) | [--> [Next Charpter: object]](../csharp/csharp-object.html)