---
title: get和post请求
date: 2018-10-10 18:52:53
tags: http
categories: 前端
---
在HTTP中，与服务器交互的方法，最常用的有4种：即Put（增）、Delete（删）、Post（改）、Get（查）。
get是获取数据，post是修改数据。
<!-- more -->
##### get 和 post 的区别

get把请求的数据放在url上，即HTTP协议头上，其格式为： 
    以?分割URL和传输数据，参数之间以&相连。 
    数据如果是英文字母/数字，原样发送， 
    如果是空格，转换为+， 
    如果是中文/其他字符，则直接把字符串用BASE64加密，及“%”加上“字符串的16进制ASCII码”。 
post把数据放在HTTP的包体内（requrest body）。

get提交的数据最大是2k（原则上url长度无限制，那么get提交的数据也没有限制咯？限制实际上取决于浏览器，(大多数)浏览器通常都会限制url长度在2K个字节，即使(大多数)服务器最多处理64K大小的url。也没有卵用。）。 
post理论上没有限制。实际上IIS4中最大量为80KB，IIS5中为100KB。

GET产生一个TCP数据包，浏览器会把http header和data一并发送出去，服务器响应200(返回数据); 
POST产生两个TCP数据包，浏览器先发送header，服务器响应100 continue，浏览器再发送data，服务器响应200 ok(返回数据)。

GET在浏览器回退时是无害的，POST会再次提交请求。 

GET产生的URL地址可以被Bookmark，而POST不可以。 

GET请求会被浏览器主动cache，而POST不会，除非手动设置。 

GET请求只能进行url编码，而POST支持多种编码方式。 

GET请求参数会被完整保留在浏览器历史记录里，而POST中的参数不会被保留。 

GET只接受ASCII字符的参数的数据类型，而POST没有限制

那么，post那么好为什么还用get？get效率高！。

![get post](/assets/images/get-post.png)
##### 其他 HTTP 请求方法
![get post](/assets/images/http.png)

> 
> 参考文章 [HTTP 方法：GET 对比 POST](http://www.w3school.com.cn/tags/html_ref_httpmethods.asp) 、[get和post的区别--面试经常被问到！](https://blog.csdn.net/qq_26360877/article/details/70665820)
> 





