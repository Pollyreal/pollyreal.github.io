---
date: 2017-05-11 6:52:31+00:00
layout: post
title: 《Angular权威教程》笔记
categories: 文档
tags: Angular 前端
---

[TOC]

### 组件的host配置项（与Angular4有区别）
```javascript
  @Component{
    selector: '',
    inputs: [''],
    host: {'class': 'item'},
    template: ``
  }
```
host配置项很有用，可以在组建内部配置宿主元素。例子为给宿主元素添加class为item的CSS类。  

备注：在Angular4中，用 @HostListener @HostBindings @Input @Output 替换了。


### 直接在表达式写三元表达式
`html
<span>{{ i < (product.department.length-1) ? '>' : ''}}</span>
`  
表示只要不是最后一级分类，就显示一个'>'号，否则显示一个空字符串。


### 数据架构管理方面的两个主要流派
1. 使用基于观察者模式的架构，如RxJS；
2. 使用基于Flux的架构。

### 内置指令
ngIf ngSwitch ngStyle ngClass ngFor ngNonBindable
