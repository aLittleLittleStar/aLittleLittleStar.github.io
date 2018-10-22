---
title: cookit和session
date: 2018-10-10 17:40:59
tags: 
  cookit
  session
  面试题
categories: 前端
---

#### cookie
> __cookie机制采用的是在客户端保持状态的方案__
> Cookie的主要内容包括：名字，值，过期时间，路径和域。使用Fiddler抓包就可以看见.
> 
> 
> 
#### session
> __session机制采用的是在服务器端保持状态的方案__
> 存在服务器的一种用来存放用户数据的类HashTable结构。
> 浏览器第一次发送请求时，服务器自动生成了一HashTable和一SessionID来唯一标识这个HashTable，并将其通过响应发送到浏览器。浏览器第二次发送请求会将前一次服务器响应的
> 
> Session ID放在请求中一并发送到服务器上，服务器从请求中提取出Session ID，并和保存的所有Session ID进行对比，找到这个用户对应的HashTable。
> 

#### cookie 和 session 的区别
+ cookie数据存放在客户的浏览器上，session数据放在服务器上。
+ session 中保存的是对象，cookie 中保存的是字符串。
+ session 不能区分路径，同一个用户在访问一个网站期间，所有的session在任何地方都可以访问到。而 cookie 中如果设置了路径参数，那么同一个网站不同路径下的 cookie 互相是不可以访问的。
+ cookie不是很安全，别人可以分析存放在本地的COOKIE并进行COOKIE欺骗,考虑到安全应当使用session。
+ session会在一定时间内保存在服务器上。当访问增多，会比较占用你服务器的性能考虑到减轻服务器性能方面，应当使用cookie。
+ 单个cookie保存的数据不能超过4K，很多浏览器都限制一个站点最多保存20个cookie。


#### cookie 和 webStorage 的区别
+ webStorage 的优势
  + 从容量上讲WebStorage一般浏览器提供5M的存储空间。
  + 安全性上WebStorage 并不作为 HTTP header 发送的浏览器，所以相对安全。
  + 从流量上讲，因为WebStorage不传送到服务器，所以不必要的流量可以节省。 Cookie和webstorage区别 
  + 数据的有效期不同
    + Webstorage:1.localstorage 2.sessionstorage 
    + sessionStorage：仅在当前的浏览器窗口关闭有效；
    + localStorage：始终有效，窗口或浏览器关闭也一直保存，因此用作持久数据,除非手动删除；
    + cookie：只在设置的cookie过期时间之前一直有效，即使窗口和浏览器关闭
  + 作用域不同
    + sessionStorage：不在不同的浏览器窗口中共享，即使是同一个页面；
    + localStorage：在所有同源窗口都是共享的；
    +  cookie：也是在所有同源窗口中共享的
  + webStorage支持事件通知机制，可以将数据更新的通知发生给监听者
  + ![WebStorage](/assets/images/session.png)

#### 如果 cookie 被篡改了怎么办？
> 预防 Cookie 被篡改
> set-cookie时加上防篡改验证码。 
> 


> 
> 参考文章 [cookie和session, cookie和webStorage的区别](https://blog.csdn.net/statham_li/article/details/80226447)
> 
> 
> 