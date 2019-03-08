---
title: bind、apply和call的区别
date: 2018-10-10 18:21:49
tags: JS
categories: 前端
---
ing...
bind和call的作用都是将某个函数的this指向绑定到另外一个作用域中，
他们的参数调用都是相同的，第一个参数为绑定的作用域对象是什么，接下
来就是可以添加不限的参数，而区别就是call在绑定的同时调用函数，
bind是返回一个改变this指向的函数。
apply方法和call方法有些相似，它也可以改变this的指向
同样apply也可以有多个参数，但是不同的是，第二个参数必须是一个数组
