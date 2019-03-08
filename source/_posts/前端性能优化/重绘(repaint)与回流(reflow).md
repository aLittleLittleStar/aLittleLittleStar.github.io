---
title: 重绘(repaint)与回流(reflow)
date: 2019-2-14 15:07:34
tags: 前端性能优化
categories: 前端优化
---
<!-- toc -->

文章转载： 
　[浏览器重绘和重排](http://www.cnblogs.com/blackmanba/p/browser-repaint-reflow.html)
文章参考：
　[重构与回流](https://www.cnblogs.com/jiayuexuan/p/7490140.html)
　[浏览器的重绘与重排](https://kb.cnblogs.com/page/169820/)
　[探讨css中repaint和reflow](http://www.cnblogs.com/shenqi0920/p/3545820.html)
##### __前言：__
　　页面设计中，不可避免的需要浏览器进行repaint和reflow。那到底什么是repaint和reflow呢。下面谈谈自己对repaint和reflow的理解，以及结合其他技术牛的讲解，谈谈如何优化repaint和reflow。
##### __概述：__
　　**重排(回流)**, 顾名思义就是重新排版的意思; **重绘**, 就是浏览器重新绘制。理解重排和重绘的含义十分重要, 因为在评审页面交互效果的时候, 重绘和重排是必须考虑的因素。并不是说交互效果实现了就可以了, 必须同时考虑到这样做会引发什么性能问题。也就是说, 浏览器在进行重绘和重排的时候是要付出高昂的性能代价的。
　　只有静态页面才会不存在repaint和reflow。repaint主要是针对某一个DOM元素进行的重绘，reflow则是回流，针对整个页面的重排。字面意思来说：repaint就是重绘，reflow就是回流。**回流必将引起重绘，而重绘不一定会引起回流**，repaint和reflow的目的是：展示一个新的页面样貌。<!-- more -->
##### __浏览器执行流：__
　　浏览器每次从服务器下载完页面后就会对页面进行渲染(Render), 这里面就包含了重绘以及重排。每种浏览器虽然工作原理略有差别, 但也遵循以下流程:
　　览器引擎会解析HTML文档来构建DOM树。树的每个节点都是标签, 有大小边距等等的属性, 这是因为每个HTML元素都遵循**盒子模型**(隐藏元素不包括在文档树中, 浏览器不会将其渲染)。
　　渲染树构建完毕后, 浏览器就能够确定每个元素的位置并将元素放到正确的位置上, 再根据***树节点的样式属性***绘制出页面元素。
　　由于浏览器的流布局的方式, 对渲染树的计算通常只需要遍历一遍即可。但table及其内部元素除外, 可能需要执行多次计算才能确定好在渲染树中的属性,这个过程通常要耗费3倍以上的时间。
　　这也是我们要避免使用table标签的其中一个原因。
　　简言之浏览器执行顺序为：
  　　1. 首先获取html，然后构建dom树 ，DOM树里包含了所有HTML标签，包括display:none隐藏，还有用JS动态添加的元素等。
  　　2. 浏览器把所有样式(用户定义的CSS和用户代理)解析成样式结构体，在解析的过程中会去掉浏览器不能识别的样式，比如IE会去掉-moz开头的样式，而FF会去掉_开头的样式。
  　　3. DOM Tree 和样式结构体组合后构建render tree, render tree类似于DOM tree，但区别很大，render tree不包含隐藏的节点 (比如display:none的节点，还有head节点)，因为这些节点不会用于呈现。
  　　4. 一旦render tree构建完毕后，浏览器就可以根据render tree来绘制页面了。
　　
##### __严重性：__
　在性能优先的前提下，性能消耗 reflow大于repaint。
##### __体现：__
　repaint是某个DOM元素进行重绘；reflow是整个页面进行重排，也就是页面所有DOM元素渲染。
##### __如何触发：__
　style变动造成repaint和reflow。
　不涉及任何DOM元素的排版问题的变动为repaint，例如元素的color/text-align/text-decoration等等属性的变动。
　除上面所提到的DOM元素style的修改基本为reflow。例如元素的任何涉及长、宽、行高、边框、display等style的修改。
##### __常见触发场景：__
　　重绘是一个元素外观的改变所触发的浏览器行为，例如改变visibility、outline、背景色等属性。浏览器会根据元素的新属性重新绘制，使元素呈现新的外观。重绘不会带来重新布局，并不一定伴随重排。重排(回流)是更明显的一种改变，可以理解为渲染树需要重新计算。
1. 触发repaint：
  1. color的修改，如color=#ddd；
  2. text-align的修改，如text-align=center；
  3. a:hover也会造成重绘。
  4. :hover引起的颜色等不导致页面回流的style变动。
2. 触发reflow：
  1. width/height/border/margin/padding的修改，如width=778px；
  2. 动画，:hover等伪类引起的元素表现改动，display=none等造成页面回流；
  3. appendChild等DOM元素操作；
  4. font类style的修改；
  5. background的修改，注意着字面上可能以为是重绘，但是浏览器确实回流了，经过浏览器厂家的优化，部分background的修改只触发repaint，当然IE不用考虑；
  6. scroll页面，这个不可避免；
  7. resize页面，桌面版本的进行浏览器大小的缩放，移动端的话，还没玩过能拖动程序，resize程序窗口大小的多窗口操作系统。
  8. 读取元素的属性(这个无法理解，但是技术达人是这么说的，那就把它当做定理吧)：读取元素的某些属性(offsetLeft、offsetTop、offsetHeight、offsetWidth、scrollTop/Left/Width/Height、clientTop/Left/Width/Height、getComputedStyle()、currentStyle(in IE))

__如何避免：__
　　说避免那是不可能的，不然就是以前古老的静态页面了，没有交互，那在现在看来，就是一个失败的作品。所以，在我们进行网页设计的时候，就必须尽量减少页面的repaint和reflow。repaint和reflow的目的是为了展示一个新的页面，那么我们在进行页面交互时，尽量通过各种方法减少repaint和reflow但又能展示一个新的页面的目的。所以下面将结合其他技术达人的建议，通过自己的理解，给大家讲解如何避免和优化repaint和reflow：
  1. 尽可能在DOM末梢通过改变class来修改元素的style属性： 将多次改变样式属性的操作合并成一次操作，尽可能的减少受影响的DOM元素。
  2. 避免设置多项内联样式：使用常用的class的方式进行设置样式，以避免设置样式时访问DOM的低效率。
  3. 设置动画元素position属性为fixed或者absolute：由于当前元素从DOM流中独立出来，因此受影响的只有当前元素，元素repaint。
  4. 牺牲平滑度满足性能：动画精度太强，会造成更多次的repaint/reflow，牺牲精度，能满足性能的损耗，获取性能和平滑度的平衡。
  5. 避免使用table进行布局：table的每个元素的大小以及内容的改动，都会导致整个table进行重新计算，造成大幅度的repaint或者reflow。改用div则可以进行针对性的repaint和避免不必要的reflow。
  6. 避免在CSS中使用运算式：学习CSS的时候就知道，这个应该避免，不应该加深到这一层再去了解，因为这个的后果确实非常严重，一旦存在动画性的repaint/reflow，那么每一帧动画都会进行计算，性能消耗不容小觑。
