---
title: "CSharp Core - Points"
layout: post
date: 2017-09-16 22:56:12
toc: true
---

> 大致性过一遍知识点

## Data type(数据类型) ##

C#是一种类型安全的编程语言，CLR支持两种类型：**引用类型(reference type)**和**(struct type)**

* 内存必须从托管堆上分配
* 堆上分配的每个对象都有一些额外的成员，这些成员必须初始化
* 对象中的其他字节总是设为零
* 从托管堆上分配一个对象时，可能强制执行一次GC操作

所有的对象类型都是从`System.Object`派生的。所有的结构都是抽象类型`System.ValueType`的直接派生类。所有的枚举都是从`System.Enum`抽象类型派生。

* 值类型都是隐式密封的，不需要从其他任何类型**继承**，也不会**派生**出其他任何类型。
* 值类型具有基元类型的行为，是不可变类型，其字段也可简单标记为`readonly`.
* 自定义值类型必须重写`Equals`和`GetHashCode`方法，并提供它们的**显式实现**。

**PS:** 如果一个类型重写了`GetHashCode`方法，便不能调用它来获取对象的一个唯一ID, 但是通过系统内置的`System.Runtime.CompilerServices`的`RuntimeHelpers`类提供的公共静态方法`GetHashCode`来实现。

如何检查对象的同一性，可以通过`Object`静态方法`ReferenceEquals`比较实现。

在内存分配上，引用类型(reference)是存储在堆(heap)上的, 值类型(struct)是存储在栈(stack)上的。

值类型的**装箱(box)**与**拆箱(unbox)**
通常会有代码装箱和拆箱次数的计算。

### 类型成员及特性 ###

通常包括`常量`, `字段`, `构造函数`, `方法`, `属性`, `事件`等成员构成。

成员可访问性， 看官方注解：

* `public`      The type or member can be accessed by any other code in the same assembly or another assembly that references it.
* `private`     The type or member can be accessed only by code in the same class or struct.
* `protected`   The type or member can be accessed only by code in the same class, or in a class that is derived from that class.
* `internal`    The type or member can be accessed by any code in the same assembly, but not from another assembly.
* `protected internal`  The type or member can be accessed by any code in the assembly in which it is declared, or from within a derived class in another assembly.
* `private protected`   The type or member can be accessed only within its declaring assembly, by code in the same class or in a type that is derived from that class.

字段修饰符`readonly`和`const`区别：

* `readonly`字段只能在一个构造器方法中写入，但是利用反射可以修改`readonly`字段
* `const`永远不会改变。

类或方法的修饰符`abstract`, `sealed`, `static`, `new`, `virtual`, `override`，注意其用法与区别

扩展方法(extension)是在静态类下的静态方法在第一个参数前加上`this`关键字。

并且`C#`只支持扩展方法，不支持其他扩展属性或扩展操作符之类。

### 静态类(static) ###

* 静态类不能实现任何接口，因为只有使用类的一个实例时才会调用类的接口。
* 静态类只能定义静态成员。
* 静态类布恩那个作为字段， 方法参数或局部变量使用。

静态类和非静态类如何实现**单例模式(Singleton)**

## 参数(Parameter) ##

`ref`和`out`参数关键字区别：

* 两者都可以作为参数进行引用传递。
* `ref`关键字的参数调用必须赋初始值，而`out`不用
* `ref`属于引用类型指针，但在做值类型传递时不会进行装箱操作。
* `ref`和`out`都不能用于异步方法(通过使用 async 修饰符定义)和迭代器方法(包括`yield return`或`yield break`语句)

用于`swap`交换的经典方法：

```csharp
public static void Swap<T>(ref T a, ref T b)
{
    T t = b;
    b = a;
    a = t;
}
```

不要混淆`dynamic`和`var`，可以使用`dynamic`定义局部变量, 字段和参数。表达式不能转型为`var`,但能转型为`dynamic`

`params`关键字向方法传递可变数量的参数，eg: `params Object[] objects`, 是因为`params`实现了`System.ParamArrayAttribute`参数引用的一个实例。

声明方法参数类型应尽量指定最弱的类型，最好是`interface`而不是`class`, eg: `IEnumerable<T>`与`List<T>`, 而方法的返回类型声明要根据具体情况而定。eg: `FileStream`和`Stream`, `List<String>`和`IList<String>`.

## 属性(Property) ##

属性与方法的区别:

属性可以包括只读和写保护操作，方法有重载功能

`Tuple`有哪些好处，元组字段属性只读。

实现`IEnumerable<T>`接口的属性

`public get`和`protected set`使用

## 泛型(Generic) ##

使用泛型有哪些好处：
* 源代码保护
* 类型安全
* 代码清晰，减少了强制类型之间的相互转换
* 类型参数化，提高代码复用，提升程序性能

`List<T>`继续了哪些接口: `IList<T>`, `ICollection<T>`, `IEnumerable<T>`, `IList`， `ICollection`, `IEnumerable`

泛型比较，`List`和`ArrayList`
* `ArrayList`属于`System.Collections`, 并且也继承了`IList`, `ICollection`, `IEnumerable`等接口，在对象声明时不需要指定长度，可以动态扩充和收缩。
* `ArrayList`允许存储多类型字段，所以属于**类型非安全**，在存储或检索值类型时通常发生装箱和取消装箱操作，带来很大的性能耗损。

其他泛型集合类：
* BigList<T>    有序的，操作100个以上数据项时效率高
* Bag<T>        无序的，允许重复
* OrderedBag<T> 有序的，允许重复
* Set<T>        无序的，不允许重复
* OrderedSet<T> 有序的，不允许重复
* Deque<T>      双端队列，在起始处insert/remove比List快

另外还有`OrderedDictionary<TKey, TValue>`,`MultiDictionary<TKey, TValue>`等字典系泛型集合。

泛型约束:
* 主要约束

可以是一个引用类型，标识没有密封的类，但不能指定以下特殊引用类型`System.Object`, `System.Array`, `System.Delegate`, `System.ValueType`, `System.Enum`...

* 两个特殊的主要约束`class`和`struct`
* 次要约束

主要表示接口类型，多数体现在泛型约束上
``` csharp
public static List<TBase> ConvertIList<T, TBase>(IList<T> list)
    where T: TBase {
        List<TBase> baseList = new List<TBase>(list.count);
        // todo sth
        return baseList;
    }
```
* 构造器约束

一个类型参数可以指定0或1个构造器约束，但不能同时指定`struct`约束和构造器约束
``` csharp
internal sealed class ConstructorConstraint<T> where T : new() {
    public static T Factory() {
        // ...
        return new T();
    }
}
```

## 接口(Interface)##

如果接口方法是`sealed`派生类不能重写它。但派生类可以重新继承同一个接口，并为该接口方法提供自己的实现。

``` csharp
internal class Base : IDisposable {
    //this method is sealed
    public void Dispose() {
        // todo sth
    }
}

internal class Derived : Base, IDisposable {
    // using new to implements
    new public void Dispose() {
        // todo sth
        // and can invoke base Dispose method
        base.Dispose();
    }
}
```

隐式接口和显示接口(Explicit)的实现。

* 注意定义显示接口方法时不允许指定可访问性，但在编译时其访问性会被自动设为`private`, 类似的通过`IDisposable.Dispose()`调用。
* 另外一个显示接口方法不能标记为`virtual`, 它不可被重写。
* 显示接口方法不能由派生类型调用

设计层面使用接口还是基类:

* IS-A vs CAN-DO 

类型只能继承一个实现，如果派生类型不具有与基类型的IS-A关系，就不应使用基类而使用接口。如果多种对象类型都具有CAN-DO功能就是用接口。

* 易于使用

基类提供基本的实现，派生类直接继承或稍作修改，但是接口的实现必须实现所有成员。

* 一致性和版本控制层面

## 字符(Char)与字符串(String) ##

首先各类型所占字节数

* `byte(1)`, `short(2)`, `int(4)`, `long(8)`
* `float(4)`, `double(8)`
* `boolean(1)`
* `char(2)`
* `unicode(2)`

`System.Char`提供了两个公共常量字段`MinValue('\0)`和`MaxValue('\uffff)`

调用静态方法`GetUnicodeCategory`可以返回`System.Globalization.UnicodeCategory`枚举类型的值。另外也可通过调用其他静态方法判断字符是Digit还是Letter或者其它。

`String`总存储在堆上，它实现了哪些接口，`IComparable`, `ICloneable`, `IConvertible`, `IComparable<String>`, `IEnumerable<char>`, `IEnumerable`, `IEquatable<String>`.

`StringBuilder`(Represents a mutable string of characters.)它属于密封类，继承`ISerializable`接口， 它的最大容量是`Int32.MaxValue`

## Enum(枚举) ##

属于强类型，直接从`System.Enum`派生，枚举类型不能定义任何方法属性或事件，但可以通过扩展方法来实现。

枚举遍历及内部元素获取，`String <-> Enum`相互转换, Enum 元素判断

``` csharp
///
/// Contains System.Enum and System.Type methods
///
//getValues
public static Array GetValues(Type enumType);
public Array GetEnumValues();

//getNames
public static string GetName(Type enumType,Object value);
public string GetEnumName(Object value);

public static string[] GetNames(Type enumType);
public string[] GetEnumNames();

//parse Tryparse
public static Object Parse(Type enumType,string value);
public static Object Parse(Type enumType,string value, boolean ignoreCase);

public static boolean TryParse<TEnum>(string value, out TEnum result) where TEnum : struct;
public static boolean TryParse<TEnum>(string value, boolean ignoreCase, out TEnum result) where TEnum : struct;

//check enum element
public static boolean IsDefined(Type enumType, Object value);
public boolean IsEnumDefined(Object value);
```

## 可空值类型(Nullable) ##

看下面这个具体实现：

``` csharp
[Serializable, StructLayout(LayoutKind.Sequential)]
public struct Nullable<T> where T : struct {
    
    private boolean hasValue = false;
    internal T value = default(T);

    public Nullable(T value) {
        this.value = value;
        this.hasValue = true;
    }

    public boolean HasValue { get { return hasValue; } }

    public T Value {
        get {
            if (!hasValue) {
                throw new InvalidOperationException("Nullable object must have a value.");
            }
            return value;
        }
    }

    public T GetValueOrDefault() { return value; }

    public override boolean Equals(Object other) {
        if (!HasValue) return (other == null);
        if (other == null) return false;
        return value.Equals(other);
    }

    public override int GetHashCode() {
        if (!HasValue) return 0;
        return value.GetHashCode();
    }

    public override string ToString() {
        if (!HasValue) return string.Empty;
        return value.ToString();
    }

    public static implicit operator Nullable<T>(T value) {
        return new Nullable<T>(value);
    }
}
```

**PS.** 
* 一元操作符：`+, ++, -, --, !, ~`, 操作数为null, 结果就是null
* 二元操作符：`+, -, *, /, %, &, |, ^, <<, >>`, 两个操作数任何一个是null, 结果就是null

可空值类型的装箱与拆箱。

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