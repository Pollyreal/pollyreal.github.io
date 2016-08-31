---
date: 2016-08-31 15:24:31+00:00
layout: post
title: Markdown学习笔记与脱焚纪念
categories: 文档 花痴
tags: markdown
---

[TOC]

---
<span id="jump"></span>
#Markdown学习笔记
##怎么显示一条标题。  
　　显示了，但没显示全角空格。

* 标题和列表
* 组合起来
* 不就是目录吗
    - 所以目录怎么显示呢？
    	+ 哈哈哈哈
    - 哈哈哈
* en

## 一篇文章
　　这里是一篇文章。
> 哈哈，这句话是我说的
>> 我啊就是我。
>>> 我是很奇妙的引用quote？

>> en

> haole

##一段代码
``` csharp
        public static void Rotate(int[] nums, int k)
        {
            k %= nums.Length;
            int[] tmp = new int[k];
            Array.Copy(nums, nums.Length - k, tmp, 0, k);
            Array.Copy(nums, 0, nums, k, nums.Length - k);
            Array.Copy(tmp, 0, nums, 0, k);
        }
```
也可以加一句小的行内代码` int [] nums={1,2,3,4,5,6,7}; ` 就像这样。` Rotate(nums, 3);`这样。 
##一张图片
![图片没显示%>_<%](https://ss0.baidu.com/6ONWsjip0QIZ8tyhnq/it/u=3549104949,2425667904&fm=58 "是什么")
**能**看到*图片*吗 **粗体**和*斜体*

## 一张表格
##### 给表格一个title吧
|	一个 	|	两个	|	三个	|	四个|
|:------|------:|:------:|--------|
|	左对齐	|	右对齐	|	居中	|	默认居中	|
|[砰](#jump)|~~呵~~|~~框~~|~~啪~~|

## 那就来点链接
[PollyNg](http://pollyng.me "我啊行内链接")
[控制狂][1]
[控制狂是参考式链接][1]
[1]: http://y.qq.com/portal/song/000t1j9v3NmIOh.html "控制狂"
# 最后
## 砰砰分割线
--------------
**************
* * * * * * * *
![砰](http://pollyng.me/images/%E7%99%BE%E6%A5%BC.jpg "是什么")
