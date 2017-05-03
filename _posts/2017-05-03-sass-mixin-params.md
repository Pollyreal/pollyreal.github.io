---
date: 2017-05-03 18:08:31+00:00
layout: post
title: SASS带参数的mixin
categories: 文档
tags: 问题 sass
---

属性型指令

```stylesheet
//声明
@mixin card-hover-animation($color) {
  background-color: rgba($color, 0.1);
  animation: hover-card-shake 0.2s 1.2s 3 linear;
  -webkit-animation-delay: 0s; -moz-animation-delay: 0s; -o-animation-delay: 0s; animation-delay: 0s;
}
//声明
@mixin card-order($color) {
  padding: $cardPadding;
  box-sizing: border-box;
  margin: 0 2rem 1rem 0;
  min-height: 2rem;
  border: $outerBorder solid {
    radius: 0.6rem;
    color: $color;
  }
  &:hover {
  	//引用
    @include card-hover-animation($color)
  }
}
```



机智如我(*^__^*) 