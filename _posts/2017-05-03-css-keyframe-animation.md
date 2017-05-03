---
date: 2017-05-03 18:08:31+00:00
layout: post
title: css帧动画
categories: 文档
tags: 问题 css
---

属性型指令

```stylesheet
//animations
@keyframes hover-card-shake {
  0% {
    -webkit-transform: rotate(0deg); -moz-transform: rotate(0deg); -ms-transform: rotate(0deg); -o-transform: rotate(0deg); transform: rotate(0deg);
  }
  30% {
    -webkit-transform: rotate(5deg); -moz-transform: rotate(5deg); -ms-transform: rotate(5deg); -o-transform: rotate(5deg); transform: rotate(5deg);
  }
  //50% {
  //  -webkit-transform: rotate(-0deg) scale(1.2); -moz-transform: rotate(-0deg) scale(1.2); -ms-transform: rotate(-0deg) scale(1.2); -o-transform: rotate(-0deg) scale(1.2); transform: rotate(-0deg) scale(1.2);
  //}
  80% {
    -webkit-transform: rotate(-5deg); -moz-transform: rotate(-5deg); -ms-transform: rotate(-5deg); -o-transform: rotate(-5deg); transform: rotate(-5deg);
  }
  100% {
    -webkit-transform: rotate(0deg); -moz-transform: rotate(0deg); -ms-transform: rotate(0deg); -o-transform: rotate(0deg); transform: rotate(0deg);
  }
}

//引用
@mixin card-hover-animation($color) {
  background-color: rgba($color, 0.1);
  animation: hover-card-shake 0.2s 1.2s 3 linear;
  -webkit-animation-delay: 0s; -moz-animation-delay: 0s; -o-animation-delay: 0s; animation-delay: 0s;
}
```



机智如我(*^__^*) 