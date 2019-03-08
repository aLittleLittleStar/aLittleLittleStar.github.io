---
title: CSS布局
date: 2019-2-15 16:27:32
tags: 页面布局
categories: 前端
---
转载：
　　知乎专栏：[CSS布局十八般武艺都在这里了](https://zhuanlan.zhihu.com/p/25565751)
<!-- toc -->
#### 单列布局
##### 水平居中
###### 使用inline-block 和 text-align实现
```bash
.panrent {
  text-align: center;
}
.child {
  display: inline-block;
}
```
优点：兼容性好；不足：需要同时设置子元素和父元素

###### 使用margin:0 auto来实现
```bash
.child2 {
  width: 200px;
  margin: 0 auto;
  
}
```
优点：兼容性好缺点: 需要指定宽度

###### 使用table实现
```bash
.child2 {
  display: table;
  margin: 0 auto;
}
```
优点:只需要对自身进行设置不足:IE6,7需要调整结构

###### 使用绝对定位实现
```bash
.parent{
  position:relative;
}
# 或者实用margin-left的负值为盒子宽度的一半也可以实现，不过这样就必须知道盒子的宽度，但兼容性好
.child{
  position: absolute;
  left: 50%;
  transform: translate(-50%);
}
```
不足：兼容性差,IE9及以上可用

###### 实用flex布局实现
```bash
# 第一种方法
.parent{
  display:flex;
  justify-content:center;
}
# 第二种方法
.parent{
  display:flex;
}
.child{
  margin:0 auto;
}
```


#### 常用居中方法
　　居中在布局中很常见，我们假设DOM文档结构如下，子元素要在父元素中居中：
##### 水平居中


##### 垂直居中
　　
#### 单列布局
　　
#### 二列&三列布局
　　
##### float + margin
　　
##### position + margin
　　
##### 圣杯布局 (float + 负margin)
　　
##### 双飞翼布局 (float + 负margin)
　　
##### flex布局
　　
#### 总结　　