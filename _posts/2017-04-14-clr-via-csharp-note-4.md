---
date: 2017-04-14 10:42:31+00:00
layout: post
title: CLR via C#学习笔记4
categories: 文档
tags: 笔记
---
[TOC]
# CLR via C#学习笔记4
##字符、字符串和文本处理
###字符
在.NET Framework中，字符总是表示成16位Unicode代码值。
System.Char(一个值结构)
两个公共制度常量字段：MinValue('\0')和MaxValue('\uffff')。  
三种技术实现各种数值类型与Char实例的相互转换。
- 转型（强制类型转换）。效率最高，因为编译器会生成中间语言（IL）指令来执行转换，不必调用任何方法。
- 使用Convert类型。所有方法都已checked方式执行转换，所以一旦发现转换会造成数据丢失，就会抛出一个OVerflowException异常。
- 使用IConvertible接口：效率最低。因为在值类型上调用一个接口方法，要求对实例进行装箱。
### System.String类型
一个String代表一个不可变的顺序字符集。
#### 逐字字符串
```csharp
String file = @"C:\Windows\System32\Notepad.exe";
```
#### 比较字符串
Equals Compare  StartsWith EndsWith
#### 字符串池
#### 高效率构造字符串
StringBuilder
#### 安全字符串
SecureString
## 枚举类型和位标识
###枚举类型
- 枚举类型是程序更容易编写、阅读和维护。
- 枚举类型是强类型的。  

枚举类型是值类型。
ToString方法，Format方法，可调用它们格式化一个枚举类型的值。
```csharp
internal enum Color:byte{
    White,
    Red,
    Green,
    Blue,
    Orange
}

Color c = Color.Blue;
Console.WriteLine(c);
Console.WriteLine(c.ToString());
Console.WriteLine(c.ToString("G"));//“Blue”常规格式
Console.WriteLine(c.ToString("D"));//"3"(十进制格式)
Console.WriteLine(c.ToString("X"));//“03”（十六进制格式）
```
Format有一个有事：允许为value参数传递一个数值。这昂就并非一定要有枚举类型的一个实例。
```csharp
Console.WriteLine(Enum.Format(typeof(Color),3,"G"));
```
### 位标识
Enum类定义了一个HasFlag方法，但建议避免使用HasFlag方法，理由：由于它获取Enum类型的一个参数，所以传给它的任何值都必须装箱，会产生一次内存分配。
### 向枚举类型添加方法
C#扩展方法功能向枚举类型模拟添加方法