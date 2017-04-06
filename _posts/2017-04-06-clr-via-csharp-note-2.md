---
date: 2017-04-06 16:11:31+00:00
layout: post
title: CLR via C#学习笔记2
categories: 文档
tags: 笔记 
---

[TOC]
# CLR via C#学习笔记2
## 参数
### 可选参数和命名参数
- 可以为方法、构造器方法和有参属性（C#索引器）的参数指定默认值。委托定义一部分的参数指定默认值。
- 有默认值的参数必须放在没有默认值的所有参数之后。
- 默认值必须是编译时能确定的常量值。default关键字、new关键字。
- 主要不要重命名参数变量。
- 如果方法是从模块外部调用的，更改参数的默认值具有潜在的危险性。可考虑将默认值0/null作为哨兵值使用，从而指出默认行为。
- 如果参数用ref或out关键字进行了标识，就不能设置默认值。
- 实参可按任何顺序传递；但是，命名实参只能出现在实参列表的尾部。
- 可按名称将实参传给没有默认值的参数。
- C#不允许省略逗号之间的实参。因为这会造成对可读性的影响。

### 隐式类型的局部变量
```csharp
var collection = new Dictionary<String, Single>(){ { ".NET", 4.0f } };
```
用var声明局部变量只是一种简化语法，它要求编译器根据一个表达式推断具体的数据类型。

### 以传引用的方式向方法传递参数 out ref
CLR允许以传引用而非传值的方式传递参数。在C#中，这是用关键字out或ref来做到的。这两个关键字都告诉C#编译器生成元数据来指明该参数是传引用的。  
从CLR的角度看，这两个关键字完全一致。这就是说，无论哪个关键字，都会生成相同的IL代码。元数据也几乎完全一致，只有bit除外。  
但是，C#编译器是将这两个关键字区别对待的。如果方法的参数用out来标记，表示不指望调用者在调用方法之前初始化好了对象。被调用的方法不能读取参数的值，而且在返回前必须向这个值写入。相反，如果方法的参数用ref来标记，调用者就必须在调用该方法前初始化参数的值，被调用的方法可以读取值以及/或者向值写入。
```csharp
public sealed class Program{
    public static void Main(){
        Int32 x;//x没有初始化
        GetVal(out x);//x不必初始化
        Console.WriteLine(x);//显示“10”。
    }
    private static void GetVal(out Int32 v){
        v = 10;//该方法必须初始化v
    }
}
```

***为大的值类型使用out，可提升代码的执行效率，因为它避免了在进行方法调用时复制值类型实例的字段。***  
以下代码（不会编译）演示了类型安全型是如何被破坏的：
```csharp
internal sealed class SomeType{
    public Int32 m_val;
}

public sealed class Program{
    public static void Main(){
        SomeType st;
        
        //以下代码将产生编译错误：
        //error CS1503: 参数“1”：无法从“out SomeType”转换为“out object”
        GetAnObject(out st);
        
        Console.WriteLine(st.m_val);
    }
    
    private static void GetAnObject(out Object o){
        o = new String('X',100);
    }
}
```
保障类型安全的问题，可用泛型修正这些方法。
```csharp
public static void Swap<T>(ref T a ,ref T b){
    T t = b;
    b = a;
    a = t;
}
```
### 向方法传递可变数量的参数
```csharp
static Int32 Add(params Int32[] values){
    Int32 sum = 0;
    if(values != null){
        for (Int32 x = 0; x < values.Length; x++){
            sum += values[x];
        }
    }
    return sum;
}
```
params关键字只能应用于方法签名中的最后一个参数。这个参数只能标识任意类型的一个一维数组。可为这个参数传递null值，或传递

#### 写一个方法来获取任意数量、任意类型的参数
```csharp
private static void DisplayTypes(params Object[] objects){}
```
### 参数和返回类型的指导原则
声明方法的参数类型时，应尽量指定最弱的类型，最好是接口而不是基类。例如，如果要写一个方法来处理一组数据项，最好使用接口（比如**IEnumerable<T>**）来声明方法的参数，而不要用强数据类型（比如List<T>）或者更强的接口类型（比如ICollection<T>或IList<T>）  
相反，一般最好是将方法的返回类型声明为最强的类型（以免受限于特定的类型）。
### 常量性
CLR没有提供对常量对象/实参的支持。
## 属性
如何使用“对象和集合初始化器”来初始化属性，如何用C#的匿名类型和System.Tuple类型将多个属性打包到一起。  
属性的唯一好处就是提供了简化的语法。和调用普通方法（非属性中的方法）相比，属性不仅不会提升代码的性能，还会妨碍对代码的理解。
### 无参属性
简称属性,可将属性想象成只能字段。每个属性都有一个名称和一个类型（不能为void），定义属性时，通常要同时指定get和set两个方法。
编译器在你指定的属性名之前附加get_或set_前缀，从而自动生成这些方法的名称。
```csharp
public sealed class Employee{
    private String m_Name;
    private Int32 m_Age;
    
    public String get_Name(){
        return m_Name;
    }
    
    public void set_Name(String value){
        m_Name = value;
    }
}
```
#### 指定实现的属性
```csharp
public  sealed class Employee{
    public String Name{ get; set; }
}
```
如果声明一个属性而不提供get/set方法的实现，C#会自动为你声明一个私有字段。
#### 对象和集合初始化器
函数的组合使用
#### 匿名类型
可以使用非常简洁的语法来声明一个不可变的元组类型。
```csharp
var ol = new { Name = "Polly" , Year = 1964 };

Console.WriteLine("Name={0},Year={1}",ol.Name, ol.Year);
```
第一行代码创建了一个匿名类型，没有在new关键字后制定类型名称。  

匿名类型经常与LINQ技术配合使用。可用LINQ执行查询，从而生出由一组对象构成的集合。
#### System.Tuple类型
传递一个元组。
```csharp
public class Tuple<T1>
    {
        private T1 m_Item1;
        public Tuple(T1 item1) { m_Item1 = item1; }
        public T1 Item1 { get { return m_Item1; } }
    }
```
```csharp
public class Tuple<T1, T2, T3, T4, T5, T6, T7, TRest>
    {
        private T1 m_Item1; private T2 m_Item2; private T3 m_Item3; private T4 m_Item4;
        private T5 m_Item5; private T6 m_Item6; private T7 m_Item7; private TRest m_Rest;
        public Tuple(T1 item1, T2 item2, T3 item3, T4 item4, T5 item5, T6 item6, T7 item7, TRest t)
        {
            m_Item1 = item1; m_Item2 = item2; m_Item3 = item3; m_Item4 = item4;
            m_Item5 = item5; m_Item6 = item6; m_Item7 = item7; m_Rest = t;
        }
        public T1 Item1 { get { return m_Item1; } }
        public T2 Item1 { get { return m_Item2; } }
        public T3 Item1 { get { return m_Item3; } }
        public T4 Item1 { get { return m_Item4; } }
        public T5 Item1 { get { return m_Item5; } }
        public T6 Item1 { get { return m_Item6; } }
        public T7 Item1 { get { return m_Item7; } }
        public TRest Item1 { get { return m_Rest; } }
    }
```
```csharp
private static Tuple<Int32, Int32> MinMax(Int32 a, Int32 b)
        {
            return new Tuple<Int32, Int32>(Math.Min(a, b), Math.Max(a, b));
        }
//调用
public IEnumerable<string> Get(DateTime dt = default(DateTime))
        {
            var collection = new Dictionary<String, Single>() { { ".NET", 4.0f }, { ".NET", 4.0f } };
            var minmax = MinMax(6, 2);
            Console.WriteLine("Min={0}, Max={1}", minmax.Item1, minmax.Item2);
            return new string[] { "value1", "value2" };
        }
```
### 有参属性
在C#中被称为索引器（indexer）
C#使用数组风格的语法来公开有参属性（索引器）。换句话说，可将索引器看作C#开发人员重载[]操作符的一种方式。
### 调用属性访问器方法时的性能
### 属性访问器的可访问性
### 泛型属性访问器方法