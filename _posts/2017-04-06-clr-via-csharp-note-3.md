---
date: 2017-04-07 15:21:31+00:00
layout: post
title: CLR via C#学习笔记3
categories: 文档
tags: 笔记 
---

[TOC]
# CLR via C#学习笔记3
## 事件
如果定义一个事件成员，意味着类型要提供一下能力。
- 方法可登记它对该事件的关注。
- 方法可注销它对该事件的关注。
- 该事件发生时，等急了的方法会收到通知。

CLR的事件模型建立在**委托**的基础上。委托是调用（唤出）回调方法的一种类型安全的方式。

### 设计要公开事件的类型
#### 第一步：定义类型来容纳所有需要发送给事件通知接收者的附加信息
```csharp
//第一步：定义一个类型来容纳所有应该发送给事件通知接受者的附加信息
internal class NewMailEventArgs : EventArgs{
    private readonly String m_from, m_to, m_subject;
    
    public NewMailEventArgs(String from, String to, String subject){
        m_from = from; m_to = to; m_subject = subject;
    }
    
    public String From { get { return m_from;} }
    public String To { get { return m_to; } }
    public String Subject { get { return m_subject; } }
}

//后续的步骤将在MailManager类中进行
internal class MailManager{}
```
#### 第二步：定义事件成员
```csharp
internal class MailManager{
    //第二步：定义事件成员
    public event EventHandler<NewMailEventArgs> NewMail;
}
```
泛型System.EventHandler委托类型的定义如下：
```csharp
public delegate void EventHandler<TEventArgs>(Object sender, TEventArgs e)where TEventArgs: EventArgs;
```
方法原型必须具有以下形式：
```csharp
void MethodName(Object sender, NewMailEventArgs e);
```
#### 第三步：定义负责引发事件的方法来通知事件的登记对象
#### 第四部：定义方法将输入转换为期望事件
### 编译器如何实现事件

## 泛型
泛型是CLR和编程语言提供的一种特殊机制，它支持“算法重用”。  
CLR允许创建泛型引用类型和泛型值类型，但不允许创建泛型枚举类型。允许创建泛型接口和泛型委托。CLR允许在引用类型、值类型或接口中定义泛型方法。  
封装了泛型列表算法的FCL类称为List<T>。  
泛型List类的设计者紧接在类名后添加了一个<T>，表明它操作的是一个未指定的数据类型。**类型参数**
>   根据Microsoft的设计原则，泛型参数变量要么称为T，要么至少以大写T开头（如TKey和TValue）。大写T代表类型（Type），就像大写I代表接口（Interface）一样，比如IComparable。

泛型为开发人员提供了一下优势：
- 源代码保护
- 类型安全
- 更加清晰的代码
- 更佳的性能：不需要再执行任何装箱操作。

#### 开放类型和封闭类型
CLR如何为应用程序使用的每个类型创建一个内部数据结构，这种数据结构称为类型对象。
具有泛型类型参数的类型称为开放类型，CLR禁止构造开放类型的任何实例。这一点类似于CLR禁止构造接口类型的实例。  
假如为所有类型参数传递的都是实际数据类型，类型就称为封闭类型。  
**约束**
#### 泛型类型和继承
#### 泛型类型同一性
有时候，泛型语法会将开发人员弄糊涂，因为源代码中可能散布着大量“<”和“>”符号，这会损害可读性。简化代码例子：
```csharp
List<DateTime> dt = new List<DateTime>();
```
首先定义
```csharp
internal sealed class DateTimeList:List<DateTime>{
    //这里无需放入任何代码
}
```
结果
```csharp
DateTimeList dt = new DateTimeList();
```
这样做表面上是方便了，但是绝对不要单纯处于增强源代码易读性的目的来定义一个新类。这样会丧失类型同一性和相等性。

另一种简化的方式引用一个泛型封闭类型，同时不糊影响类型的相等性--using。
```csharp
using DateTimeList = System.Collections.Generic.List<System.DateTime>;
```
#### 代码爆炸
他可能造成应用程序的工作集显著增大，从而损害性能。

### 泛型接口
```csharp
public interface IEnumerator<T> : IDisposable, IEnumerator{
    T Current { get; }
}
```
### 泛型委托
```csharp
public delegate TReturn CallMe<TReturn, TKey, TValue>{TKey key, TValue value};
```
### 可验证性和约束
```csharp
public static T Min<T>(T o1, T o2) where T : IComparable<T> {
    if (o1.CompareTo{o2} < 0) return o1;
    return o2;
}
```
C#的where关键字告诉编译器，为T指定的任何类型都必须实现同类型（T）的泛型IComparable接口
#### 主要约束
主要约束可以是一个引用类型，它标识了一个没有密封的类。  
约束不能是特殊类“object”。  
有两个特殊的主要约束：class和struct  
所有值类型都隐式地有一个公共无参构造器。
#### 次要约束
一个类型参数可以指定零个或者多个次要约束，次要约束代表的是一个接口类型。
#### 构造器约束
```csharp
internal sealed class ConstructorConstraint<T> : new() {
    public static T Factory(){
        //允许，因为所有值类型都隐式有一个公共无参构造器
        //而约束要求定义的任何引用类型也要有一个公共无参构造器
        return new T();
    }
}
```
#### 其他可验证性问题
1. 泛型类型变量的转型
    - 将一个泛型类型的变量转型为另一个类型是非法的，除非将其转型为与一个约束兼容的类型。
2. 将一个泛型类型变量设为默认值
    - 将泛型类型变量设为null是非法的，除非将泛型类型约束成一个引用类型。未对T进行约束，所以它可能是一个值类型，而将值类型的一个变量设为null是不可能的。
    - 允许将一个变量设为一个默认值。使用**default**关键字。  
    ```csharp
    private static void SettingAGenericTypeVariableToDefaultValue<T>(){
        T temp = default(T);
    }
    ```
    default关键字告诉C#编译器和CLR的JIT编译器，如果T是一个引用类型，就将temp设为null；如果T是一个值类型，就将temp的所有位设为0。
3. 将一个泛型类型变量与null进行比较
    -但是对于值类型来说，obj永远都不会为null。
4. 两个泛型类型变量相互比较
5. 泛型类型变量作为操作数使用


## 接口
CLR不支持多继承，只是通过接口提供了“缩水版”的多继承。  
本章将讨论如何定义和使用一个接口，还要提供一些指导性原则，帮助你判断何时应该使用接口而不是基类。
### 类和接口继承
在CLR中，任何类都肯定是从一个类派生的。这个类称为基类。  
CLR还允许开发人员定义接口，它实际只是对一组方法签名进行了统一命名。  
类继承的一个重要特点：凡是能使用基类型实例的地方，都能使用派生类型的实例。  
接口继承的一个重要特地啊：凡是能使用具名接口类型的实例的地方，都能使用实现了接口的一个类型的实例。
### 定义接口
C#禁止接口定义任何一种这样的静态成员。  
对CLR而言，接口定义就像是一个类型定义。也就是说，CLR会为接口类型对象定义一个内部数据结构，同时可用反射机制来查询接口类型的功能。
### 继承接口
C#编译器要求将用于实现一个接口的方法标记为public。CLR要求将接口方法标记为virtual。
### 关于调用接口方法的更多探讨
### 隐式和显式接口方法实现（幕后发生的事情）
### 泛型接口
### 泛型和接口约束
### 实现多个具有相同方法名和签名的接口
### 用显式接口方法实现来增强编译时类型安全性
### 谨慎使用显示接口方法实现
### 设计：基类还是接口？
- IS-A vs. CAN-DO 关系
- 易于使用
- 一致性的实现：不管一个接口的契约文档做得有多好，都无法保证任何人都能百分之百正确地实现它。
- 版本控制：向结类型添加一个方法，派生类型将继承新方法。向接口添加一个新成员，会强迫接口的继承者更改其源代码并重新编译。
