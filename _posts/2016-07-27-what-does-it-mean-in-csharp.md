---
date: 2016-07-27 09:26:31+00:00
layout: post
title: class A <T> where T:new()是什么意思
categories: C#
tags: C#
---


这是C#泛型类声明的语法，
class A<T> 表示 A类接受某一种类型，泛型类型为T，需要运行时传入。
where表明了对类型变量T的约束关系。where T：new()指明了创建T的实例时应该具有构造函数。一般情况下，无法创建一个泛型类型参数的实例。然而，new()约束改变了这种情况，要求类型参数必须提供一个无参数的构造函数。
