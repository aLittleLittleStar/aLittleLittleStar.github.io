---
title: MVVM开发模式的理解
date: 2019-2-23 14:28:24
tags: MVVM
categories: 开发模式
---
<!-- 目录 -->
<!-- toc -->

### MVVM 
　　MVVM（模型视图ViewModel 是一种基于MVC和MVP的架构模式，<font color=FF0000 >它试图更清楚地将用户界面（UI）的开发与应用程序中的业务逻辑和行为的开发分开</font>。为此，此模式的许多实现都使用声明性数据绑定，以允许将视图上的工作与其他层分离。<!--  more-->
#### Model、View、ViewModel
　　MVVM分为Model、View、ViewModel三者。
+ Model 代表数据模型，数据和业务逻辑都在Model层中定义；
+ View 代表UI视图，负责数据的展示，是用户在屏幕上看到的结构；
+ ViewModel 负责监听 Model 中数据的改变并且控制视图的更新，处理用户交互操作；
　　Model 和 View 并无直接关联，而是通过 ViewModel 来进行联系的，Model 和 ViewModel 之间有着双向数据绑定的联系。因此当 Model 中的数据改变时会触发 View 层的刷新，View 中由于用户交互操作而改变的数据也会在 Model 中同步。
这种模式实现了 Model 和 View 的数据自动同步，因此开发者只需要专注对数据的维护操作即可，而不需要自己操作 dom。
![mvvm](/assets/images/mvvm2.png)
![mvvm](/assets/images/mvvm.png)

### 设计模式编辑
　　因为WPF技术出现，从而使MVC架构模式有所改进，MVVM 模式便是使用的是数据绑定基础架构。它们可以轻松构建UI的必要元素。
可以参考The Composite Application Guidance for WPF(prism)
View绑定到ViewModel，然后执行一些命令在向它请求一个动作。而反过来，ViewModel跟Model通讯，告诉它更新来响应UI。这样便使得为应用构建UI非常的容易。往一个应用程序上贴一个界面越容易，外观设计师就越容易使用Blend来创建一个漂亮的界面。同时，当UI和功能越来越松耦合的时候，功能的可测试性就越来越强。
在MVP模式中，为了让UI层能够从逻辑层上分离下来，设计师们在UI层与逻辑层之间加了一层interface。无论是UI开发人员还是数据开发人员，都要尊重这个契约、按照它进行设计和开发。这样，理想状态下无论是Web UI还是Window UI就都可以使用同一套数据逻辑了。借鉴MVP的IView层，养成习惯。View Model听起来比Presenter要贴切得多；会把一些跟事件、命令相关的东西放在MVC的'C',或者是MVVM的'Vm'。


### 优点和缺点
#### 优点
　　MVVM模式和MVC模式一样，主要目的是<font color=#FF0000 >分离视图（View）</font>和<font color=#FF0000 >模型（Model）</font>，有几大优点:
1. **低耦合**: __视图（View）__可以独立于 __Model__ 变化和修改，一个ViewModel可以绑定到不同的"View"上，当View变化的时候Model可以不变，当Model变化的时候View也可以不变。
2. **可重用性**: 你可以把一些视图逻辑放在一个ViewModel里面，让很多view重用这段视图逻辑。
3. **独立开发**:开发人员可以专注于业务逻辑和数据的开发(ViewModel), 设计人员可以专注于页面设计，使用Expression Blend可以很容易设计界面并生成xaml代码。
4. **可测试**:界面素来是比较难于测试的，而现在测试可以针对ViewModel来写。
5. MVVM有助于更轻松地并行开发UI以及为其提供支持的构建块
6. 抽象视图，从而减少其背后的代码所需的业务逻辑（或粘合剂）的数量
7. ViewModel比事件驱动的代码更容易进行单元测试
8. 可以在不关心UI自动化和交互的情况下测试ViewModel（比View更多的模型）

#### 缺点
1. 对于更简单的UI，MVVM可能过度
2. 虽然数据绑定可以是声明性的并且很好用，但它们比我们简单设置断点的命令式代码更难调试
3. 非平凡应用程序中的数据绑定可以创建大量的簿记。您也不希望在绑定比绑定的对象更重的情况下结束
4. 在较大的应用程序中，预先设计ViewModel以获得必要的泛化量可能更加困难

参考文章：
　　[前后端分手大师——MVVM 模式](https://www.cnblogs.com/iovec/p/7840228.html)
　　[简单理解MVVM--实现Vue的MVVM模式](https://zhuanlan.zhihu.com/p/38296857)
　　[了解MVVM - JavaScript开发人员指南](https://addyosmani.com/blog/understanding-mvvm-a-guide-for-javascript-developers/)